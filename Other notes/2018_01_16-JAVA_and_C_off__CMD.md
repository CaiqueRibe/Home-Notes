---
tags:
  - Tutorial
  - JAVA
  - C
  - CMD
---
# JAVA
``````Java
public class HelloWorld{
  
  public static void main(String[] args)
  {
    System.out.println("Hello, World!");
  }

}
``````
*HelloWorld.java*

<br>

C:\> `cd` \mywork
C:\mywork> `dir`

C:\mywork> `set path=%path%;C:\Program Files\Java\jdk1.8.0_144\bin`
C:\mywork> `javac` *HelloWorld.java*
C:\mywork> `dir`
C:\mywork> `java` *HelloWorld*

Hello, World!


# C
### Make Sure Compiler Is Installed On a Unix-like System
`````
which gcc
gcc --version
`````
### Write C Program - Example:
````c++
#include <stdio.h>

int main(void){
  printf("My first C program\n");
  return 0;
}
````
### Compile Program
```
 gcc first.c -o first
 cc first.c -o first
 ./first
`````
````
rm first
make first
./first
`````
`rm and make` to remove and make to remake the file (makefile concept)


<br>

[Cyberciti](http://link) --> Linux and Unix tutorials for new and seasoned sysadmi
[How To Compile C Program And Create Executable](https://www.cyberciti.biz/faq/compiling-c-program-and-creating-executable-file/)