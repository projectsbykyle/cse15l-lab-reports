# Lab Report - Week 3

## Part 1
In this part of the lab report, we showcase a **basic search engine** built using **Java**. 

The code is shown here:

```
import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;

class Handler implements URLHandler{
    ArrayList<String> list = new ArrayList<String>(); 
    public String handleRequest(URI url){
        final String path = url.getPath(); 
        if(path.equals("/")){
            return "Welcome"; 
        }
        else{
            if(path.contains("/search")){
                final String[] query = url.getQuery().split("=");
                if(query[0].equals("s")){
                    final String input = query[1]; 
                    ArrayList<String> arrList = new ArrayList<>(); 
                    for(String str : list){
                        if(str.contains(input)) arrList.add(str); 
                    }
                    if(arrList.size() == 0){
                        list.add(query[1]);
                        return query[1]; 
                    }
                    else{
                        String s = arrList.toString();
                        return arrList.toString(); 
                    }
                }
            }
            return "Error: invalid path name";
        }
        
    }
}

class SearchEngine{
    public static void main(String[] args) throws IOException{
        final int port = 5001; 
        Server.start(port, new Handler()); 
    }
}
```
Here, we can search for strings. If no occurrences of an input are not found on the list on the website, then they are **added** to the list. Otherwise, we print **all occurrencess** of the string to the website. 

Here are some example searches for **apple** and **pineapple**. 

![Image](https://projectsbykyle.github.io/cse15l-lab-reports/AddApple.png)

When we first add **apple**, the program enter the **handleRequest** method. The path we entered, **localhost:5001/search?s=apple**, contains **/search**, so we enter the **if statement**. The program then enters the second **if statement** because our path contains a valid **query**. 

Here, the program then check if the **instance variable** **list** contains the search term **apple**. It doesn't, so it is added to the **list** and return the string **apple**. 

![Image](https://projectsbykyle.github.io/cse15l-lab-reports/AddPineapple.png)

The process above is repeated for **pineapple**. 

Now, we query for the word **app**. 
![Image](https://projectsbykyle.github.io/cse15l-lab-reports/QueryForApp.png)

Again, the program enters the **handleRequest** method and then the **if statement** handling valid **queries**. Upon checking for occurrences of the string **app**, the program finds that **apple** and **pineapple** contain it, and they are added to **arrList**, which is returned and displayed on the website. 

At the end of each of these processes, a new element is added to the end of **list**. If it is a new element that was not found as a string or substring in **list**, it is returned to be displayed. Otherwise, all occurrences of the string are returned as a new list to be displayed on the website. 


## Part 2
In this part of the lab report, we **identify** and **solve** two different **bugs** across two **algorithms**. 

For the first one, we discuss a bug related to reversal of array elements. 

In short, for a given input array, our program must produce an output array that contains strictly the elements of the input but in reverse order. 

```
arr[] = [1, 2, 3, 4, 5]
reverse(arr)
output(arr)
=====================================================
OUTPUT: [5, 4, 3, 2, 1]
```

As we can see, the two arrays are reversed permutations of one another: no extra elements are added or removed. 

A faulty algorithm attempting to implement this is shown below. 

```
void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
}
```
To look at the issues that this algorithm may have, we can try using different inputs and seeing if it matches our expected output. 

For the first test, we can try an empty array and see what happens. 
```
int arr[] = []; //expected output -> []
reverseInPlace(arr);
output(arr); 
=====================================================
OUTPUT: []
```
Then, we can try an array of size 1. 

```
int arr[] = [1]; //expected output -> [1]
reverseInPlace(arr);
output(arr); 
=====================================================
OUTPUT: [1]
```
Now, we can move onto arrays of larger sizes to see if this algorithm works as intended for input sizes greater than 1. 

```
int arr[] = [1, 2, 3, 4, 5]; //expected output -> [5, 4, 3, 2, 1]
reverseInPlace(arr);
output(arr); 
=====================================================
OUTPUT: [5, 4, 3, 4, 5] 
```
This pattern is repeated for all input arrays whose sizes are above one. Clearly, the algorithm is not performing its job correctly. Only half of the array is reversed, while the other half is unchanged. Now that we have this symptom, we can begin examining our code to see what went wrong. 

Below is the faulty algorithm:
```
void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
}
```
At first glance, we can already tell that the algorithm does not traverse the array correctly. To reverse an array of N elements, we must process **N/2 pairs of items** (we are swapping elements **N/2 times**). This means that we only need to traverse to the midpoint of the array, as items are processed in **pairs**. 

However, the algorithm traverses from index 0 to the last element of the array. Knowing that we only need to do N/2 processes, we can edit the loop to only travel to the midpoint of the array. 
```
void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length / 2; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
}
```
However, we are still not finished. We have a correct stopping point in traversal, but the  procedure is still faulty. If we let the algorithm stay as it is now, it will produce the exactly same faulty outputs as the original algorithm. This is because we do not have an adequate swapping procedure yet. 

Right now, we are simplying just copying the **last index - i** element in the array to the **ith index**, but there are no swaps occuring. The following example illustrates the faulty algorithm visually:
```
[1, 2, 3, 4, 5]
//i = 0 -> arr[0] = arr[arr.length - 0 - 1]
[5, 2, 3, 4, 5]
//i = 1 -> arr[1] = arr[arr.length - 1 - 1]
[5, 4, 3, 4, 5]
//i = 2 -> (i < arr.length) is false; exit loop
=====================================================
OUTPUT: [5, 4, 3, 4, 5]
```
Clearly, the elements in the latter half after the midpoint are not swapped at all. To fix this, we simply define a swapping procedure. 

```
void swap(int arr[], int a, int b){
    int save = a; 
    arr[a] = b; 
    arr[b] = save; 
}
```
Because copying an element overwrites the original data at the destination, we save the original data in a temporary variable. Then, we can copy the data from the temporary variable onto the elements respective location. After applying this logic onto our reversal algorithm, it becomes this: 

```
void reverseInPlace(int arr[]) {
    for(int i = 0; i < arr.length / 2; i++) {
        int save = arr[i];
        arr[i] = arr[arr.length - i - 1];
        arr[arr.length - i - 1] = save; 
    }
}
```
To test, this algorithm, we can start with the **smallest** cases and gradually move onto **larger** cases. 
First, we input an empty array. 
```
int arr[] = []; //expected output -> []
reverseInPlace(arr);
output(arr); 
=====================================================
OUTPUT: []
```
Then, we can try an array of size 1. 

```
int arr[] = [1]; //expected output -> [1]
reverseInPlace(arr);
output(arr); 
=====================================================
OUTPUT: [1]
```
Then, we can try arrays of sizes greater than 1. 
```
int arr[] = [1, 2, 3, 4, 5]; 
//expected output -> [5, 4, 3, 2, 1]
reverseInPlace(arr);
output(arr); 
=====================================================
OUTPUT: [5, 4, 3, 2, 1]
-----------------------------------------------------
int arr[] = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]; 
//expected output -> [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
reverseInPlace(arr); 
output(arr);
=====================================================
OUTPUT: [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
```
The program produces the expected output every time. 

In short, for this particular algorithm, the symptom (incomplete array reversal) was caused by two bugs. The first bug involved incorrect array traversal. To reverse an array, we only need to do **N/2 processes** as we swap **N/2 elements**. However, the original algorithm traversed the entirety of the array to do **N processes**. 

Secondly, the swapping procedure simply did not work at all in the original algorithm. It did not account for the second element in the pair to be swapped, resulting in only a partial swap. 

Fixing these bugs produces an algorithm that correctly reverses the array for all possible and legal inputs. 

The second algorithm involves removing elements from a list. We have a **Chooser** object that will determine whether an element should be kept or removed, using parameters defined by a user. A **new list** will be returned after the filtering. 

When we use this object on the list, all of elements flagged by the object to be removed should be removed from the list. It is important that the algorithm preserves the original order of the elements. 

For example, we are removing all string with numbers from an input list. 

```
list = ["123hi", "bye", "hello123", "apple", "lab"]
Chooser.filter(list, "Remove all strings with numbers in them"); 
output(list) //output ["bye", "apple", "lab"]
=====================================================
OUTPUT: ["bye", "apple", "lab"]
```
A faulty implementation of this algorithm is shown below. 
```
List<String> filter(List<String> list, StringChecker sc) {
    List<String> result = new ArrayList<>();
    for(String s: list) {
      if(sc.checkString(s)) {
        result.add(0, s);
      }
    }
    return result;
}
```

The implementation of the **Chooser object** is not important here, but we will assume that it operates on **lists of strings** to remove **all strings containing numbers**. **checkString(String str)** will return **true** if an String with no numbers is found and false otherwise. 

We can now begin testing this algorithm against some inputs, from **small** to **large**. 

```
list = new List("apple"); 
//EXPECTED OUTPUT -> [apple]
filter(list, new StringChecker());
output(list); 
=====================================================
OUTPUT: [apple]
```
Now, we can try removing an element. 
```
list = new List("apple123"); 
//EXPECTED OUTPUT -> []
filter(list, new StringChecker());
output(list); 
=====================================================
OUTPUT: []
```
Then, we can move onto lists with multiple elements. 
```
list = new List("apple123", "hi", "bark", "move123", "hello"); 
//EXPECTED OUTPUT -> ["hi", "bark", "hello"]
filter(list, new StringChecker());
output(list); 
=====================================================
OUTPUT: ["hello", "bark", "hi"]
```
As we can see, for arrays of multiple elements, the algorithm is **unstable**. Elements with numbers are successfully removed, but the order is not preserved. To find the bug, we must first examine the algorithm. 
```
List<String> filter(List<String> list, StringChecker sc) {
    List<String> result = new ArrayList<>();
    for(String s: list) {
      if(sc.checkString(s)) {
        result.add(0, s);
      }
    }
    return result;
}
```
We can first note that the traversal is done correctly. All elements of the array are checked. Therefore, the issue lies within the **loop**. 

We can see that there is an **if-statement** in which the **chooser object** checks if the element should be kept or removed. If it returns true, the item is kept, and it is **added** to the new list. 

However, we can see that it **prepends** the element instead of adding to the end. It adds all elements to **index 0**, meaning that elements are added in reversed order. 

To fix this, we can simply change the function so that elements are **appended** to the end of the new list. 

```
List<String> filter(List<String> list, StringChecker sc) {
    List<String> result = new ArrayList<>();
    for(String s: list) {
      if(sc.checkString(s)) {
        result.add(s);
      }
    }
    return result;
}
```
We can now begin testing the revised algorithm against some inputs, again from **small** to **large**. 

```
list = new List("apple"); 
//EXPECTED OUTPUT -> [apple]
filter(list, new StringChecker());
output(list); 
=====================================================
OUTPUT: [apple]
```
Now, we can try removing an element. 
```
list = new List("apple123"); 
//EXPECTED OUTPUT -> []
filter(list, new StringChecker());
output(list); 
=====================================================
OUTPUT: []
```
Then, we can move onto lists with multiple elements. 
```
list = new List("apple123", "hi", "bark", "move123", "hello"); 
//EXPECTED OUTPUT -> ["hi", "bark", "hello"]
filter(list, new StringChecker());
output(list); 
=====================================================
OUTPUT: ["hi", "bark", "hello"]
-----------------------------------------------------
list = new List("apple123", "hi", "bark", "move123", "hello", "farewell", "charm"); 
//EXPECTED OUTPUT -> ["hi", "bark", "hello"]
filter(list, new StringChecker());
output(list); 
=====================================================
OUTPUT: ["hi", "bark", "hello", "farewell", "charm"]
```
As we can wee, the algorithm now **preserves** the original order of the elements. 

In short, for this particular algorithm, the symptom (order of list not being preserved) was caused by one bug. Instead being added to the end of the new list, elements are added to the **front**. Changing this so that elements are **appended** to the end of the list fixes the algorithm, and it now produces the expected output for all possible and legal inputs. 







