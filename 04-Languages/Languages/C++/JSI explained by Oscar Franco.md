
JSI is not a React Native only feature, because it's actually implemented in the JavaScript Engine itself. 

You can call synchronously from the JavaScript layer C++ functions and objects via the JSI. 

# Turbo Modules

Turbo Modules is a new architecture for React Native packages. 
For context, before auto-linking was there you would have had to go in the native code, especially on android, and initialize your own packages yourself and then register them on your React class on the android side of things, Java side of things. 
Then auto-linking came in that simplifies that work and now the next step is Turbo Modules. You can think of it as a way to create the native packages. 

Besides a new API, what Turbo Modules is meant to do is to allow for lazy initialization of packages. 
Right now when your React Native app starts, all of your native packages need to be initialized at the same time. So if the application is large every time you add a React Native package the start up time is gonna get slower and slower because all of them need to be initialized at the same time. 

With Turbo Modules they can be lazily evaluated. That is, your native package might not be initialized until you need it or you call it so this will improve the initial loading time of your app.
Besides this, Turbo Modules offers a new API and a new way of creating these packages. JSI modules are kinda meant to work together with Turbo Modules. 

# CODEGEN

Initializing a package incurred a certain amount of native knowledge because you had to go inside the native code and initialize the packages.
Codegen is meant to help you with this where you can define your interface for your package in TypeScript and then it's gonna generate all the boilerplate code that you need to initialize your native module. 
Codegen doesn't generate the implementation code of your package, it generates all the boilerplate code that is necessary for the modules to work.
Codegen and Turbo Modules work in unison. 
All the implementation of the Codegen packages will generate Turbo Modules code. 

# JSI 

RN takes advantage of this feature of the JavaScript's engine in order to get the most performance possible. 

# Fabric

Fabric takes advantage of all the new pieces of the new architecture and implements the renderer of the React code into a more native version of a renderer. 

# Setting up a JSI module

The "runtime" instance of the JSI is the most important object that allows us to inject properties in the JavaScript context, to create certain objects that can be read both by JavaScript and C++. It's pretty much the key that allows us to write the JSI modules. 

# Installing JSI stubs


# # Registering a function

How do we register a JSI function ?
Everything is based on the Runtime object: "facebook::jsi::Runtime &rt"
The Runtime object is the bridge between our C++ code, the native binary land and the JavaScript runtime. 
rt.global(). ... => returns the global scope of JavaScript. 
You create a JSI function by using the function class: facebook::jsi::Function

# JSI API

with JSI you manipulate JavaScript from the host language, from C++
We create all the JavaScript objects that we need, we just have to do it through the JSI. 
