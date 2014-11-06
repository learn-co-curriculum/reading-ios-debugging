#Debugging

If only we could write code without making a mistake!

The reality of the situation is, as human beings, we are prone to error. And numerous debugging tools have been built to help us determine and locate the source of computer "bugs". The first known mention of "debugging" comes from Grace Hopper, who invented the first compiler for a computer language. The story goes that a moth was found inside of a computer. But since that incident, "debugging" has come to be synonymous with the process of finding our own programming errors and fixing them.

But how do you get started? Imagine if when you attempted to compile your program, all that happened was that it crashed. What would you do? Go through hundreds if not thousands or tens of thousands of lines of code? How would you even know where to begin to look? These questions, and others, are accounted for by many of the basic tools we have been provided by our IDE, XCode.

##The Debug Console

The command center for debugging is called the "debug console". Combined with the "variable view" this entire section of the IDE is known as the "debug area". We will focus on the Debug Console and some of the tools available to you within it for the time-being. 

![](https://github.com/flatiron-school-curriculum/reading-ios-debugging/blob/master/screenWithoutDebugConsole.png)

To access the Debug Console, click the button on the upper left of your toolbar that shows the bottom third of the rectangle highlighted in blue. If you only see a single window, look down at the lower left corner of the IDE and check to see which of the two rectangles are highlighted in blue. We care about the one with the inner blue rectangle on the right.

![](https://github.com/flatiron-school-curriculum/reading-ios-debugging/blob/master/screenWithDebugConsole.png)



##`NSLog`

Now that we have our Debug Console showing, it's time to start using it.

One way in which we can take advantage of the console window is to log our variables to it. We do this using `NSLog` in our code.

How might this be helpful?

`NSLog` can be used in a few helpful ways:

1) A developer may place an `NSLog` statement at the beginning (or end) of each of their methods such that they can see the program run through it's various processes. This is usually overkill if used throughout the program but might be used selectively when evaluating loops or pieces of code you think might not be running when expected. (See the section called "Breakpoints" below for a more standard alternative.)

2) We might want to make sure a variable is being populated as expected (i.e. is not nil, or being populated with an unexpected value or data). While this can be done in the debug console (see the section called "LLDB: p / po" below), if we know we will want to regularly check to see how a variable has been populated, this is a more efficient manner in that we only have to type our `NSLog` once.

3) `NSLog` may be used to simply read a data sample downloaded from the web, in order to better understand a large dataset.

Why `NSLog` may not always be the right choice:

1) In production code, you do not want any `NSLog` statements. Therefore some developers may choose to go with other debugging methods to avoid having to do code cleanup later.

2) `NSLog` only gives you certain limited information, prior to runtime. If you are looking to further explore a variable at runtime beyond the initial `NSLog` code you wrote prior to compiling, you will have to use other methods.

Let's take a look at a few simple examples of `NSLog`.

######Example
```objc
NSLog(@"Check this out in our debug console!");
```

Check it out. Once we run our program, our debug console logs the statement.


######Debug Console Output
![](https://github.com/flatiron-school-curriculum/reading-ios-debugging/blob/master/example1.png)

But what if we want to inspect our variables.

######Example
```objc
NSString *helloWorld = @"Hello World";
NSLog(@"Our first `NSLog` statement that includes a variable, %@", helloWorld);
```

`NSLog` and `stringWithFormat` are very similiar in syntax. They both escape variables with the `%` followed by the appropriate letter to designate the type of variable that will be logged. For `NSString` and `NSNumber` that would be the `@` symbol. For other data types, it will vary, but fortunately for us, the XCode linter will show us an error that will even update the syntax appropriately for other types!


######Debug Console Output
![](https://github.com/flatiron-school-curriculum/reading-ios-debugging/blob/master/example2.png)

You may include as many variables in your `NSLog` statement as you wish.

######Example
```objc
NSString *helloWorld = @"Hello World";
NSNumber *aNumber = @42;
NSArray *anArray = @[@"Hello",@"World"];
CGFloat aFloat = 3.14159;

NSLog(@"Let's log a string: %@ and a number: %@ and an array: %@ and a float:%f", 
helloWorld, aNumber, anArray, aFloat);
```

######Debug Console Output
![](https://github.com/flatiron-school-curriculum/reading-ios-debugging/blob/master/example3.png)



##Breakpoints

Breakpoints are an essential debugging tool. With breakpoints, the developer can interrupt a program that is running code, and make one of three basic decisions to:

A) Continue running the program after pausing the code (until any other breakpoint is hit or the application terminates).

B) Step "over" a line of code such that the program only runs one line at a time.

C) Step "into" a line of code, effectively letting the developer see the details of the code being run. A classic example of this would be a line of code that runs a method. If the developer steps "into" this line of code, s/he will be brought to the implementation of the method that is being called.

Why is this useful?

While "paused" at a breakpoint, the developer can inspect the current "state" of the code. By state, we mean the current values assigned to variables, from simple `NSNumber` / `NSString` objects all the way up to objects that inherit from `UIViewController`.

###All Exception Breakpoint

There is an incredible array (no pun intended) of ways to take advantage of breakpoints, but one of the most important to have at your fingertips early and often is the "All Exceptions" breakpoint. First, a quick primer on how to turn it on.

Head to the left side of your screen where you find your "Navigators" and choose the "Breakpoint Navigator"

![](https://github.com/flatiron-school-curriculum/reading-ios-debugging/blob/master/breakpoints1.png)

Then go to the bottom left corner of the screen and click on the "+" icon. When you do this you will have a few options in a small pop-up window. 

![](https://github.com/flatiron-school-curriculum/reading-ios-debugging/blob/master/breakpoints2.png)

Choose "Exception Breakpoint".

![](https://github.com/flatiron-school-curriculum/reading-ios-debugging/blob/master/breakpoints3.png)

With an "All Exceptions" breakpoint, in most cases when you hit a bug in your code, the code will break at the point of the bug, and you will be able to inspect the state of the program at that point. However, if you do not see messages in your debug console, take note of the location, and then continue running the program until the messages appear. Then go back to the code at which the breakpoint "tripped", and read the message in the context of that code.

###How to step over, step in, and continue running after a breakpoint

Just above the top left of the Variable View (or Debug Console, if you do not have the Variables View open), you will see a series of icons. These is your breakpoints command center, where you can navigate through your breakpoints.

![](https://github.com/flatiron-school-curriculum/reading-ios-debugging/blob/master/breakpointCommandCenter.png)

These five buttons accomplish the following when pressed, in order from left to right:

1) Toggle disable / enable your breakpoints throughout your project (⌘Y will do the same.) When the "chevron" symbol is blue, our breakpoints are enabled. If disabled, the chevron will change color to gray.

2) Toggle pause / play your breakpoints throughout your project (CTRL-⌘Y will do the same.) Keep in mind that most of the time your program will run too quickly for you to pause it; so traditionally you will see the "play" icon in this location instead of the "pause" icon when you are running your app and it has stopped at a breakpoint.

3) Step over: As discussed earlier, this will evaluate the line of code and take you to the next line. (F6 key does the same.)

4) Step into: As discussed earlier, this will take us through the process of evaluating a line of code via its parts. (F7 key does the same.)

5) Step out of: Not previouslt discussed, but gets us back to the base line of code that we had previously stepped into. (F8 key does the same.) JOE - BETTER WAY TO SAY THIS?


## LLDB: p / po

So you've added breakpoints and your program has appropriately been interrupted. But what now?

LLDB (the debugger) has two commands that are going to become your new best friends. 

### po
Most frequently used is the `po` command. With `po` you can print the description of an object. If your object's description method has not been defined with an `NSString`, when you `po` the object, you will get a return value of it's memory address. For the time-being, let's steer clear of memory addresses. We will discuss them below.

So assuming you have a class with a valid `description` method, you will receive a string-based description of the variable you are inspecting.


#######Example

```objc
//House.h

#import <Foundation/Foundation.h>

@interface House : NSObject

@property (strong, nonatomic) NSString *address;

@end
```

```objc
//House.m

-(NSString *)description {
    return self.address;
}
```

```objc
//AnotherClass.m

- (void)setupHome {
	
	House *myHouse = [[House alloc] init];
    myHouse.address = @"11 Broadway, New York, NY 10004";
}
```

######Debug Console Output
![](https://github.com/flatiron-school-curriculum/reading-ios-debugging/blob/master/po.png)

### p
With an object that implements a description, you can use `p` to print the memory address of that object. Memory addresses are explained in more detail below.

######Debug Console Output
![](https://github.com/flatiron-school-curriculum/reading-ios-debugging/blob/master/p.png)

Combined, these tools are invaluable. Breakpoints + LLDB allow us to effectively `NSLog` our program at runtime! We can see the order in which our code runs and determine where some of its weaknesses may be.

## Memory addresses

As you pass variables around your program, you may want to make sure that the variable is actually the same variable. For instance, you might call the variable different things in different places. If the variable is a `House`, and you are attributing the `House` object to two `Person` objects (you and your mother), the `Person` object that is your mother might refer to the `House` object as `myHouse`; you might refer to it as `myMothersHouse`. How would you know for sure these objects are the same.

When you `po` an object such as `myHouse`, you get back the result of the `description` method. In addition, you also get back a memory address. A memory address is a unique identifier for a specific instance of an object.

######Example
```objc
	//Assumes there is already a House class and a Person class that had a House property.

    House *myHouse = [[House alloc] init];
    myHouse.address = @"11 Broadway, New York, NY 10004";
    
    Person *mother = [[Person alloc] init];
    mother.house = myHouse;
    
    House *myMothersHouse = myHouse;
    
    Person *me = [[Person alloc] init];
    me.house = myMothersHouse;
    
    House *myFriendsHouse = [[House alloc] init];
    
    Person *myFriend = [[Person alloc] init];
    myFriend.house = myFriendsHouse;
```

######Debug Console Output
![](https://github.com/flatiron-school-curriculum/reading-ios-debugging/blob/master/memoryAddresses.png)

Therefore, by `po`ing (new word!) `myHouse` and `myMothersHouse`, you could compare the memory addresses and see if they are the same. As you might imagine, this becomes quite useful when you are trying to trace the source of an error coming from a variable's value.

###Next steps

There are many more useful methods and tools for debugging, from advanced breakpoints and Instruments to third party plug-ins like Facebook's Chisel. Once you become more familiar with the basics, don't hesitate to check out some of the other LLDB tools!