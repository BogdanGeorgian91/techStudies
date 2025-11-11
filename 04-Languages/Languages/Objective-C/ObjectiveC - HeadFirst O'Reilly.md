
book 3rd edition (2014)

It’s like Java.
Object-oriented, with its roots in Smalltalk.

Since it’s a C-based language, all the syntax is the same as that in C for loops, types, pointers, etc. It comes from a long line of Apple heritage, starting with NeXTStep, which led to OpenStep, and finally CocoaTouch

It uses CocoaTouch frameworks.
If you’ve worked with Mac programming, you already know all about Objective-C, but there’s still a lot of iOS-specific things that you need to learn

Memory management can be automatic.
With iOS5, Apple finally introduced some automatic memory mangement tools, like in Java, called automatic reference counting (ARC). That means no more counting references to prevent memory leaks.

We wrote this book with Xcode 5.0.

Classes in your project are compiled
Time to talk files a little. An Objective-C class typically has a .h and a .m file, used as
the header and the implementation, respectively. A header file describes the public
interface to a class—its API. The implementation file is just that, where the meat goes
that makes the class do what it does. Those two get combined and compiled into binary
form during the build process.

Your UIs are processed and converted to a binary form
UIs in Xcode are usually built as storyboards and are XML documents that describe
what goes where. Xcode compiles those down into a binary format along with
optimizing any images, icons, etc. that your app uses to a format that’s easily renderable
by the target device.

Framework code is linked in
There’s lots of shared library code in an iOS app: UI drawing code (UIKit), mapping
support (MapKit), low-level drawing code (CoreGraphics), etc. Apple calls these shared
libraries frameworks. Frameworks are more than just shared compiled code like JARs
or .so files from other languages. In Objective-C, frameworks can include header files,
graphics resources, and more. Some frameworks, like MapKit, are optional and needed
only if your application uses them, whereas others, like CoreGraphics, are required
because every application relies on the code they contain.

Your compiled code, images, etc. get packaged up
If you’ve never worked with OS X or iOS applications before, SomeApp.app is
actually just a directory with specific contents. An iOS application is a folder with
a compiled executable, a number of configuration files, and any resources the app
needs to run.

If you’re installing on a device, the code is signed
Installation onto devices—everything from just testing on your personal device to
posting on the App Store—is controlled with certificates based on your developer
credentials. This is how the iOS ecosystem controls what can be installed on
devices. Certificates are managed in the organizer and you use provisioning profiles
to match up applications with devices.

Your app is installed on the simulator
The simulator acts a lot like a real device. It has a directory where your data is
stored, apps get their own directories by IDs, etc.

Your app is launched and the debugger is attached
Xcode will start up your application in the simulator and then attach
the debugger to it if you chose to debug the app (versus just running
it without debugging). We’ll see more of this shortly, but the console
will start logging events in the app and you can monitor it through a
panel in Xcode.

Your code is stored in source files
The actual code for the application is broken up into classes and each class is
made up of two files, a header (.h) file and an implementation (.m) file. The
header file (.h) is the public API to the class, whereas the implementation file
(.m) has all the meat. Our problem has to do with the data our app is showing,
not some issue with a class’s API.

```
- (void)viewDidLoad {
    [super viewDidLoad];
    
    self.activities = @[@"networking", @"coding", @"tweeting", @"wireframing",
                        @"working", @"meeting", @"pitching", @"pivoting", @"going mobile"];
    self.feelings = @[@"awesome", @"confident", @"smart", @"agile", @"friendly",
                      @"savvy", @"psyched", @"hopeful", @"efficient"];
}
```

![[viewDidLoad_method_objC.png]]

InstaTwitViewController.h is the header file for this
class; it declares the public interface, meaning the public
properties and actions for this class. Navigate over to the
header file and take a look if you’re curious.

# LAST PAGE LEFT

==page 32
