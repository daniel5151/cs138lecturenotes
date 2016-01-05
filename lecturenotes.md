# 
<h1 style="margin:auto">CS-138 Introduction to Data Abstraction</h1>
<h1 style="margin:auto">Notes by Daniel Prilik</h1>
<h1 style="margin:auto">Software Engineering 2020</h1>

#### Prof: Mike Godfrey
###### Email: migod@uwaterloo.ca
###### Office Hours: Thursdays from 11:30 to 12:30 in DC2340

#### TA: Tim Yatayev
###### Email: cs138@uwaterloo.ca
###### Office Hours: TBA


## Lecture 1 - Jan 6th
### Introduction
Lots of Boilerplate, nothing seriously relevant.

### C++ Boot Camp
This course doesn't teach you C\++. It teaches you how to make things with C\++.

Nevertheless, it's still important to know the language, so this is a short summary of what's changed / been added from C.

#### Class vs. Struct
Same thing, but *Structs* have all their elements **Public** by default, and *Classes* have all their elements **Private** by default.

Structs are better from procedural programming, Classes are mainly for OOP.

Also, don't mix and match (even though you can).

#### Style considerations:
- Readability is important. One liners may look cool, but they aren't really readable.
- Use for loops over while loops most of the time.
- Use curly braces, even when you don't need to (single liners).
- Testing infrastructure is important.

#### Sample Code
#### *first.cpp*
```cpp
	#include <iostream>
    #include <string>

    using namespace std;

    // globals
    const string kidDrink = "juice";
    string adultDrink = "coffee";

    int main (int argc, char* argv[]) {
    	int age = 100;
        while (age > 0) {
        	cout << "How old are you?";
            cin >> age;
            cout << "Would you like some ";
            if (age < 18) {
            	cout << kidDrink << "?" << endl;
            } else {
            	cout << adultDrink << "?" << endl;
            }
            if (age == 47) {
            	adultDrink = "beer"; // changes global var
            }
        }
        return 0;
    }
```

Notice:
- `using namespace std;` -- Use this, or else errors. Why? You'll learn.
- How input and output work.

#### Booleans
`bool` is a built in type in C\++. Use it, it's nice!
It lets you write `bool someBool = true;` or `bool someBool = false;`.
Nevertheless, C\++ still lets you treat 0 as False, and non-zero as True (like in C).

#### Chars and Strings
**DONT USE `char*` WHEN YOU CAN AVOID IT**.
C\++ has a better built in string library!
**READ ABOUT THE C\++ STRING LIBRARY. IT'S BALLER.**
There is a solid summary in the lecture slides.

## Lecture 2


