# Contents:
1. [Day-1](#day-1)
    * [How Java Programs work](#how_java_programs_work)
    * [Why Java?](#why_java)
    * [JDK vs JRE vs JVM](#jdk_vs_jre_vs_jvm)
    * [JVM Architecture](#jvm_architecture)
    * [More about JIT Compiler](#more_about_jit_compiler)
    * [Writing the first Java Program](#writing_the_first_java_program)
    * [How to run Java Program](#how_to_run_java_programs)

## Day-1
<a name='day-1'></a>

### How Java Programs work:
<a name='how_java_programs_work'></a>
For any java program to run these are the steps that it needs to follow:
* Write the source code in a `.java` file. For example, `Hello.java`.
* The source code, wrote in a `.java` file is compiled using software called compiler.
* This generates a file with an extension of `.class`. So the file which will be generated will be `Hello.class` for the java source code `Hello.java`. This file is used to run the code.
* This `.class` file is not meant to run on any particular platform. This means that whether we run the program on a Windows, Mac or Linux platform, it'll run in the same manner.

When we compile the source code, it produces an intermediatory byte code(`.class` file). This intermediatory byte code is not meant for any real platform, but this can only run on __JVM(Java Virtual Machine)__. The __JVM__ is a java runtime environment, which translates from platform independent byte code to platform specific native code. This is called interpretation(runtime translation).

### Why Java?
<a name='why_java'></a>
When Starting Java, one thing to understand is- Why Java? What makes the Java Language this popular?
Some other benefits of using Java:
* Simple & Robust
* Platform or Architecture independent
* Secure
* Automatic Memory Management
* Inherent Multi-threaded support
* Object Oriented support -- Encapsulation,Inheritance & polymorphism
* Excellent I/O support
* Inherent networking support for TCP/IP , UDP/IP programming.

Steps to run a java program:
* Type the source code in a file with extension`.java ` file. For example, 'Test.java'.
* Run the command `javac Test.java`. This will output of a file with an extension `.class`. For our case, 'Test.class'.
* After the `.class` file is generated, run the command `java Test` to run the program. The `.class` part should be removed.

### JDK vs JRE vs JVM:
<a name='jdk_vs_jre_vs_jvm'></a>
![image](./additional_resources/jdk_vs_jre_vs_jvm.jpg)  
From the above diagram, we can see that JDK(Java Development Kit) can be divided into two section: _Java DEV Tools_ and _JRE(Java Run-time environment)_.

Java DEV Tools are referred to the applications/tools that are shipped with java for development of applications.

JRE can be further divided into two sections: _Java API Libraries_ and _JVM(Java Virtual Machine)_.

Java API Libraries are the default provided libraries along with the java application which help/support in the development on applications. Under the lib directory, there is a jar file named 'rt.jar' which contains all the packages(bundled collection of functionally similar classes).

And JVM(Java Virtual Machine) is the actual runtime where Java apps run. This consists of multiple sections which can be seen in the sectin below.


__NOTE:__ When we write `import java.io.*;` in a java function, it does n't mean that all the classes in `java.io` package will be loaded in the program, as `java.io` has hundreds of classes, and that'll only slow down the execution of the program. It only loads the classes that are requried in the program. 

### JVM Architecture:
<a name='jvm_architecture'></a>
![image](./additional_resources/JVM-architecture.png)
JVM has various sub components internally. 

1. Class loader sub system: JVM's class loader sub system performs 3 tasks
      * It loads .class file into memory.
      * It verifies byte code instructions.
      * It allots memory required for the program.

2. Run time data area: This is the memory resource used by JVM and it is divided into 5 parts
      * Method area: Method area stores class code and method code. (metaspace in Java SE 8)
      * Heap: Objects are created on heap.
      * Java stacks: Java stacks are the places where the Java methods are executed. A Java stack
	  contains frames. On each frame, a separate method is executed.
      * Program counter registers: The program counter registers store memory address of the
	  instruction to be executed by the micro processor.
      * Native method stacks: The native method stacks are places where native methods 
	  (for example, C language programs) are executed. Native method is a function,
	  which is written in another language other than Java.

3. Native method interface: Native method interface is a program that connects native methods 
	libraries (C header files) with JVM for executing native methods.

4. Native method library: holds the native libraries information.

5. Execution engine: Execution engine contains interpreter and JIT compiler, which converts byte 
	code into machine code. JVM uses optimization technique to decide which part to be interpreted
	and which part to be used with JIT compiler. The HotSpot represent the block of code executed
	by JIT compiler.

When class loader loads the classes on JVM, the HotSpot profiler finds out which section/piece of code is executed a lot of times, and thus if once compiled, can be used later with little to no calling change. So, the HotSpot profiler finds the HotSpot, and then gives it to the JIT Compiler. And JIT has a cache(local memory) and maintains native code caching, i.e., it saves the native code of the hotspot and whenever it encounters the same calling again, it uses the native code cache, rather than interpreting runtime again.

### More about JIT Compiler:
<a name='more_about_jit_compiler'></a>
The Just In Time Compiler (JIT) concept and more generally adaptive optimization is well known concept in many languages besides Java (.Net, Lua, JRuby).
 
Compiler is "a computer program that transforms the source language into another computer language (the target language)".

Static java compiler (javac) is a compiler that compiles human readable _.java_ files to a byte code that can be interpreted by JVM - _.class_ files. 

<p>JIT Compiler gathers statistics, finds the "hot" code, compiles it from JVM interpreted bytecode (that is stored in .class files) to a native code that is executed directly by Operating System and heavily optimizes it.  Smallest compilation unit is single method. Compilation and statistics gathering is done in parallel to program execution by special threads.During statistics gathering the compiler makes hypotheses about code function and as the time passes tries to prove or to disprove them. If the hypothesis is dis-proven the code is 
deoptimized and recompiled again.</p>

#### What optimizations JIT does?
Let's look closely at more optimizations done by JIT:
* _Inline methods_: instead of calling method on an instance of the object it copies the method to caller code. The hot methods should be located as close to the caller as possible to prevent any overhead. 
* Eliminate locks if monitor is not reachable from other threads
* Replace interface with direct method calls for method implemented only once to eliminate calling of virtual functions overhead
* Join adjacent synchronized blocks on the same object
* Eliminate dead code
* Drop memory write for non-volatile variables
* Remove prechecking NullPointerException and IndexOutOfBoundsException 
    
#### How JIT compiler and JVM work side by side:
* When the Java VM invokes a Java method, it uses an invoker method as specified in the method block of the loaded class object. 
* The Java VM has several invoker methods, for example, a different invoker is used if the method is synchronized or if it is a native method.The JIT compiler uses its own invoker. 
* The JVM check the method access bit for value __ACC_MACHINE_COMPILED__ to notify the interpreter that the code for this method has already been compiled and stored in the loaded class. 
* JIT compiler compiles the method block into native code for this method and stores that in the code block for that method. 
* Once the code has been compiled the __ACC_MACHINE_COMPILED__ bit, which is used on the Sun platform(JVM), is set.

### Writing the first Java Program:
<a name='writing_the_first_java_program'></a>

```java
class Hello {
      public static void main(String[] args) {
            System.out.println("Hello World");
      }
}
```

__Some Points to Note:__
* The above code can be saved in any file with an extension of `.java`.
* When not using any access specifier before the _class_ keywork, it'll use the _default_ access specifer.
* __Default__ access specifier means that when using or invoking the class with access set to _default_, the class will be visible, or can only be invoked from a different class from the same folder(package) and not from any class outside the folder in which the class in question is.
* The only two access specifiers which are available on class level:  
      * __Default__  
      * __Public__  
* If the class is created as a public class, you have to name the file of the source code as same name of the class. That's the reason that one `.java` file cannot have more than one public classes.
* Class is a unit of encapsulation, ie., the data and methods are encapsulated using a class as an encapsulating unit.
* Norms for naming:   
      1. The name of the class should begin with an upper case and then go with camel casing.  
      2. The name of the members(data members and methods) of the class should start with lower case and then go with camel casing.  
* The __main()__ method in a program is the starting point of execution of any program.
* Access specifers of the members of the class(data members and methods), in order of access from narrowest to widest, are:   
      1. __Private__ (Scope: visible within the same class)  
      2. __Protected__ (Scope: visible only to the subclasses/derived classes)  
      3. __Default__(package private) (Scope: visible only in the same folder/package)  
      4. __Public__ (Scope: visible everywhere)  
* The _static_ keyword in _main()_ method defination means that the _main()_ method can be called without creating an object/without instantiation.
* _void_ is a keyword used for the return type of the function. _main()_ method returns nothing. This is different from __C/C++__. That's because when running _C/C++_ we're running the code directly on the OS, and it needs a return to know whether the exuction of the program was a success or not. But since Java Programs don't run directly on the platform/OS, rather than a VM, it doesn't need to return anything back to the OS.
* In the signature of the _main()_ method, it needs a string array as an argument to capture the command line arguments. In Java String is a class in _java.lang_ package, rather than an array of characters like C/C++.
* The first element of the string argument array is the element that is passes. It doesn't represent the name of the program, like C/C++.

__How `System.out.println()` statement works:__
* Whenever a new class is created, `java.lang` package is automatically imported.
* In the `java.lang` package, there is a class called System. 
* The `System` class has a static data member of type `PrintStream`(from `java.io` package) class called as `out`. 
* `PrintStream` class has a lot of overloaded method with name `print()/println()` with different type of arguments.
* Using this data member of the system class, we can call the `print()/println()` method based .
And that's how the print function works.

### How to run Java programs:
<a name='how_to_run_java_programs'></a>
Before compilation of java source code, let's look at the folder structure:  
_src_: All the source code should be in this directory.  
_bin_: All the compiled code(intermeditiary byte code/`.class` files) should be saved in this directory.  

Thus, to compile the source code, use this command:
```bash
javac -d ../bin Hello.java
```
This means that after the compilation, all the `.class` files will be generated in the bin directory.
To run this program, you have to call the name of the class:
```bash
java Hello
```
```output
Hello World
```

__NOTE:__ Even if your program doesn't have a main method, and you try to run it, it doesn't give any compiler error and gives a `.class` file as an output of the compiler. Java thinks that we're compiling this file to be a part of/to be used in any other package. But if we try to run the code generated, it'll give this error:
```output
Error: Main method not found in class Hello, please define the main method as:
   public static void main(String[] args)
or a JavaFX application class must extend javafx.application.Application
```
And the reason being that inorder to run the code, JVM looks for a special function with a signature: `public static void main(Sting[] args)` in order to run the program.  
The same happens for any change in the signature of the main function, eg. `public static void main(String args)`, because the argument list doesn't have an array of Strings, but it has a single String parameter.

__NOTE:__ When compiling a single java source file, it'll produce the number of `.class` files as there are classes in the source code. For example:
```java
class Hello {
        public static void main(String[] args) {
                System.out.println("Hello from main");
        }
}

class A{
      // Empty class
}

class B{
      // Empty class
}
```
When we compile the above source file, it'll produce three `.class` files: Hello.java, A.java, B.java. And when we run these, other than Hello, the others give an error, as there is no main method in them.