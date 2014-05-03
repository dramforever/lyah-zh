Baby's first functions

In the previous section we got a basic feel for calling functions. Now let's try making our own! Open up your favorite text editor and punch in this function that takes a number and multiplies it by two.

    doubleMe x = x + x  

Functions are defined in a similar way that they are called. The function name is followed by parameters seperated by spaces. But when defining functions, there's a = and after that we define what the function does. Save this as baby.hs or something. Now navigate to where it's saved and run ghci from there. Once inside GHCI, do :l baby. Now that our script is loaded, we can play with the function that we defined.

    ghci> :l baby  
    [1 of 1] Compiling Main             ( baby.hs, interpreted )  
    Ok, modules loaded: Main.  
    ghci> doubleMe 9  
    18  
    ghci> doubleMe 8.3  
    16.6   

Because + works on integers as well as on floating-point numbers (anything that can be considered a number, really), our function also works on any number. Let's make a function that takes two numbers and multiplies each by two and then adds them together.

    doubleUs x y = x*2 + y*2   

Simple. We could have also defined it as doubleUs x y = x + x + y + y. Testing it out produces pretty predictable results (remember to append this function to the baby.hs file, save it and then do :l baby inside GHCI).

    ghci> doubleUs 4 9  
    26  
    ghci> doubleUs 2.3 34.2  
    73.0  
    ghci> doubleUs 28 88 + doubleMe 123  
    478  

As expected, you can call your own functions from other functions that you made. With that in mind, we could redefine doubleUs like this:

    doubleUs x y = doubleMe x + doubleMe y   

This is a very simple example of a common pattern you will see throughout Haskell. Making basic functions that are obviously correct and then combining them into more complex functions. This way you also avoid repetition. What if some mathematicians figured out that 2 is actually 3 and you had to change your program? You could just redefine doubleMe to be x + x + x and since doubleUs calls doubleMe, it would automatically work in this strange new world where 2 is 3.

Functions in Haskell don't have to be in any particular order, so it doesn't matter if you define doubleMe first and then doubleUs or if you do it the other way around.

Now we're going to make a function that multiplies a number by 2 but only if that number is smaller than or equal to 100 because numbers bigger than 100 are big enough as it is!

    doubleSmallNumber x = if x > 100  
                            then x  
                            else x*2   

this is you

Right here we introduced Haskell's if statement. You're probably familiar with if statements from other languages. The difference between Haskell's if statement and if statements in imperative languages is that the else part is mandatory in Haskell. In imperative languages you can just skip a couple of steps if the condition isn't satisfied but in Haskell every expression and function must return something. We could have also written that if statement in one line but I find this way more readable. Another thing about the if statement in Haskell is that it is an expression. An expression is basically a piece of code that returns a value. 5 is an expression because it returns 5, 4 + 8 is an expression, x + y is an expression because it returns the sum of x and y. Because the else is mandatory, an if statement will always return something and that's why it's an expression. If we wanted to add one to every number that's produced in our previous function, we could have written its body like this.

    doubleSmallNumber' x = (if x > 100 then x else x*2) + 1  

Had we omitted the parentheses, it would have added one only if x wasn't greater than 100. Note the ' at the end of the function name. That apostrophe doesn't have any special meaning in Haskell's syntax. It's a valid character to use in a function name. We usually use ' to either denote a strict version of a function (one that isn't lazy) or a slightly modified version of a function or a variable. Because ' is a valid character in functions, we can make a function like this.

    conanO'Brien = "It's a-me, Conan O'Brien!"   

There are two noteworthy things here. The first is that in the function name we didn't capitalize Conan's name. That's because functions can't begin with uppercase letters. We'll see why a bit later. The second thing is that this function doesn't take any parameters. When a function doesn't take any parameters, we usually say it's a definition (or a name). Because we can't change what names (and functions) mean once we've defined them, conanO'Brien and the string "It's a-me, Conan O'Brien!" can be used interchangeably. 
