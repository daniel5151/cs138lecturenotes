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
#### *first.cc*
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

Here is a brief summary of it:

Operation|What it does
--|--
`+` | string catenation
`==, <=, <, etc.	` |comparison	based	on	standard	lexical	ordering	
`s.length()` 	 |	number	of	chars	in	s		
`s.substr(i,n)`|substring	of	n	characters	starQng	at	index	i		
`s.substr(i)` |substr	starQng	at	index	i	and	ending	at	the	end	of	s		
`s.at(i)` 	|		returns	ith char	(checked	access)		
`s[i]` 	|			returns	ith	char	(unchecked	access)	
`s.c_str()` 	| 	returns	the	C-style	char*	equivalent	of	s		
`s.find(str, pos)`| return	first	index	of	str	in	s	starQng	at	or	aver	pos	\**	
`s.rfind(str, pos)` |return	last	index	of	str	in	s	starQng	at	or	before	pos	\**	
`s.replace(...)` |replace	chars	within	s		
`s.insert(...)`| insert	chars	within	s

\** returns -1 if str is not found (technically, that's string::npos)

Now, chars	have	a	dual	nature:	they're	both	characters	AND	integers:	
- When	you	use	the	"+"	operator	on	chars,	it	performs	integer	
addiQon,	not	string	catenaQon.		
- …	but	when	you	print	a	char,	you	get	the	character	not	the	integer.

Take the following example code:

```cpp
    #include <iostream>
    #include <string>

    using namespace std;

    int main (int argc, char* argv[]) {
        cout << "m" << endl;                       // m
        cout << 'm' << endl;                       // m
        cout << (int) 'm' << endl;                 // 109
        cout << "m" + "g" << endl;                 // Won't compile.
        cout << (string) "m" + (string)"g" << endl; // mg
        cout << (string) "m" + 'g' << endl;        // mg
        cout << 'm' + 'g' << endl;                 // 212=109+103
        return 0;
    }
```

## Lecture 2 - Jan 7th
### Command Line Arguments
If `main` is given the arguments `(int argc, char* argv[])`, the OS will automagically fill them in with the command line arguments to the program!
- `argc` is an `int` on the number of arguments given to the program
	- minimum of one, as `argv[]` always contains at least the call of the program itself
- `argv` is an array of c-strings that correspond to the given command line arguments.

#### *argfun.cc*
```cpp
    // Try running this with different #s of command-line args
    #include <iostream>
    #include <string>

    using namespace std;

    int main (int argc, char* argv[]) {
        cout << "argc = " << argc << endl;

        for (int i=0; i<argc; i++) {
            cout << "argv[" << i << "] = \"" << argv[i] << "\"\n";
        }
        // Now some goofing around … string vs. char*
        string s = argv[0]; // implicit type conversion
        cout << "as a string: " << s << endl;
        s += " hi mom";

        // c_str returns const char* by default, so
        // must cast to char* to send back to argv[0]
        argv[0] = (char*) s.c_str();
        cout << "argv[0] = " << argv[0] << endl;

        return 0;
    }
```

Notice that there is a magical type conversion when doing something such as `string s = argv[0]`. C++ will automagically convert `argv`'s into strings (NOT OTHER C-STRINGS!).

### Simple IO
`#include <iostream>` gives you access to 3 streams:
- `cin` - Standard Input
- `cout` - Standard Output
- `cerr` - Standard Error

The `<<` operator is for Output, and `>>` is for Input.
These are overloaded operators in respect to these streams (i.e they have other uses in other contexts (like bitshifting and such)).

Here is a quick example of basic IO:
``` cpp
	#include <iostream>
    #include <string>

    using namespace std;

    int main (int argc, char* argv[]){
        cout << "Hello world!" << endl;
        cout << "pi is approx. " << 22/double(7) << endl;

        string name;
        int age;

        cout << "What's your name and age? ";

        cin >> name >> age; // reads next two input tokens

        if (age < 0) {
            cerr << "Error, age must be non-negative.\n";
        } else {
            cout << "So, " << name << ", how's being " << age << " like?";
        }

        return 0;
    }
```

### Formatted Output
Just google it.
There are many ways to do it.
Take your pick.

You don't need any text formatting libs for assignment 1 by the way.

### Reading Input
To read single tokens, just use `cin >> foo`.
This will discard whitespace (spaces, tabs, newlines), and only return tokens.

To read full lines, use `string s; getline(cin, s)`.
This will read full lines into s.

A few notes about `getline`:
- You'll	have	to	parse	the	input	string	yourself	(e.g.,	line.find(…)) to extract tokens.
- The	newline	char	is	removed	from	the	input	stream	but	**NOT**	stored	in the	variable	line.
- `getline`	works	with	other	kinds	of	input	streams	too,	not	just	cin!

### How to detect end of input
1) The old way was to be given the ammount of data to be read, and manually keep track of that. But that sucks.

2) Using a dummy variable and seeing if anything has been read to it (also kind of sucks)

**3)** Use `cin.fail()` or `cin.eof()`, flags which are automatically set on reaching the end of the file.
``` cpp
	#include <iostream>

    using namespace std;

    int main (int argc, char* argv[]) {
    	double sum = 0;
        int count = 0;
        while (true) {
        	double next;
            cin >> next;
            if (cin.fail()) { // or cin.eof()
            	break;
            }
            sum += next;
            count++;
        }
        if (count > 0){
        	cout << "Avg is " << sum/count << endl;
        }
        return 0;
    }
```

**Subtle	detail:**		eof()/fail()	don't	return	true	*until	you've tried	to	go	one	step	too	far!*
You	need	to	check	right	after	an	attempted	read, but	before using	the	variable	you	tried	to	read	into (as done in the code above)

**4)** A slightly more idomatic approach:
``` cpp
	#include <iostream>

    using namespace std;

    int main (int argc, char* argv[]) {
    	double sum = 0;
        int count = 0;

        double next;
        while (cin >> next) { // shoter, but kind of obtuse...
            sum += next;
            count++;
        }
        if (count > 0){
        	cout << "Avg is " << sum/count << endl;
        }
        return 0;
    }
```

### Reading and Writing to files directly

`#inlcude <fstream>` allows you to set up filestreams.
After setting up the file, the stream variable is used **equivalently to `cin` / `cout`**

Here is a quick example that copies the words from one file into another, adding "lmao" to each word.
#### *ayllmao.cpp*
```cpp
	#inlcude <fstream>
    #include <iostream>

    int main() {
    	string filename = "in.txt";

        ifstream is_intxt (filename); 
        // Do note that the line above
        // implicitly calls is_txt.open(filename) at
        // construction

        if (!is_intxt) {
        	cerr << "Can't open file!" << endl;
            return 0;
        }

        ofstream os_outtxt ("out.txt");
        // should also check, but YOLO

        string word;
        while (is_intxt >> word) {
        	os_outtxt << word << "lmao" << endl;
        }

        is_intxt.close()
        os_outtxt.close()
    }
```

Here is an example showing all of the many inputs and outputs one can access:
```cpp
    // Example adapted from Adam Roegiest
    #include<string>
    #include<iostream>
    #include<fstream>

    using namespace std;

    void printAnswer (ostream & output, string answer){
    	output << "The answer is " << answer << endl;
    }

    int main (int argc, char* argv[]) {
        printAnswer (cout, "42");
        printAnswer (cerr, "clean living");
        ofstream myout("foo.txt");
        if (myout) {
        	// print to the file now
        	printAnswer (myout, "blowing in the wind");
        }
        return 0;
    }
```

### Arrays in C\++
C\++ arrays are nearly identical to C arrays.
Use them as such.
If interested, look up the differences, but i'm not getting into C\++ arrays because most of the time, it's easier to just use...

### Vectors!
`#inlcude <vector>` gives you access to the magical `vector<T>` data structure!
Vectors are essentially **Dynamic Arrays!** (but they still only hold one data type).

**Advantages**	over	using	an	C-style	array:			
- Declared	size	can	be	an	integer	variable,	not	just	a	compile-time constant
- Safe	accessing	(bounds	checking)	via	`at()`
- Can	ask	for	size	via	`size()`

But, as with all good things, there are some **Disadvantages**:
- Bounds	checking	can	slow	access	(but	you	don't	have	to	use	it)
- Very	mild	space	overhead	to	use	an	object.

BUT, if you know that you won't be growing / shrinking the array, then just use	the	**C\++11	array	class** instead.


Vectors really shine when being used to implement a **Stack**.
Briefly, a stack is a data structure where you can **push** elements to the "top" of the stack, and **pop** elements off the "top"

Check out this example code:

#### *vectors.cpp*
```cpp
    #include <iostream>
    #include <vector>

    using namespace std;

    int main() {
        vector<int> v;
        for (int i = 0; i < 10; i++) {
            cout << "size = " << v.size()
                 << " capacity = " << v.capacity() << endl;
            v.push_back(i);
        }
        while (v.size() != 0) {
            int i = v.back();
            cout << "Popping " << i << endl;
            v.pop_back();
        }
        return 0;
    }
```

Output:
```
    size = 0 capacity = 0
    size = 1 capacity = 1
    size = 2 capacity = 2
    size = 3 capacity = 4
    size = 4 capacity = 4
    size = 5 capacity = 8
    size = 6 capacity = 8
    size = 7 capacity = 8
    size = 8 capacity = 8
    size = 9 capacity = 16
    Popping 9
    Popping 8
    Popping 7
    Popping 6
    Popping 5
    Popping 4
    Popping 3
    Popping 2
    Popping 1
    Popping 0
```

Here is a small table of important bits of the vector API:
Operation | What it does
----------|------------
**v.at(i)** 			 | returns	i^th^ element	(index	checked)		
**v[i]** 				 | returns	i^th^	element	(index	unchecked)
**v.size()**  | returns	#	of	elements	v	has		
v.capacity() |  returns	#	of	elements	v	currently	has	space	for		
v.empty() |  returns	true	iff	v	has	no	elements		
v.resize(int)  | reset	size	to	exactly	n,	deleQng/padding	as	needed		
v.reserve(int)  | reset	capacity	to	max	(n,	oldCapacity)	
**v.push_back(..)**  | add	elt	onto	the	end	of	v	(will	increment	capacity	if	neccessary)		
**v.pop_back(..)** |  remove	last	element	from	end	of	v		
v.front() |  returns	reference	to	first	element	of	v	(not	very	useful)	
**v.back()** |  returns	reference	to	last	element	of	v		
v.begin()  | returns	an	iterator	poinQng	to	first	element	of	v	
v.end() |  returns	an	iterator	poinQng	aver	last	element	of	v		

\***Bolded** are REALLY important to know

This doesn't even scrape the surface of how to use `vector<T>`, so I reccomend looking up the documentation / stackoverflow!

One last thing.
There are 2 main ways to iterate over a vector.
Look at this example code:
```cpp
    #include <iostream>
    #include <string>
    #include <vector>

    using namespace std;

    int main (int argc, char* argv[]) {
        vector<string> v;

        v.push_back("hello");
        v.push_back("there");
        v.push_back("world");

    	// It's fine to do this, for now
        for (int i=0; i<v.size(); i++) {
        	cout << "v[" << i << "] = \"" << v[i] << "\"\n";
        }

        v.pop_back();
        v.pop_back();
        v.push_back("eh");

		// This is more advanced; you don't have to do this yet
        for (vector<string>::const_iterator ci = v.begin(); 
        	 ci != v.end();
             ci++) {
            cout << (*ci) << endl;
        }
        return 0;
    }
```

If you're anything like me, you're probably a bit confused about approach 2.
Good thing we are going to learn about it later!
Just stick with approach 1 for now.

### Unix Shell
We talked a bit about how to use the UNIX shell and it's history.
I didn't take notes, because in the words of Lex Murphy from Jurrassic Park:
<a href="https://www.youtube.com/watch?v=dFUlAQZB9Ng">*"This is a UNIX System... I know this!"*</a>

This stuff is testable though, so brush up on your basic UNIX commands, command line useage, **AND HISTORY OF UNIX**.

The slides are good for this.
Here is a link:
https://www.student.cs.uwaterloo.ca/~cs138/current/LectureSlides2016/02-UnixNotes.pdf

## Lecture 3