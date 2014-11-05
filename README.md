#Debugging

If only we could write code without making a mistake!

The reality of the situation is, as human beings, we are prone to error. And numerous debugging tools have been built to help us determine and locate the source of computer "bugs". The first known mention of "debugging" comes from Grace Hopper, who invented the first compiler for a computer language. The story goes that a moth was found inside of a computer. But since that incident, "debugging" has come to be synonymous with the process of finding our own programming errors and fixing them.

But how do you get started? Imagine if when you attempted to compile your program, all that happened was that it crashed. What would you do? Go through hundreds if not thousands or tens of thousands of lines of code? How would you even know where to begin to look? These questions, and others, are accounted for by many of the basic tools we have been provided by our IDE, XCode.

##The Debug Console

The command center for debugging is called the "debug console". Combined with the "variable view" this entire section of the IDE is known as the "debug area". We will focus on the Debug Console and some of the tools available to you within it for the time-being. 

![](https://github.com/flatiron-school-curriculum/reading-ios-debugging/blob/master/screenWithoutDebugConsole.png.png)

To access the Debug Console, click the button on the upper left of your toolbar that shows the bottom third of the rectangle highlighted in blue. If you only see a single window, look down at the lower left corner of the IDE and check to see which of the two rectangles are highlighted in blue. We care about the one with the inner blue rectangle on the right.

![](https://github.com/flatiron-school-curriculum/reading-ios-debugging/blob/master/screenWithDebugConsole.png.png)




##NSLog

Now that we have our Debug Console showing, it's time to start using it.

One way in which we can take advantage of the console window is to log our variables to it. We do this using `NSLog` in our code. Let's take a look at a few simple examples

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
NSLog(@"Our first NSLog statement that includes a variable, %@", helloWorld);
```

NSLog and `stringWithFormat` are very similiar in syntax. They both escape variables with the `%` followed by the appropriate letter to designate the type of variable that will be logged. For `NSString` and `NSNumber` that would be the `@` symbol. For other data types, it will vary, but fortunately for us, the XCode linter will show us an error that will even update the syntax appropriately for other types!


######Debug Console Output
![](https://github.com/flatiron-school-curriculum/reading-ios-debugging/blob/master/example2.png)

You may include as many variables in your NSLog statement as you wish.

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




###