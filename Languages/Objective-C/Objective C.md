# Methods:

```
ObjC
[someObject doSomething];

Swift equivalent
someObject.doSomething()

Argument to out method:
[someObject doSomethingWithArg:arg]; - ObjC
someObject.doSomething(withArg:arg) - Swift

[someObject doSomethingWithArg:arg1 otherArg:arg2]; - ObjC
someObject.doSomething(withArg:arg1 otherArg:arg2) - Swift

```

# Method signatures:

```
objC
-(ReturnType)doSomethingWithArg:(Arg1Type)arg1Name
						otherArg:(Arg2Type)arg2Name
dash (-) denotes an instance method, if changed to a "+" that denotes a class method.

func doSomething(withArg arg1Name:Arg1Type,
				otherArg arg2Name: Arg2Type) -> ReturnType

```

# Types:

```
Swift: String, Array, Dictionary
In Swift these are structs which are value types

Objective C: NSString *, NSArray *, NSDictionary *
In ObjectiveC these are classes which are reference types. So in declaring them and whenever we declare a class we use the "*" which comes from C when dealing with pointers. 

What is with the NS part?
ObjectiveC doesn't have namespacing. As a work around type names in frameworks both from Apple and 3rd party developers are often prefixed with 2 or 3 letter abbreviations to distinguish themselves. 
NS stands for Next Step, the foundation framework since it's evolved from NextStep company.

other examples: UITableViewController, CGSize.

NSString * aString=@"Hello World";
NSArray * anArray=@[@"First", @"Second"];
NSDictionary * aDictionary = @{@"Key1":@"Value1",
								@"Key2:@"Value2"};

the "@" is used for ObjC literals. This distinguishes them from their C counterparts.

So if we don't wanna create a Dictionary with literal we can use an init method: 

NSArray *keyArray = @[@"key1", @"key2"];
NSArray *valueArray = @I@"valuel", @"value2"];

NSDictionary *dictionary = [[NSDictionary alloc] initWithObjects:valueArray
forKeys: keyArray];

to call an init method you have to allocate memory first. So you call "alloc" on the class and then you call init on that. 
```

# Mutability

There's a much bigger difference in ObjC between value types and reference types than there is in Swift. In ObjC value types are stuff but they can't "do" stuff. 

In Swift you can call methods on value types.  