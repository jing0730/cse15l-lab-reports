# Create your grading script here


J=".:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar"

rm -rf student-submission
git clone $1 student-submission

echo "The code is successfully cloned"

F=student-submission/ListExamples.java

if  [ -f "$F" ]; then
    echo "The file was found"
else
    echo "The file was not found."
    echo "Grade:0%"
    exit 1
fi

cp -r lib student-submission
cp TestListExamples.java student-submission/
cd student-submission
echo "In student-submission"

javac -cp $J *.java 2> CompileErr.txt
if [[ $? -ne 0 ]]; then
    echo "Your code did not compile successfully."
    echo "Grade:0%"
    exit 1
fi
echo "Your code was compiled"
java -cp $JU org.junit.runner.JUnitCore TestListExamples > JunitOut.txt

grep -q "2 tests" JunitOut.txt
if [[ $? -eq 0 ]]; then
   echo "Grade:100%"
 echo "All tests were passed!"
fi
grep -q "Failures: 1" JunitOut.txt
if [[ $? -eq 0 ]]; then
   echo "Grade:50%"
   echo "A test was failed"
fi

