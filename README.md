# Checksum.c
### COMP122: Generating a checksum value using a buffer

```
check·sum | ˈCHekˌsəm |
noun
a digit representing the sum of the correct digits in a piece of stored or transmitted 
digital data, against which later comparisons can be made to detect errors in the data.
```


# Purpose:
Now that you have demostrated the ability to write a Java program to compute a checksum, your task is to write the same program in C. The algorithm, however, is slighlty different to perform the checksum incremently within a loop.  This algorthm was described in class.  Also within this implementation you must use a signal 'read' system call to obtain 10 bytes used as the input to the program. 

The purpose of this assignment is as follows:
  1. Introduce you to the C program language and to demostrate that because you know Java you already know a fair amount of C.
  1. Familize yourself with the 'make' utility used to maintain software projects.
  1. Establish an understanding of  OS system calls and the use buffers.
  

# Assignment:
1. Fork this repository as a new software project
1. Use 'git' as part of your software development process
1. Write a C program that computes a simple checksum of 10 8-bit integers.  This program is *based* upon the calculation of the checksum value of a IPv4 header, defined by RFC791. 
1. Provide the URL of your project as the sumbisssion to this assignment.

Your C program should conform to the following specification:

  * Program name: checksum
  * Reads 10 bytes from standard input (stdin), using the 'read' system call
  * Interpets or casts each of these bytes as an integer value in the range of 0..2^8-1 (I.e., 0..255). 
  * Via a loop, examines each of these bytes in turn,
     * computes the running 1's complement sum for 8-bit numbers
     * saves the 6th byte into a variable called "checksum", and use the value of zero (0) instead for the calculation of the sum
  * Performs the one's complement of the final sum and saves this value to a variable called "complement"
  * Outputs the value of "checksum" and "complement" to standard output (stdout) via the printf C library call
  * If the value of "checksum" and "complement" are not the same, outputs the string "Error Detected!" to standard error (stderr).


### Minimum Validation Checks:
* I leave this up to you.  Document these validation checks as part of a summary documentation at the top of the program.

### Starter Code:

```
/********************************/
/* Program Name:                */

#include "stdio.h"
#include "stdlib.h"

#define max_int (255)
#define byte (char)

int main (int argc, char * argv[], char ** envp) {

  int count = 10;
  int sum = 0;   
  byte checksum; 
  byte complement;

  /* the following is the prototype for the read system call */
  /* int read(int fildes, void *buf, size_t nbyte);  */
```

```
  fprintf(stdout, "Stored Checksum: %d, Computed Checksum: %d\n", checksum, complement);
  if (checksum != complement ) {
    fprintf(stdout, "Error Detected!\n"); 
    return 1;
  }
  return 0;
}
```

### Testing:
Use the following to test your program.

```
$ java checksum < 156.txt
Stored Checksum: 156, Computed Checksum: 156
```

```
$ java checksum < 229.txt
Stored Checksum: 229, Computed Checksum: 132
Error Detected!
```

```
$ java checksum < 81.txt
Stored Checksum: 81, Computed Checksum: 81
```

The file "47201.txt" is a correct test case for the enhanced program using 16-bit integers.

### Timing the Program:
Once you have your program working, you need to time the execution of the program. Moreover, you should compare your time to the execution time of two other programs written by the professor.  The first program was written in Java, and the second in C.  You should expect to see that the Java programs run much slower that the corresponding C program.

For this part of the assignment, you need to ensure your program works correctly on the ssh.sandbox.csun.edu server.  Thsi is the server in which the executing times will be perform.

```
$ scp checksum.java ssh.sandbox.csun.edu:    # Secure copy your program to the ssh.sandbox.csun.edu server.
$ scp 156.txt ssh.sandbox.csun.edu:          # Secure copy the 156 test case to the ssh.sandbox.csun.edu server.
$ ssh ssh.sandbox.csun.edu                   # Log into the ssh.sandbox.csun.edu server
$ javac checksum.java                        # Compile your checksum program
```

Now that you have a working program on the server, let's time and compare execution times.  Here we use the 'script' program to record this activity. The file 'timing

```
$ script checksum.typescript                 # Start and record a session, with the session recorded in the checksum.typescript file
$ java checksum < 156.txt                    # Run your checksum program to make sure things are working
$ pushd ~steve/comp122/checksum              # Temporarily change the working directory to utilize his version
$ time java checksum < 156.txt               # Time the execution of the professor's Java version
$ time ./checksum < 156.txt                  # Time the execution of the professor's C version
$ popd                                       # Change the workind directory back
$ time java checksum < 156.txt               # Time the execution of your Java version
$ exit                                       # Exit the script program
```


### Submission:
1. The checksum.java source code
1. The checksum.typescript file

# Program Enhancements:
1. Modify the program and your test cases to use 16-bit integers (i.e., input numbers can now be (I.e., 0..64k, or 0..65535)
1. Use the first input number to determine the number of integers to be read and store this value in the variable "count".  
  * The value of "count" is determined by the following java assignment `count = ((number >> 8) & 0x0F)*2;` where number is the first input number read.

# Other Information and Resources:
This program is based upon the structure if a IPv4 packet.  Although you do not need to understand this structure to complete this assignment, you may want to review some of the material associated with IPv4.  More information will be provided in the lecture.
* https://en.wikipedia.org/wiki/IPv4
* https://en.wikipedia.org/wiki/IPv4_header_checksum

### Notes:
* The IPv4 header utilizes a checksum field that is 16-bit quanity. This program reduces the size in half to a 8-bit quantity.  Correspondingly, the following changes are made to keep things consistent: 
* The IPv4 header, without any options, has a total size of 20 bytes. This program reduces the size in half to a 10 byte size.
* This checksum value is stored in the 11th & 12th byte of the IPv4 header. This program assigns its location to the 6th byte.


