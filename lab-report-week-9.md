# Lab Report Week 9

## Proof of Concept

The following is a bash script for grading basic Java programs for students. 
```
# set -e

rm -rf student-submission
git clone $1 student-submission

# Check if ListExamples.java exists
echo "CHECK IF LISTEXAMPLES EXISTS" 
cd student-submission 
if [[ -f "ListExamples.java" ]]
then
    echo "File found"
else
    echo "ERROR: ListExamples.java not found"
    exit 1
fi 

# Copy STUDENT CODE and YOUR OWN TEST JAVA FILE into same directory 
echo "COPYING INTO DIRECTORY"
mkdir TestStudentWork 
cp *.java TestStudentWork 
cp ../TestListExamples.java TestStudentWork
 
# Compile tests -> if fail detect and give feedback 
echo "COMPILING TESTS"
cd TestStudentWork
javac -cp .:../../lib/hamcrest-core-1.3.jar:../../lib/junit-4.13.2.jar *.java 2> CompileError.txt

if [[ $? -eq 0 ]]
then
    echo "COMPILED SUCCESSFULLY"
else
    echo "COMPILE ERROR"
    cat CompileError.txt
    echo "SCORE: ZERO"
    exit 2
fi     

# Run tests and report based off of junit output 
echo "RUNNING TESTS"
java -cp .:../../lib/hamcrest-core-1.3.jar:../../lib/junit-4.13.2.jar org.junit.runner.JUnitCore TestListExamples > RunResults.txt
if [[ $? -eq 0 ]]
then
    echo "ALL TESTS PASSED"
    echo "SCORE: 100"
else
    tail -2 RunResults.txt 
    echo "FAIL"
fi    
```

It is a biggest grading script that can allow us to test some List methods from class. Below, we test a couple two faulty merge implementations along with a working one. We use the Server class provided for this, using the following link format to generate the output we show later:

```
http://localhost:4000/grade?repo=<insert Github link>
```

First, we input a [student submission](https://github.com/ucsd-cse15l-f22/list-methods-compile-error) that has a **compile error**and here is the output. 


![Image](https://projectsbykyle.github.io/cse15l-lab-reports/CompileError.png)


Next, we input a [student submission](https://github.com/ucsd-cse15l-f22/list-methods-lab3) that has a **logic error**, and here is the output. 


![Image](https://projectsbykyle.github.io/cse15l-lab-reports/RuntimeError.png)



Finally, we input a [student submission](https://github.com/ucsd-cse15l-f22/list-methods-corrected/blob/main/ListExamples.java) that works as intended. 

![Image](https://projectsbykyle.github.io/cse15l-lab-reports/WorkPerfect.png)

## Script Tracing
We now consider the second example with the **runtime error**, **tracing its script** to determine the **standard error**, **standard output**, and **exit** code for each bash command.  
![Image](https://projectsbykyle.github.io/cse15l-lab-reports/RuntimeError.png)

We first begin at the following two lines, where we clone the student work into our own folder.

```
rm -rf student-submission
git clone $1 student-submission
```
Because we wrote **set -e**, the program will stop if there are any errors. We see that there are none, so we know that the exit codes for these two commands are **0**. Because there are no errors, **standard error** is not used, and the only **output** we see is from **git**, which is telling us that it is cloning into the folder. 

Next, we enter this code block:
```
echo "CHECK IF LISTEXAMPLES EXISTS" 
cd student-submission 
if [[ -f "ListExamples.java" ]]
then
    echo "File found"
else
    echo "ERROR: ListExamples.java not found"
    exit 1
fi 
```
Echo simply prints the given argument, and we see that it is successfully printed to **standard output**, so the **exit code** is again **0**. **cd** simply changes the current directory to **student-submission**, and because that folder exists, the exit code is also **0**. 

The boolean condition --- **-f "ListExamples.java"** --- evaluates to true because we successfully cloned the repository, and the file **ListExamples.java** is inside of it. The exit code is also **0**. Because the condition is satisfied, we print "File found" to **standard output**, and the exit code here is also **0**.Note that the two commands in the else clause do not wrong because the condition is true. 

We then enter the next part: 

```
# Copy STUDENT CODE and YOUR OWN TEST JAVA FILE into same directory 
echo "COPYING INTO DIRECTORY"
mkdir TestStudentWork 
cp *.java TestStudentWork 
cp ../TestListExamples.java TestStudentWork
 
# Compile tests -> if fail detect and give feedback 
echo "COMPILING TESTS"
cd TestStudentWork
javac -cp .:../../lib/hamcrest-core-1.3.jar:../../lib/junit-4.13.2.jar *.java 2> CompileError.txt
```

**echo** and **mkdir** have exit codes of **0**, and the former prints to **standard output**. The two **cp** commands also have exit codes of **0** because the files exist. 

The second **echo** command also prints to **standard output** and exits with code **0**. The **cd** command has an exit code of **0** because the directory exists. The **javac** command has an exit code of **0** because the compilation was successful. If it failed, then the **standard error stream** would be redirected to **CompileError.txt**.

We then check if the compilation was successful: 
```
if [[ $? -eq 0 ]]
then
    echo "COMPILED SUCCESSFULLY"
else
    echo "COMPILE ERROR"
    cat CompileError.txt
    echo "SCORE: ZERO"
    exit 2
fi    
```
Because our submission compiles, **javac** has an **exit code 0**, so the boolean condition is satisifed, and **echo**, which again has **exit code 0**, reports that the compilation was successful through the **standard output** stream. Note that the four commands in the **else clause** are not run because the boolean condition was true. 

Finally, we run the tests on the student submission: 

```
# Run tests and report based off of junit output 
echo "RUNNING TESTS"
java -cp .:../../lib/hamcrest-core-1.3.jar:../../lib/junit-4.13.2.jar org.junit.runner.JUnitCore TestListExamples > RunResults.txt
if [[ $? -eq 0 ]]
then
    echo "ALL TESTS PASSED"
    echo "SCORE: 100"
else
    tail -2 RunResults.txt 
    echo "FAIL"
fi    
```

**Echo** simply prints to **standard output** and has an exit code of **0**. Upon running the test though, because we use a faulty submission, the test runs fail for **Junit**, and the exit code for the **java** command is **nonzero**. The **standard error** stream is redirected to **RunResults.txt**. Because the exit code for the previous command is **nonzero**,the boolean condition fails, so the statements in the **if-clause** are not run. We then print the last line of **RunResults.txt**, and this prints to **standard output** and has exit code **0**. **Echo** also does the same and has exit code **0**.  