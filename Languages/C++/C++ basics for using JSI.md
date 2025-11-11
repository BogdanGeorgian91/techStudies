# Basic compilation and the pre-processor

## Compiler

Needs a C++ compiler.
The first output of the compilation of a C++ program is an object file.
An object file is not an executable, not necessarily.
It is a one step before a standalone binary.
You need to link the object files with the dependencies, the dynamic libraries or static libraries that you want to use.
your program gets compiled into an object file. if you're using any dynamic library you won't be able to run it until you link it. And after you link it, then you will get a standalone executable. You can port it to a different machine and execute it.

## Pre-processor
Whenever you need to compile a program there are different stages. 
On C++ the initial stage which is actually not a compilation stage is the pre-processing of your file. 
Basically a pre-processor is a program that's gonna go through your program and take a look into the special directives (like imports) which are prefixed with a hashtag "#". And it's gonna do certain manipulation to your program based on these statements. 

# Namespaces and headers

 Header files allow you to declare interfaces to the implementation of your own code or classes or modules and they end with the ".h" extension. 
 The .cpp implementation files need to comply to the header files. 

So there can be the case when there are many header files stipulating the same interface and this might result in a conflict in a global space.
Namespaces help in the regard by telling the program the correct "context", interface which should be used by the implementation.
So you declare interfaces in certain namespaces and in the implementation files you need to write the respective namespace as well. 

The double colon operator "::" is similar to dot "." in JavaScript, except it's specifically meant for namespaces.
It's supposed to give you a clue, you're not calling an object or an instance of a specific function, you're just accessing a namespace and within that namespace you're accessing a particular function. 

 # Variables, vectors, maps, structs
 
 ## Variables
 "auto" type will dynamically take the type of whatever you'll assign it. After that you cannot change the type of that variable.
 In C++ you declare the type of a variable in front of it. 
 Also for functions you declare in front of it the type of the returned value.
 This way is convenient if you don't know the types of the variables you are trying to write. 

"const" keyword is a way to declare a variable which works similar to JS. The value is fixed.
You can mark arguments as constants too.  

## Vectors

Vectors are a type used for arrays when you want those arrays to be dynamically transformed, when their values are not fixed.
Behaves more like a JS array. 

## Maps

you can include the <map> namespace
std::map<std::string, std::string> myMap = std::map<std::string, std::string>();
myMap["Hello"] = "World";  

## Structs

they are not necessarily classes, they are small bundles of data.
similar to interfaces in TypeScript? 

# Pointers

A pointer is a memory address. 
how do we get the memory address of a variable? 
We have to use a special operator.  "&"


# Strings

# Function Scopes and  De-allocations

In C++ memory gets de-allocation, making the concept of function scopes work differently than how it is in JavaScript. 
When a function terminates in C++, memory gets de-allocated, it's context/scope gets de-allocated. 

# Lambdas

# Bitwise operations and Masks

# CMake







 
 
  
 

 



