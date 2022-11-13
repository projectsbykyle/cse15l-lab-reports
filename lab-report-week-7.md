
# Lab Report - Week 7 - 

## Part 1: 
Here, we attempt to insert a new text of code using less than thirty Vim commands. This includes all keystrokes, such as **h**, **j**, **k**, **l**, and **shift-o** (this will be counted as two), just to name a few. 

This is the unedited code. 

![image](https://projectsbykyle.github.io/cse15l-lab-reports/UneditedFile.png)
Our aim is to insert a new line above **line 17**, which is:
```
file[] paths = f.listFiles(); 
```
In the least keystrokes possible, we aim to insert this line above it:
```
System.out.println(f.toString() + " is a directory"); 
```
To start off, we must first jump to the line. One way would be to contiuously use the down command **j**, but there is a faster way. 

We simply type the following into the normal mode command line and press enter. 
```
:17 
```
This takes us to line 17 immediately. 
![Image](https://projectsbykyle.github.io/cse15l-lab-reports/JumpingToLine17.png)

Now that we are at line 17, we have to enter insert mode to input our desired text. One way would be to press **i** and use the **up arrow key** to move up a line before finally using **enter** to insert a new line.

However, Vim has a faster command that does all of these for us. We simply type the following and press enter: 
```
<shift>-O
```
For clarification, we press the **shift** key and the **o** key. It is important to note that no feedback in the normal mode command line is given for this command. 

As we can see in the screenshot, a new line is inserted above what used to be line 17. 
![Image](https://projectsbykyle.github.io/cse15l-lab-reports/EnteringInsertMode.png)

Conveniently, the command also places us in **insert mode**, so we can begin typing our line immediately. 


![Image](https://projectsbykyle.github.io/cse15l-lab-reports/Typing.png)

Finally, we have to save our work. To do so, we first press the **escape** key to re-enter **normal mode**. Finally, we type **:wq** to **save** and **exit**. 

![Image](https://projectsbykyle.github.io/cse15l-lab-reports/SavingWork.png)

After pressing **enter**, the Vim editor closes, and we have successfully finished our work. 

From start to finish, these are the keystrokes that we used. 

```
:17<enter><shift>o System.out.println(f.toString() + "is a directory) <esc>:wq<enter>
```
In total, including **enter** and **esc**, we used **11 keystrokes** to call Vim commands in our edits. 
## Part 2

Using the first method, where we make the edits locally and **scp** it onto the the remote server, it took me **55** seconds from start to finish. 

Using the second method, where we start on the server and make edits using Vim, it took me **28 seconds** from start to finish. 

I prefer using the second method simply because it is much quicker than first editing on Visual Studio Code and copying the file to the remote server. Using the command line and only the keyboard is a much more efficient work process. 
 
 However, project factors may also affect my final choice in deciding between the two. One big issue for me is debugging. It is much easier to debug in an IDE, such as Visual Studio Code, than in Vim. For big projects with lots of code, navigation may be easier in Vim, but it there is no code auto-complete or syntax checking, making IDEs much more practical in this regard. As such, another factor other than working speed is the size of the project. 