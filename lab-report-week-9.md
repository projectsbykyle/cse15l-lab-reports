# Lab Report Week 9

The following is a bash script for grading basic Java programs for students. 
```
# Create your grading script here
# set -e

rm -rf student-submission
git clone $1 student-submission

# Check if ListExamples.java exists
echo "CHECK IF LISTEXAMPLES EXISTS" 
pwd
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
pwd
mkdir TestStudentWork 
cp *.java TestStudentWork 
cp ../TestListExamples.java TestStudentWork
 
# Compile tests -> if fail detect and give feedback 
echo "COMPILING TESTS"
cd TestStudentWork
pwd
javac -cp .:../../lib/hamcrest-core-1.3.jar:../../lib/junit-4.13.2.jar *.java 2> CompileError.txt

if [[ $? -eq 0 ]]
then
    echo "COMPILED SUCCESSFULLY"
else
    echo "COMPILE ERROR"
    cat error.txt
    echo "SCORE: ZERO"
    exit 2
fi     

# Run tests and report based off of junit output -> don't use set -e -> mark each bad exit code 
echo "RUNNING TESTS"
pwd 
java -cp .:../../lib/hamcrest-core-1.3.jar:../../lib/junit-4.13.2.jar org.junit.runner.JUnitCore TestListExamples > RunResults.txt
if [[ $? -eq 0 ]]
then
    tail -2 RunResults.txt
else
    tail -2 RunResults.txt 
    echo "FAIL"
fi    


```