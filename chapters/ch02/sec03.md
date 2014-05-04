# 2.3 An intro to lists

Much like shopping lists in the real world, lists in Haskell are very useful. It's the most used data structure and it can be used in a multitude of different ways to model and solve a whole bunch of problems. Lists are SO awesome. In this section we'll look at the basics of lists, strings (which are lists) and list comprehensions.

In Haskell, lists are a **homogenous** data structure. It stores several elements of the same type. That means that we can have a list of integers or a list of characters but we can't have a list that has a few integers and then a few characters. And now, a list!
> Note: We can use the `let` keyword to define a name right in GHCI. Doing `let a = 1` inside GHCI is the equivalent of writing `a = 1` in a script and then loading it.

    ghci> let lostNumbers = [4,8,15,16,23,42]  
    ghci> lostNumbers  
    [4,8,15,16,23,42]  

As you can see, lists are denoted by square brackets and the values in the lists are separated by commas. If we tried a list like `[1,2,'a',3,'b','c',4]`, Haskell would complain that characters (which are, by the way, denoted as a character between single quotes) are not numbers. Speaking of characters, strings are just lists of characters. `"hello"` is just syntactic sugar for `['h','e','l','l','o']`. Because strings are lists, we can use list functions on them, which is really handy.

A common task is putting two lists together. This is done by using the `++` operator.

    ghci> [1,2,3,4] ++ [9,10,11,12]  
    [1,2,3,4,9,10,11,12]  
    ghci> "hello" ++ " " ++ "world"  
    "hello world"  
    ghci> ['w','o'] ++ ['o','t']  
    "woot"  

Watch out when repeatedly using the `++` operator on long strings. When you put together two lists (even if you append a singleton list to a list, for instance: `[1,2,3] ++ [4]`), internally, Haskell has to walk through the whole list on the left side of `++`. That's not a problem when dealing with lists that aren't too big. But putting something at the end of a list that's fifty million entries long is going to take a while. However, putting something at the beginning of a list using the `:` operator (also called the cons operator) is instantaneous.

    ghci> 'A':" SMALL CAT"  
    "A SMALL CAT"  
    ghci> 5:[1,2,3,4,5]  
    [5,1,2,3,4,5]  

Notice how `:` takes a number and a list of numbers or a character and a list of characters, whereas `++` takes two lists. Even if you're adding an element to the end of a list with `++`, you have to surround it with square brackets so it becomes a list.

`[1,2,3]` is actually just syntactic sugar for `1:2:3:[]`. `[]` is an empty list. If we prepend `3` to it, it becomes `[3]`. If we prepend `2` to that, it becomes `[2,3]`, and so on.

> Note: `[]`, `[[]]` and`[[],[],[]]` are all different things. The first one is an empty list, the seconds one is a list that contains one empty list, the third one is a list that contains three empty lists.

If you want to get an element out of a list by index, use `!!`. The indices start at 0.

    ghci> "Steve Buscemi" !! 6  
    'B'  
    ghci> [9.4,33.2,96.2,11.2,23.25] !! 1  
    33.2  

But if you try to get the sixth element from a list that only has four elements, you'll get an error so be careful!

Lists can also contain lists. They can also contain lists that contain lists that contain lists …

    ghci> let b = [[1,2,3,4],[5,3,3,3],[1,2,2,3,4],[1,2,3]]  
    ghci> b  
    [[1,2,3,4],[5,3,3,3],[1,2,2,3,4],[1,2,3]]  
    ghci> b ++ [[1,1,1,1]]  
    [[1,2,3,4],[5,3,3,3],[1,2,2,3,4],[1,2,3],[1,1,1,1]]  
    ghci> [6,6,6]:b  
    [[6,6,6],[1,2,3,4],[5,3,3,3],[1,2,2,3,4],[1,2,3]]  
    ghci> b !! 2  
    [1,2,2,3,4]   

The lists within a list can be of different lengths but they can't be of different types. Just like you can't have a list that has some characters and some numbers, you can't have a list that has some lists of characters and some lists of numbers.

Lists can be compared if the stuff they contain can be compared. When using `<`, `<=`, `>` and `>=` to compare lists, they are compared in lexicographical order. First the heads are compared. If they are equal then the second elements are compared, etc.

    ghci> [3,2,1] > [2,1,0]  
    True  
    ghci> [3,2,1] > [2,10,100]  
    True  
    ghci> [3,4,2] > [3,4]  
    True  
    ghci> [3,4,2] > [2,4]  
    True  
    ghci> [3,4,2] == [3,4,2]  
    True  

What else can you do with lists? Here are some basic functions that operate on lists.

`head` takes a list and returns its head. The head of a list is basically its first element.

    ghci> head [5,4,3,2,1]  
    5   

`tail` takes a list and returns its tail. In other words, it chops off a list's head.

    ghci> tail [5,4,3,2,1]  
    [4,3,2,1]   

`last` takes a list and returns its last element.

    ghci> last [5,4,3,2,1]  
    1   

`init` takes a list and returns everything except its last element.

    ghci> init [5,4,3,2,1]  
    [5,4,3,2]   

If we think of a list as a monster, here's what's what.

![list monster](http://drops.illumer.org/usr/uploads/2014/05/2004517552.png)

But what happens if we try to get the head of an empty list?

    ghci> head []  
    *** Exception: Prelude.head: empty list  

Oh my! It all blows up in our face! If there's no monster, it doesn't have a head. When using `head`, `tail`, `last` and `init`, be careful not to use them on empty lists. This error cannot be caught at compile time so it's always good practice to take precautions against accidentally telling Haskell to give you some elements from an empty list.

`length` takes a list and returns its length, obviously.

    ghci> length [5,4,3,2,1]  
    5  

`null` checks if a list is empty. If it is, it returns True, otherwise it returns False. Use this function instead of xs == [] (if you have a list called xs)

    ghci> null [1,2,3]  
    False  
    ghci> null []  
    True  

`reverse` reverses a list.

    ghci> reverse [5,4,3,2,1]  
    [1,2,3,4,5]  

`take` takes number and a list. It extracts that many elements from the beginning of the list. Watch.

    ghci> take 3 [5,4,3,2,1]  
    [5,4,3]  
    ghci> take 1 [3,9,3]  
    [3]  
    ghci> take 5 [1,2]  
    [1,2]  
    ghci> take 0 [6,6,6]  
    []  

See how if we try to take more elements than there are in the list, it just returns the list. If we try to take 0 elements, we get an empty list.

`drop` works in a similar way, only it drops the number of elements from the beginning of a list.

    ghci> drop 3 [8,4,2,1,5,6]  
    [1,5,6]  
    ghci> drop 0 [1,2,3,4]  
    [1,2,3,4]  
    ghci> drop 100 [1,2,3,4]  
    []   

`maximum` takes a list of stuff that can be put in some kind of order and returns the biggest element.

`minimum` returns the smallest.

    ghci> minimum [8,4,2,1,5,6]  
    1  
    ghci> maximum [1,9,2,3,4]  
    9   

`sum` takes a list of numbers and returns their sum.

`product` takes a list of numbers and returns their product.

    ghci> sum [5,2,1,6,3,2,5,7]  
    31  
    ghci> product [6,2,1,2]  
    24  
    ghci> product [1,2,5,6,7,9,2,0]  
    0   

`elem` takes a thing and a list of things and tells us if that thing is an element of the list. It's usually called as an infix function because it's easier to read that way.

    ghci> 4 `elem` [3,4,5,6]  
    True  
    ghci> 10 `elem` [3,4,5,6]  
    False  

Those were a few basic functions that operate on lists. We'll take a look at more list functions later
