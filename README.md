# The Debugging Process

*Errare humanum est,*  
*perseverare autem diabolicum,*  
*et tertia non datur.*  
—Seneca the Younger, *attributed.*

## Objectives

1. Know the importance of debugging and the origin of the term.
2. Review the layout of Xcode's debugging suite.
3. Learn how to use `NSLog()` for debugging.
4. Create, toggle, and remove breakpoints.
5. Set up the "All Exceptions" breakpoint.
6. Read the Variable Viewer and recognize memory addresses.
7. Use `p` and `po` in the Console Output Viewer.

## What Is Debugging?

"To err is human, to forgive divine." So goes the popular English quote borrowed by Alexander Pope in his *An Essay on Criticism*. The idea in literature originated with the ancient Greeks, appearing in different forms in the writings of Livy and Cicero, and later Augustine of Hippo. For as much as we as programmers are human, however, computers are anything but forgiving.

Software development is a precise form of art. Unlike poetry, music, or painting which each possess certain implicit tolerances for errors (which can actually be part of the art itself, e.g. Philip Glass), instructions for computers must be explicitly correct. The punctuation of English can be altered to change its meaning, the rhythm of music survives breaking a chord in dissonance, but a missing semicolon in Objective-C will crash the program.

To appropriate the thoughts above, "to debug is human, to compile divine."

### The Moth in the Machine

There's a popular misconception that "debugging" computer code borrows the image of killing insects, citing the case in 1947 when Grace Hopper discovered a moth in relay 70 of the Harvard University Mark II calculator. While programmers nowadays can be heard referring to "squashing bugs", this "[Moth in the Machine](http://www.computerworld.com/article/2515435/app-development/moth-in-the-machine--debugging-the-origins-of--bug-.html)" story is not the origin of the phrase. Ms. Hopper's own note from this moment, "first actual case of bug being found", belies a deeper history at work.

An earlier documented usage of the term resides with Thomas Edison, first appearing in his notebooks in 1876 about troubleshooting multiplexed signals over electrical wires. To quote Alexander Magoun and Paul Israel's [article at the IEEE Institute](http://theinstitute.ieee.org/technology-focus/technology-history/did-you-know-edison-coined-the-term-bug) on the matter:

> Grace Hopper did not invent the bug, but she did draw cartoons of gremlins that represented chads, or fragments, created when holes are made in her computer’s punched paper tapes. Like Edison, she was recalling the word’s older origins in the Welsh *bwg*, the Scottish *bogill* or *bogle*, the German *bögge*, and the Middle English *bugge*: the hobgoblins of pre-modern life, resurrected in the 19th century as, to paraphrase philosopher Gilbert Ryle, ghosts in the machine. Bugs are not only for today’s computers or software; for more than 100 years, they have represented the challenges of an imperfect world that engineers work to overcome.

It's a phrase that inherits more from scaring away evil spirits than from pest control! And it's quite applicable—when writing code, the devil really is in the details.

## The Debug Area

The "Debug area" is the command center for flushing out the gremlins from our code. As you should recall from the *Introduction to Xcode* reading earlier, the Debug area is attached to the bottom of the IDE window and governed by the middle of the three Workspace Configuration Buttons in the upper right corner.

![](https://curriculum-content.s3.amazonaws.com/reading-ios-debugging/screenWithoutDebugConsole.png)

*Clicking the middle Workspace Configuration Button will show or hide the Debug area.*

The Debug area consists of a toolbar and two subviews. The left subview is the Variable Viewer; the right subview is the Console Output Viewer. Any reference you see to the "console" or "debug console" means this Console Output Viewer located here.

![](https://curriculum-content.s3.amazonaws.com/reading-ios-debugging/screenWithDebugConsole.png)

A pair of configuration buttons in the lower right of the Debug area governs these two subviews. You can show either the Variable Viewer alone, the Console Output Viewer alone, or both together. The bar separating them can be adjusted by dragging it left or right.

## Your Friendly Neighborhood `NSLog()`

![][spiderman]

We've previously introduced `NSLog()` unbeknownst to you that it's one of the superheroes in the never ending fight against bugs! The `NSLog()` function grabs a variable or group of variables and prints an interpolated string to the console. It is Objective-C's form of `print` which is common to most programming languages.

So how exactly is this helpful in debugging? Here are a few ways an `NSLog()` can be the tool that you need:

1. It can be placed inside of a method body or within a loop or `if` statement to see if that particular place in code is ever reached, to see how a variable changes across each iteration of a loop, or to see how a `BOOL` is evaluating.

2. It can be used to check that a variable is being populated as expected (e.g. that it is not `nil`). While this can be done in the debug console, if you know that you will want to repeatedly check to see how a variable has been populated, an `NSLog` only needs to be typed once.

3. A series of them can be used to read the order in which different elements of your code are executed.

Let's take a look at a few simple examples of `NSLog()`.

```objc
NSLog(@"Check this out in our debug console!");
```
This will appear in the console looking similar to the image:

![](https://curriculum-content.s3.amazonaws.com/reading-ios-debugging/example1.png)

We can also inspect a variable by passing it into an `NSLog()` with a string formatter:

```objc
NSString *helloWorld = @"Hello World";
NSLog(@"Our first `NSLog` statement that includes a variable, %@", helloWorld);
```
This will appear in the console looking similar to the image:

![](https://curriculum-content.s3.amazonaws.com/reading-ios-debugging/example2.png)

You may format as many variables into your `NSLog()` as you wish:

```objc
NSString *helloWorld = @"Hello World";
NSNumber *aNumber = @42;
NSArray *anArray = @[@"Hello",@"World"];
CGFloat aFloat = 3.14159;

NSLog(@"Let's log a string: %@ and a number: %@ and an array: %@ and a float:%f", 
helloWorld, aNumber, anArray, aFloat);
```
This will appear in the console looking similar to the image:

![](https://curriculum-content.s3.amazonaws.com/reading-ios-debugging/example3.png)

There is no theoretical limit to the number of variables that can be passed into a single `NSLog()`, however the interpolated string can get difficult to read in the code so don't shy away from stacking `NSLog()`s that each only handle a single variable.

However, `NSLog()` may not always be the best choice. Since it only retrieves a limited amount of predetermined information. If you wish to actively poke around in your code as it's running, `NSLog()` won't quite cut it.

## Breakpoints

###### Point breaks are awesome, man!

![][pointbreak]

*Sorry Keanu, but we did mean to say "breakpoints".*

Breakpoints are the quintessential debugging tool. A breakpoint will interrupt a program that is running code and allow us to make one of three basic decisions to either:

1. "Continue" running the program after pausing the code until either  another breakpoint is hit or the application finishes (or "runs out"),
2. "Step over" the highlighted statement—this is like a "frame-forward" button when working with video: it advances the program execution by only a single statement,
3. "Step into" a method call—if the highlighted statement contains a method call, this will bring us to the implementation of the method being called that we can "step over" through.

The real power of breakpoints, and the primary advantage that they hold over `NSLog()` in certain situations, is that while "paused" at a breakpoint we can inspect the current "state" of the code. This means that we can assess the current values assigned to any variable currently in existence. It's like having a portal into our code, but we can only look through it when time is stopped.

![][trinity]

*Trinity, when hitting a breakpoint.*

Before we discuss how to look inside our running code, we first need to know how to pause our program when or where we want.

### Adding, Toggling, and Removing Breakpoints

The easiest way to add a breakpoint is to click in the Code Editor's "gutter" on the line number at which you wish to pause the program execution. Doing so will cause a blue chevron to appear in the gutter at that line number. This is the breakpoint indicator.

![][settingABreakpoint]

*A breakpoint set on line 24.*

Notice also that adding a breakpoint also adds a link to it in the Breakpoint Navigator in the Navigator Area. This is a quick way to get back to your breakpoint once you've navigated away from it. It's especially useful for working in large applications with many files.

We can toggle an established breakpoint by clicking on its chevron in either the gutter or the breakpoint navigator. Disabling a breakpoint will cause it to be skipped at runtime without removing it. Disabled breakpoints are noted with a bluish grey color.

![][disabledBreakpoint]

*A disabled breakpoint on line 24.*

Breakpoints can be removed by either dragging the chevron out of the gutter and releasing it (this is similar to the drag-to-remove action of dock items in OS X). Alternatively, you can select a breakpoint or a set of breakpoints in the Breakpoint Navigator pane and pressing the `delete` key. This is especially helpful when removing a large number of breakpoints from throughout your code once you've finished your debugging process.

**Note:** *It's good practice to remove breakpoints in your code once you're done with them. When working on large projects with other developers, it will keep your breakpoints from getting mixed in with those of your teammates and vice-versa.*

### The "All Exceptions" Breakpoint

There is an incredible array (pun intended) of ways to take advantage of breakpoints and using them effectively is a skill that you'll develop over time. One of the most valuable uses of breakpoints that's particularly helpful early on is the "All Exceptions" breakpoint; without an "All Exceptions" breakpoint added to your project, if your code encounters an error that it cannot resolve and subsequently crashes, Xcode will often highlight the location of the error as the `return UIApplicationMain(...);` line in your project's `main.m` file. Xcode rarely gets any less helpful than in this moment.

The "All Exceptions" breakpoint, however, will highlight the last line of code that ran before the application crashed. Combined with a skilled interpretation of the readout in the Console Output Viewer, this can lead to valuable insight about just what's going on in your code.

#### Activating the "All Exceptions" Breakpoint

Since the "All Exceptions" breakpoint is not activated by default, here's a quick walk-through on how to set it up in your projects:

Open the Breakpoint Navigator pane in the Project Navigator; it's the button in the selection bar indicated by a right-facing chevron.

![](https://curriculum-content.s3.amazonaws.com/reading-ios-debugging/breakpoints1.png)

In the bottom left corner of the Breakpoint Navigator, select the `+` ("plus") icon. A dialogue box with a few options will pop open. Select "Add Exception Breakpoint" from the list.

![](https://curriculum-content.s3.amazonaws.com/reading-ios-debugging/breakpoints2.png)

You should see the "All Exceptions" breakpoint added to the list in the Breakpoint Navigator pane.

![](https://curriculum-content.s3.amazonaws.com/reading-ios-debugging/breakpoints3.png)

##  The Debug Area Toolbar

At the top of the Debug area is a row of buttons:

![](https://curriculum-content.s3.amazonaws.com/reading-ios-debugging/breakpointCommandCenter.png)

Here's a table of what the icons mean and what they do:

| Icon | Name | Description |
|------|:----:|:------------|
|![][toolbar_hideDebugArea]| Hide Debug area | Allows you to close the Debug area without reaching across to the Workspace Configuration Buttons in the upper left corner |
|![][toolbar_toggleBreakpoints_on]| Toggle breakpoints | (Shortcut `⌘``Y`) Switches between running the program with or without breakpoints enabled (*blue fill is enabled, empty outline is disabled*). This allows you to run your program without disabling or deleting each individual breakpoint. |
|![][toolbar_continueExecution]| Continue program execution | (Shortcut `control``⌘``Y`) This symbol will replace the "Pause" symbol when a breakpoint is reached. Selecting it will continue past the current breakpoint. |
|![][toolbar_stepOver]| Step Over | Move ahead one statement only. This is like a "frame forward" button with video. |
|![][toolbar_stepIn]| Step In | Move into the implementation of the method call in the current statement (if applicable). |
|![][toolbar_stepOut]| Step Out | Leave the method implementation stepped into and return to the previous scope. |
|![][toolbar_viewDebugger]| Open view debugger | This is a tool you will use later. It is helpful for visualizing how UIViews layer on top of each other. |
|![][toolbar_locationSimulator]| Simulate location | Another tool that you may use later on when working with Core Location, this can feed prerecorded GPS data to the iOS Simulator at run time.

Keep in mind that when navigating with breakpoints at run time, there is no "reverse" direction. Your code cannot run backwards or even rewind! Once you pass any given point, you cannot return to it without stopping and restarting. While your program might execute the same method body or loop statement multiple times, you cannot return to that specific call of the method or iteration of the loop. 

## Using the Variable Viewer

![][encounteringBreakpoint]

*Encountering a breakpoint at run time.*

When encountering a breakpoint at run time, you'll notice that the Variable Viewer populates itself with current information about all the variables visible in the current scope. What's visible in the Variable Viewer can change as your execution moves through your code.

![][variableViewer]

*A closer look at the Variable Viewer.*

The search bar at the bottom right of the Variable Viewer can be used to filter what appears. This is useful primarily in large applications populated with a large number of variables.

![][variableFilter]

*Using the search bar to filter out unimportant variables.*


### Memory Addresses

If you're wondering what the notation following the `self = (FISAppDelegate *)` variable is in the image above, that's a memory address. The value `0x7fe32052e410` is a [hexadecimal](https://en.wikipedia.org/wiki/Hexadecimal) number describing where in the computer's RAM the specified variable is stored.

**Advanced:** *Assigned a memory address is part of the functionality performed by* `+alloc` *when initializing a variable.*

Reading memory addresses allows us to disambiguate between different instances of the same type in our code, and especially ones that are not given specific names in our code.


## Using The Console Output Viewer

In addition to reading out the content of `NSLog()`s and system messages, the Console Output Viewer also runs the debug program LLDB which responds to a few interactive commands when stopped at a breakpoint. The two most useful and common commands are `p` ("print") and `po` ("print object").

**Top-tip:** *You'll notice that autocomplete works for the variables and their properties visible in the current scope. If you're typing a variable name that doesn't autocomplete, it probably isn't visible.*

#### `p`

This command prints the designated variable along with its class type and the value it holds.

#### `po`

In many cases, this will be equivalent to the value the object holds but it may also just print the objects memory address. Technically, this command is accessing the description property on the object, so if that property isn't set to anything `po` just prints the memory address.

![][usingDebugConsole]


[pointbreak]: https://curriculum-content.s3.amazonaws.com/reading-ios-debugging/pointbreak.gif
[spiderman]: https://curriculum-content.s3.amazonaws.com/reading-ios-debugging/spiderman.gif
[trinity]: https://curriculum-content.s3.amazonaws.com/reading-ios-debugging/trinity.gif

[disabledBreakpoint]: https://curriculum-content.s3.amazonaws.com/reading-ios-debugging/disabledBreakpoint.png
[encounteringBreakpoint]: https://curriculum-content.s3.amazonaws.com/reading-ios-debugging/encounteringBreakpoint.png
[settingABreakpoint]: https://curriculum-content.s3.amazonaws.com/reading-ios-debugging/settingABreakpoint.png

[toolbar_continueExecution]: https://curriculum-content.s3.amazonaws.com/reading-ios-debugging/toolbar_continueExecution.png
[toolbar_hideDebugArea]: https://curriculum-content.s3.amazonaws.com/reading-ios-debugging/toolbar_hideDebugArea.png
[toolbar_locationSimulator]: https://curriculum-content.s3.amazonaws.com/reading-ios-debugging/toolbar_locationSimulator.png
[toolbar_stepIn]: https://curriculum-content.s3.amazonaws.com/reading-ios-debugging/toolbar_stepIn.png
[toolbar_stepOut]: https://curriculum-content.s3.amazonaws.com/reading-ios-debugging/toolbar_stepOut.png
[toolbar_stepOver]: https://curriculum-content.s3.amazonaws.com/reading-ios-debugging/toolbar_stepOver.png
[toolbar_toggleBreakpoints_on]: https://curriculum-content.s3.amazonaws.com/reading-ios-debugging/toolbar_toggleBreakpoints_on.png
[toolbar_viewDebugger]: https://curriculum-content.s3.amazonaws.com/reading-ios-debugging/toolbar_viewDebugger.png

[variableFilter]: https://curriculum-content.s3.amazonaws.com/reading-ios-debugging/variableFilter.png
[variableViewer]: https://curriculum-content.s3.amazonaws.com/reading-ios-debugging/variableViewer.png

[usingDebugConsole]: https://curriculum-content.s3.amazonaws.com/reading-ios-debugging/usingDebugConsole.png

