# **Lab Report 3**
Jason Kim, A16953241

In Week 4's lab, we looked at JUnit tests and debugging programs with various symptomatic errors. In Week 5's lab, we used different commands in bash to help us manipulate files and gather more information about certain attributes of those files and directories. In this lab report, we will see the efforts and process of our debugging, as well as perform a deep dive into `find`.

## Part 1: Bugs

After forking and cloning the provided repository, we started our debugging process with the `ArrayExamples.java` and `ArrayTests.java` files. The first of the two contains a buggy method called `reverseInPlace` that we will fix.

We wrote a first test for the `reverseInPlace` method called `testReverseInPlace`. 
```
@Test 
	public void testReverseInPlace() {
    int[] input1 = { 3 };
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{ 3 }, input1);
	}
```
This test method instantiates an int array with only one element: 3. We expect that if we call the method on this array, since there is only one element, the reversed array will be exactly the same.


We can see below that the files compiled properly and there were no test failures. This test passed for our method!
```
Jasons-Air:lab3-main jasonkim$ javac -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar *.java
Jasons-Air:lab3-main jasonkim$ java -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore ArrayTests
JUnit version 4.13.2
.
Time: 0.013

OK (1 test)
```


Now let us write another test for our method `reverseInPlace` with some more elements in our input array:
```
@Test
  public void testReverseInPlace2() {
    int[] input2 = {1,2,3};
    ArrayExamples.reverseInPlace(input2);
    assertArrayEquals(new int[]{3,2,1}, input2);
  }
```
This test, named `testReverseInPlace2()`, instantiates an int array with three elements: 1, 2, and 3. We expect this method to reverse the elements, so after calling the method on our array, we compare it using `assertArrayEquals` to compare our modified array to a new array with the elements assigned in reversed order.


As we can see below, the files compiled but there was a test failure from the new test we implemented.
```
Jasons-Air:lab3-main jasonkim$ javac -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar *.java
Jasons-Air:lab3-main jasonkim$ java -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore ArrayTests
JUnit version 4.13.2
.E.
Time: 0.01
There was 1 failure:
1) testReverseInPlace2(ArrayTests)
arrays first differed at element [2]; expected:<1> but was:<3>
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:78)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:28)
        at org.junit.Assert.internalArrayEquals(Assert.java:534)
        at org.junit.Assert.assertArrayEquals(Assert.java:418)
        at org.junit.Assert.assertArrayEquals(Assert.java:429)
        at ArrayTests.testReverseInPlace2(ArrayTests.java:16)
        ... 32 trimmed
Caused by: java.lang.AssertionError: expected:<1> but was:<3>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at org.junit.internal.ExactComparisonCriteria.assertElementsEqual(ExactComparisonCriteria.java:8)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:76)
        ... 38 more

FAILURES!!!
Tests run: 2,  Failures: 1
```
We can see that JUnit tells us what was expected and what we actually got from running the program. We see that the assertionError occurred on the last element (element with index 2) of our array, where we expected to have a value of 1 (since 1 was our value in index 0 of the original array), but we got a value of 3 instead. 

When we take a look at our code in the `ArrayExamples.java` file:
```
// Changes the input array to be in reversed order
  static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
  }
```
We can iterate using the for loop through our small array of three elements manually to see why the error occurs.

Iteration 1: i = 0
The statement in the loop evaluates to `arr[0] = arr[3 - 0 - 1]`, which assigns the int in index 2 to index 0. So our array looks like: `{3, 2, 3}`.

Iteration 2: i = 1
The statement in the loop evaluates to `arr[1] = arr[3 - 1 - 1]`, which assigns the int in index 1 to index 1. So our array looks like: `{3, 2, 3}`.

Iteration 3: i = 2
The statement in the loop evaluates to `arr[2] = arr[3 - 2 - 1]`, which assigns the int in index 0 to index 2. So our array looks like: `{3, 2, 3}`. This is where we have our bug. We did not save the int in index 0 anywhere else, so when we overwrite the value at index 0, the original int is lost and we cannot reverse the array properly.

We can fix our bug by creating a "temporary" variable to store each value in before overwriting:
```
// Changes the input array to be in reversed order
  static void reverseInPlace(int[] arr) {
    for (int i = 0; i < arr.length / 2; i += 1) {
      int temp = arr[i];
      arr[i] = arr[arr.length - i - 1];
      arr[arr.length - i - 1] = temp;
    }
  }
```
We assign the value at index `i` to `temp` before overwriting with the value at index `arr.length - i - 1`. Then we overwrite the value at index `arr.length - i - 1` with the value originally at index `i` (but now stored in `temp`). Thus, we successfully finish swapping the two values.

We also change the conditional of the loop to `arr.length / 2` since we are swapping each pair of ints (one in the first, one in the second halves of the array), we only need to iterate through half of the array.

Let us see if this new method implementation works for our tests:
```
Jasons-Air:lab3-main jasonkim$ javac -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar *.java
Jasons-Air:lab3-main jasonkim$ java -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore ArrayTests
JUnit version 4.13.2
..
Time: 0.011

OK (2 tests)
```
We have successfully debugged our method!

## Part 2: Researching Commands: `find`

We explored the `find` command a little bit in Lab 5, and in this lab report, we will explore four different options for the `find` command that we can utilize for different results.

All options below were found using the `man find` command, which gives us a a manual on arguments and options for a given command directly in the terminal.

`-name` option
---
We first saw the use of the `-name` option during lab to find certain files with a specific pattern; for example, all files ending in `.txt`. Let us explore using different patterns to find files in the `./technical` directory from lab.

```
Jasons-Air:docsearch jasonkim$ find ./technical -name Legal*
./technical/government/About_LSC/LegalServCorp_v_VelazquezSyllabus.txt
./technical/government/About_LSC/LegalServCorp_v_VelazquezDissent.txt
./technical/government/About_LSC/LegalServCorp_v_VelazquezOpinion.txt
./technical/government/Media/Legal-aid_chief.txt
./technical/government/Media/Legal_system_fails_poor.txt
./technical/government/Media/Legal_Aid_in_Clay_County.txt
./technical/government/Media/Legal_hotline.txt
./technical/government/Media/Legal_Aid_looks_to_legislators.txt
./technical/government/Media/Legal_Aid_Society.txt
./technical/government/Media/Legal_Aid_attorney.txt
./technical/government/Media/Legal_services_for_poor.txt
./technical/government/Media/Legal_Aid_campaign.txt
```
In this example, we wanted to find something related to legal systems and legal precedents. Rather than searching through the entire filesystem, we used the pattern `Legal*` to find any files with names that follow the pattern, and the filepaths are printed out to the user. This might be helpful in situations where we are finding certain topics of files through large databases, or could be used to implement a search function or system.

```
Jasons-Air:docsearch jasonkim$ find ./technical -name *2003-4-6*
./technical/biomed/gb-2003-4-6-r41.txt
./technical/biomed/gb-2003-4-6-r37.txt
./technical/biomed/gb-2003-4-6-r39.txt
```
In this example, we wanted to find some biomedical research papers that were published on 4/6/2003. The `-name` option allows us to filter for files with that pattern, and returns the filepaths to all of the matches. If all of the files in the system had their date of publication in the filename, we would be able to search effectively for certain criteria.


`-type` option
---
We now look at the `-type` option, which allows us to use the `find` command to recursively search for certain types of files (like regular files, directories, etc.). In our case, we are most likely to use it to filter for only files or only directories.

```
Jasons-Air:docsearch jasonkim$ find ./technical -type d
./technical
./technical/government
./technical/government/About_LSC
./technical/government/Env_Prot_Agen
./technical/government/Alcohol_Problems
./technical/government/Gen_Account_Office
./technical/government/Post_Rate_Comm
./technical/government/Media
./technical/plos
./technical/biomed
./technical/911report
```
In this example, we wanted to see how many directories there were in the entire `./technical` directory (including itself). We can also see that all of the subdirectories were printed out to the user as well. This could be useful in a large filesystem for searching for a specific file, where you know the type of the file but can't remember which subdirectory it is in.

```
Jasons-Air:docsearch jasonkim$ find ./technical/government/A* -type f
./technical/government/About_LSC/LegalServCorp_v_VelazquezSyllabus.txt
./technical/government/About_LSC/Progress_report.txt
./technical/government/About_LSC/Strategic_report.txt
./technical/government/About_LSC/Comments_on_semiannual.txt
./technical/government/About_LSC/Special_report_to_congress.txt
./technical/government/About_LSC/CONFIG_STANDARDS.txt
./technical/government/About_LSC/commission_report.txt
./technical/government/About_LSC/LegalServCorp_v_VelazquezDissent.txt
./technical/government/About_LSC/ONTARIO_LEGAL_AID_SERIES.txt
./technical/government/About_LSC/LegalServCorp_v_VelazquezOpinion.txt
./technical/government/About_LSC/diversity_priorities.txt
./technical/government/About_LSC/reporting_system.txt
./technical/government/About_LSC/State_Planning_Report.txt
./technical/government/About_LSC/Protocol_Regarding_Access.txt
./technical/government/About_LSC/ODonnell_et_al_v_LSCdecision.txt
./technical/government/About_LSC/conference_highlights.txt
./technical/government/About_LSC/State_Planning_Special_Report.txt
./technical/government/Alcohol_Problems/Session2-PDF.txt
./technical/government/Alcohol_Problems/Session3-PDF.txt
./technical/government/Alcohol_Problems/DraftRecom-PDF.txt
./technical/government/Alcohol_Problems/Session4-PDF.txt
```
In this example, we can see how we can use patterns in the path argument to search for files inside of multiple directories. We specified that we want to filter for regular files inside of the `./technical/government` directory, and only inside of subdirectories that start with `A`. Thus the printed output consists of files from the `./technical/government/About_LSC` directory and the `./technical/government/Alcohol_Problems` directory. Having this option allows us to narrow down the exact file path in a complicated filesystem when we have some information about the files we're looking for.


`-print` option
---

We now look at the `-print` option for `find`, which prints the filepath for a given file or the contents of a directory. Let us first look at if a path to a file is given as the argument.

```
Jasons-Air:docsearch jasonkim$ find technical/911report/chapter-1.txt -print
technical/911report/chapter-1.txt
```
We see that when we use a path to a file, the path is printed out back to us. Not a very helpful use case, but we can also try using a directory path as an argument.

```
Jasons-Air:docsearch jasonkim$ find technical/911report -print
technical/911report
technical/911report/chapter-13.4.txt
technical/911report/chapter-13.5.txt
technical/911report/chapter-13.1.txt
technical/911report/chapter-13.2.txt
technical/911report/chapter-13.3.txt
technical/911report/chapter-3.txt
technical/911report/chapter-2.txt
technical/911report/chapter-1.txt
technical/911report/chapter-5.txt
technical/911report/chapter-6.txt
technical/911report/chapter-7.txt
technical/911report/chapter-9.txt
technical/911report/chapter-8.txt
technical/911report/preface.txt
technical/911report/chapter-12.txt
technical/911report/chapter-10.txt
technical/911report/chapter-11.txt
```
This output is much more helpful, as it printed out the filepaths of all files in that directory. If there were nested directories in this directory, the filepath to files in those directories would also be printed. This is a similar use case to the `-name` option, but we can use this as a diagnostic to see if we are getting the right directory before doing something else, like output redirection into a file. This is also helpful in navigating and looking around large filesystems without having to `cd` into multiple nested directories.


`-mtime` option
---
For the sake of exhibiting some examples, 11 of the files in `technical` were modified by adding a space or another random character in their contents.

The `-mtime` option for `find` is used to search for files based on modification time.
```
Jasons-Air:docsearch jasonkim$ find technical -mtime -1
technical
technical/government
technical/government/Env_Prot_Agen/ro_clear_skies_book.txt
technical/government/Alcohol_Problems
technical/government/Alcohol_Problems/DraftRecom-PDF.txt
technical/government/Post_Rate_Comm/Cohenetal_Cost_Function.txt
technical/plos/journal.pbio.0020353.txt
technical/plos/journal.pbio.0020404.txt
technical/biomed/1471-213X-1-3.txt
technical/biomed/1471-2121-3-18.txt
technical/biomed/1471-213X-1-13.txt
technical/911report/chapter-7.txt
technical/911report/chapter-12.txt
```
We gave the argument `-1` here, and as we manually modified these random files within the last hour of running the `find` command, the paths to those files were printed. We also see that the `technical` and `technical/government` directories were printed; this means that they were modified within the last hour as well.

Let us expand the modification time:
```
Jasons-Air:docsearch jasonkim$ find technical -mtime -5
technical
technical/government
technical/government/About_LSC
technical/government/Env_Prot_Agen
technical/government/Env_Prot_Agen/ro_clear_skies_book.txt
technical/government/Alcohol_Problems
technical/government/Alcohol_Problems/DraftRecom-PDF.txt
technical/government/Gen_Account_Office
technical/government/Post_Rate_Comm
technical/government/Post_Rate_Comm/Cohenetal_Cost_Function.txt
technical/government/Media
technical/plos
technical/plos/journal.pbio.0020353.txt
technical/plos/journal.pbio.0020404.txt
technical/biomed
technical/biomed/1471-213X-1-3.txt
technical/biomed/1471-2121-3-18.txt
technical/biomed/1471-213X-1-13.txt
technical/911report
technical/911report/chapter-7.txt
technical/911report/chapter-12.txt
```
Here we gave the argument `-5`, and in addition to all of the previous filepaths, there are more filepaths for both files and directories printed. The new filepaths are the files that were modified somewhere between 1 and 5 hours ago. These use cases can be helpful if a file was closed accidentally while working and we needed to open it up again, or if we were to accidentally lose a file by sending it to an unknown directory, we could find it through this option.

From the explorations into these various options, we can see that the `find` command is not a command limited to finding just a single given file in our filesystem.