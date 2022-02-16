# OOPS

### Issues in Software\(s\):

Correctness, Maintainability, Reusability, Openness and interoperability, portability, security, integrity, user friendliness

### Evolution of Software\(s\):

Machine Language, Assembly Language, Procedure Language, Object-Oriented Programming.

* Procedure Oriented language emphasis is on doing things \(algorithms\)
* Large programs are divided into smaller programs known as functions.
* Most of the functions share global data.
* Data move openly around the system from function to function
* Functions transform data from one form to another.
* Employs top-down approach in program design.
* OOP is an approach to program organization and that attempts to eliminate some of the pitfalls of conventional programming methods by incorporating the best of structured programming features with several powerful new concepts.
* It is a new way of organizing and developing programs and has nothing to do with any particular language
* It models real world problem very well unlike Procedure oriented languages
* Emphasis is on data rather than procedure
* Programs are divided into what are known as objects.
* Data structures are designed such that they characterize the objects.
* Functions that operates on the data of an object are tied together in the data structure
* Data is hidden and cannot be accessed by external functions.
* Objects may communicate with each other through functions.
* New data and functions can be easily added whenever necessary
* Follows bottom-up approach in program design.

> Max no limit length in identifier. only a-z, A-Z, numbers \(not in beginning\) and \_

Basic terms - Objects, Classes, Data Abstractions & Encapsulation, Inheritance, Polymorphish, Dynamic Binding, Message Passing

### Objects

* Basic runtime entities in an Object oriented system
* It may represent a person, a place, a table of data or any item that the program has to handle.
* Objects interact by sending messages to one another.

### Classes

* A class is a collection of objects of similar type on which the object operates
* Could be a user defined data type

### Data Abstraction

* Wraping up of data and function into a single unit \(class\) is known as encapsulation
* Data is not accessible to the outside world
* Functions wrapped in class can only access it
* These functions provide the interface between object's data and the program.
* Abstraction refers to act of representing essential features without including the background details and explanations
* Since classes use the concept of data abstraction, they are known as ADT

![](../.gitbook/assets/image%20%28197%29.png)

### Inheritance

* It is the process by which objects of one class acquire the properties of objects of another class.
* It provides the idea of re usability. This means we can add additional features to an existing class without modifying it.
* Reuse existing class along with tailor it in such a way that it does not introduce any undesirable side-effects into the rest of classes.

### Polymorphism

* Greek word meaning ability to take more than one form
* The operation may exhibit different behavior in different instances. Behavior depends upon the type of data used in operation.
* Operator Overloading & Functions Overloading
* It plays an important role in allowing objects having different internal structures to share the same external interface.
* Polymorphism is extensively used in implementing inheritance
* It is early binding or static binding or static linking. \(Compile Time Polymorphism\)

#### Dynamic Binding

* Dynamic binding or late binding is the mechanism a computer program waits until runtime to bind the name of a method called to an actual subroutine
* Binding refers to the linking of a procedure call to the code to be executed in response to the call.
* Code associated with given procedure call is not known until the time of the call at run-time.
* Runtime-Polymorphism.

| Compile Time Polymorphism | Run Time Polymorphism |
| :--- | :--- |
| Call resolved by compiler | Call not resolved by compiler |
| Also known as static binding, early binding, overloading | Also known as Dynamic binding, late binding, overriding |
| Overloading is compile time polymorphism where more than one methods share the same name with different parameters or signature and return type | Overriding is run time polymorphism having same method with same parameter or signature but associated in a class or it's subclass |
| It is achieved by function overloading & operator overloading | It is achieved by virtual functions |
| It provides fast execution because known early at compile time | Slow execution |
| It is less flexible as all things execute at compile time | More flexible |

### Benefits Of OOPS

* Through inheritance we can eliminate redundant code and extend the use of existing classes.
* We can build programs from the standard working modules the communicate whith one another, rather than having to start writing everything from scratch.
* The principle of data hiding helps the programmer to build secure programs.
* It is possible to have multiple instances of an object to co-exist without any interference.
* The data centric design approach enables us to capture more details of a model.
* Object oriented system can be easily upgraded from small to large systems
* Message passing techniques for communication between objects makes the interface description with external systems much simpler.
* Software complexity can be easily managed.

> Object-based programming \(ADA\) do not support inheritance and dynamic binding but object oriented programming do so

> Member functions \(Functions inside a class\) are also known as methods

> In C main\(\) by default returns the void type but in C++ it returns integer by default

> Old C has /\* \*/ while in C++ //

> Zero divided by zero error is an example of runtime error. Will stll compile. Error at runtime. There's compiler error if during compilation. Lastly there's a linker error.

> Farenheit to celcius = \(\(f-32\)/9\)\*5

Tokens are the smallest individual units in a program. C++ has following tokens - Keywords, Identifiers, Constants, Strings, Operators

> C 32 keywords C++ 64 keywords

```text
enum color { RED, BLUE, GREEN, YELLOW };
//RED, BLUE = 5, GREEN = 6, YELLOW then RED is 0 Yellow is 7
//RED = 3, BLUE, GREEN, YELLOW then automatically 4, 5, 6, 7
color bg = BLUE;
color bg = 1;           //Error in c++ okay in C
color bg = (color) 1;
int bg = BLUE;
```

```text
const int MAX_AGE = 90;
const int* a = new int;
//or int const* a = new int;
*a = 2;                 //ERROR since a is constant
a = (int*)&MAX_AGE;     //No Error now
//Can't change content of the pointer here but can change pointer address

//Constant Pointer
const int MAX_AGE = 90;
int* const a = new int;
*a = 2;                 //No Error here
a = (int*)&MAX_AGE;     //ERROR since pointer of a is constant
//Can't change the pointer address here

const int* const a = new int;
//can't change anything now
```

C++ new operators -

* :: \(Scope resolution operator\)
* ::\* \(To declare pointer to a member of a class\) - Member derefrencing operator
* \* \(To access a member using object name and pointer to that member\) - Member derefrencing operator
* -&gt;\* \(To access a member using a pointer to the object and a pointer to that member\) - Member derefrencing operator
* .\* \(Pointer-to-member operator\)
* delete \(Memory release operator\)
* new \(Memory allocation operator\)
* endl \(Line feed operator\)
* setw \(Field width operator\)

> A void pointer can be used to create generic pointer

> Expressions - Combination of constant, operator & variables it may also include function call that may return value. Types - Constant expression, Integral expression, float expression, pointer expression, relational expression, bitwise expression, logical expression

```text
int m = 10;
int main()
{
    int m = 20;
    {
        int k = m;
        int m = 30;
        // k is 20 m is 30 ::m is 10
    }
    // m is 20 ::m is 10
}
```

typedef int int32; can be declared at the beginning in case the architecture has 4 bytes of int if say on other it's 2 and we want 4 we can replace int with long and at every place were we used int32 it's 4 bytes long int now.

We can mix data types expressions m = 2+2.75; is valid. Automatic conversion happens called implicit conversion. Likewise explicit conversion

**The const was taken from C++ and incorporated in ANSI C, although quite differently.**  
In C++ const we can use it with methods it will act like read only function however in C there are no methods. Other difference we cannot use const int size as array size in C but C++ we can. In C++ const are local whearas in C it's global.

Advantages of new over malloc:

* It automatically determines the size of data object we don't need to use operator sizeof
* It automatically returns correct pointer size, malloc return void\*
* Like any operator new and delete can be overloaded
* New calls the constructor too hence initializing while malloc doesnt
* We use delete for new operator and free for malloc

Int - 4 bytes i.e. 0 to 24.8-1 \[Unsigned\] or -24.8/2 to 24\*8-1 \[Signed\]

void func\(\); //Function prototyping it is not important to specify argument name in it  
void func\(...\) //This is correct

#### Inline Functions

Objective of using function is to save time which become more if it's called multiple times. Lot of time in executing a series of instructions for tasks such as jumping the function, saving registers, pushing arguments on the stack. One solution is to use \#define preprocessor however we cannot perform complex stuffs with it since it's not really a function. Inline functions eliminates the cost of small functions it is expanded in-line when it is invoked i.e. the compiler replaces the function call with it's corresponding function code \(something simillar to macros expansion\). It makes the program runs faster however the trade off is it takes more memory because the statement that defines inline functions are reproduces at each point where the function is called.

Use inline functions when

* Instead of \#define
* When functions are very small and called often, faster code smaller executable
* Not I/O bound
* Don't use with constructors and destructors even when empty they have built-in functionalities
* Don't use with functions having - Loops, Jump statements like goto, switch & Recursive calls or having static variables

Preprocessor macros are just substitution patterns applied to your code. They can be used almost anywhere in your code. Inline functions are actual functions whose body is directly injected into their call site. They can only be useful where a function call is appropriate.

* Macros are not type safe, and can be expanded regardless of whether they are syntatically correct
* Macro done by preprocessor Inline done by compiler
* Macros can expand other macros inline functions cannot
* Inline functions are not always guaranteed to be inlined

void perform\(int a = 5\) //Default argument  


```text
int func() { return 1; }
float func() { return 1.0f; }
```

Function Overloading doesn't work like this We gotta use templates for this  


**Merits**

* Friend function acts as a bridge between two classes by operating on their private datas
* Must have access to source code for the class to make a function into friend
* Able to access without need of inheriting
* Can be used to increase versatility of operator overloading

**Demerits**

* Gives access to private members
* Conceptually messy
* Cannot do any run time polymorphism in it's member
* Maximum size of memory will be occupied by objects according to the size of friend members

Usages Of Friend Functions & Friend Class:

```text
class XYZ
{
private:
    int data;
public:
    void setVal(int val)
    {
        data = val;
    }
    friend void show(XYZ);
};
void show(XYZ xyz)
{
    cout << xyz.data << endl;
}
```

```text
class ABC;
class XYZ
{
private:
    int data;
public:
    void setVal(int val)
    {
        data = val;
    }
    friend void add(XYZ, ABC);
};
class ABC
{
private:
    int data;
public:
    void setVal(int val)
    {
        data = val;
    }
    friend void add(XYZ, ABC);
};
void show(XYZ xyz, ABC abc)
{
    cout << (xyz.data + abc.data) << endl;
}
```

```text
#include <bits/stdc++.h>
using namespace std;
 
class ABC;      //Forward Declaration
 
class XYZ
{
private:
    int data;
public:
    void setValue(int value)
    {
        data = value;
    }
    friend class ABC;
};
 
class ABC
{
public:
    int data;
    void setValue(int value, XYZ xyz)
    {
        data = value + xyz.data;
    }
};

int main()
{
    XYZ x;
    ABC a;
    x.setValue(5);
    a.setValue(4, x);
    getXYZData(x);
    cout << a.data << endl;
    return 0;
}
```

Advantages of passing by reference:

* No new copy of variable is made, so overhead of copying is saved. This makes program execute faster specially when passing objects of large struct or class.
* Array or objects can also be passed.
* Sometimes function need to change the original value like in sorting so this gets useful.
* Can manipulate multiple values passed by reference in argument unline in pass by value where we can return single return value.

```text
class myClass
{
public:
    void doShit(int);
};

void myClass::doShit(int val) { }               //Member function defination
//inline void myClass::doShit(int val) { }        We can even make it inline outside
```

Static Data Members:

* It is initialized to zero when the first object of its class is created.
* Only one copy of the entire class is created for the entire class and is shared by all the objects of that class, no matter how many objects are created.
* It is visible only within the class but it's lifetime is within the program.

Static Member Functions:

* A static function can have access to only other static members \(functions or variables\) declared in the same class.
* It is called with class name instead of object

```text
class Test
{
public:
    static int val;
    static void print()
    {
        cout << val << endl;
    }
};

int Test::val = 1;
int main()
{
    cout << Test::val << endl;
    Test::print();
    return 0;
}
```

![](../.gitbook/assets/image%20%28196%29.png)

#### Pointers To Members

```text
class MyClass
{
private:
    int x, y;
public:
    void show() { }
    friend int sum(MyClass m);
};
int sum(MyClass m)
{
    int MyClass::* ptrx = &MyClass::x;s
    int MyClass::* ptry = &MyClass::y;
    MyClass *cl = &m;
    int S = m.*ptrx + cl->*ptry;
    return S;
}
```

classes can be defined and used inside a function or a block. Such classes are called local.

* It can use global variables \(declared above\) and static variables the function. Should use scope resolution operator ::
* Static data members are not allowed in local class. It need to be defined in global scope instead.

C++ supports struct mainly to support backward compatibility with C however there are still some difference in structs in C & C++

* C++ structs can have both data members and functions. C only allow data members
* struct keyword is necessary in C to create struct type variable
* Empty struct in C is violation
* C structs cannot have static members C++ can have
* Members cannot directly be initialized in struct in C but with C++11 it can
* Both pointer & reference is allowed in C++ struct but only pointer in C

Characterstics of constructors

* They should be declared in the public section
* They are invoked automatically when the objects are created
* They do not have return type not even void
* They can have default argument like any other function
* Constructors cannot be virtual only destructors
* We cannot refer to their address
* Can be overloaded
* Type - Default, Copy, Parametrized, Dynamic initialization in constructor
* Can also explicitly delete a constructor then we cannot create instance of the method however we can still do it from the class's member function:

```text
A a = A(5,190); //Explicit
A a(5,190);
```

```text
MyClass(const MyClass &other) { curX = other.curX; } 

Time t2(t1);    //Explicit call of copy constructor
Time t2 = t1;   //Implicit call of copy constructor
```

> Destructors never take any arguments. There can be a virtual destructor too.

```text
MyClass
{
public:
    MyClass() { }
    MyClass(int val = 8) { }
}
//This will give error since both constructors are default constructor
```

We cannot overload

* Class member access operators \(. .\*\)
* Scope resolution operator \(::\)
* Size operator \(sizeof\)
* Conditional Operator \(?:\)

```text
vector operator+(vector);                   //vector addition
vector operator-();                         //unary minus
vector operator++();                        //pre-increment
vector operator++(int);                     //post-increment
friend vector operator+(vector, vector);    //vector addition
friend vector operator-(vector);            //unary minus
vector operator-(vector);                   //subtration
int operator==(vector);                     //comparison
friend int operator==(vector, vector);      //comparison
friend ostream& operator<<(ostream, vector);
void * operator new(size_t size);
void operator delete(void * p);
```

Rules for operator overloading

* Only existing operators can be overloaded. New operators cannot be created
* The overloaded operator must have atleast one user defined operand
* We shouldn't change the basic meaning of an operator i.e. + operator should add and not subtract
* They cannot be overriden
* Some operators cannot be overloaded
* There are operators on which friend cannot be used: = \(\) \[\] -&gt; because compiler is already providing it so it will create ambiguity if used friend function

The compiler does not support automatic type conversion from human defined data types so we need to define it

```text
//Basic to class type: Simply in constructor - Only single argument constructor behaves like type caster
//Class to basic type:
operator double()
{
    return x / 2;
}
```

#### Namespace

* In each scope naming it in brief means namespacing.
* Namespacing is required so as we can still use same name for variables on different scope
* It was added in C++ and not present in C

```text
namespace something
{
    class MyCoolClass
    {
        ...
    };
}
using namespace something;
```

#### Inheritance

If public then base class variables are accessible through ABC class object. Private of base class are not inherited. Base constructor first gets called then derieved.

```text
class ABC : public XYZ //Derives XYZ it can be private too simple XYZ means private
{
}

Derrived* derrived = new Derrived();
delete derrived;

//Base Constructed
//Derrived Constructed
//Derrived Destructed
//Base Destructed

Base* poly = new Derrived();
delete poly;

//Base Constructed
//Derrived Constructed
//Base Destructed
That's why we use virtual destructors
```

A protected visibility modifier is accessible by the member functions within its class and any immediately derrived from it. It cannot be accessed outside these two classes. In child class also that protected variable becomes protected.

Single Inheritance, Multilevel Inheritance, Multiple Inheritance, Hierarchial Inheritance \(One has hierarchially like many childs\), Hybrid Inheritance\(Mixture of all\)

Using class ABC : public virtual XYZ we declare virtual base class. C++ takes necessary steps to see that only one copy of that class is inherited, regardless of how many inheritance paths exists between them.

```text
class student { ... };
class test : public virtual student { ... };
class sports : public virtual student { ... };
class result : public test, public sports { ... };
```

* Abstract class is one that is not used to create objects
* Acts only as a base class
* Other classes are built through it

> Private members of base class are not inherited is there any way - assign public getter or setter for it in base class

#### This Pointer

* It represents pointer to an object that invokes the member function
* Example calling a.getMax\(\); using this pointer in getMax function will return address of object a
* No need to explicitly tell this in member function but needed if there's a variable name clash

> We can make pointer to derrived class object of type base class It's type compatible

```text
Derrived d;
Base *ptr = &d;
```

#### Virtual Function

* When we use same function in both base & derrived class. The function in base is declared as virtual
* Declared with virtual keyword like - virtual void doSomething\(\) { }
* If redefined in derrived class then redefined function will be called when derrived object is used otherwise if not redefined then base function

Rules for Virtual Functions

* Virtual functions must be a member of some class
* They cannot be static members
* They are accessed using object pointers
* A virtual function can be a friend of another class
* A virtual function of base class must be defined even if it may not be used
* If a virtual function is defined in base class it need not to be necessarily redefined in derrived class in that case call will invoke the base function
* Cannot have virtual constructors but virtual destructors

```text
XYZ *d = new XYZ();
ABC *base = d;
delete base;
```

Doing this will make base gets destroyed but derrived is not destroyed. If we call delete on d then both will be destroyed. But we want to avoid memory leaks for delete base too so we make destructor virtual

Pure Virtual Function:

* It is normal practice to declare a function virtual then redefine it
* The function inside the base class acts as a placeholder used for performing any task
* A do nothing function which gets redefined in derrived class is pure virtual function
* A class with only pure virtual functions cannot be used to declare any objects of their own and are called abstract class.
* Main objective of abstract class is to provide some traits to the derrived classes as it serves as a placeholder
* The implications of making a function a pure virtual function is to achieve run time polymorphism

> Abstract class have at least one virtual function

```text
virtual void doSomething() = 0;
```

#### Calling function 2 ways

```text
perf();
    
void (*func)() = &(perf);
(*func)();
```

#### C++ Streams

* The I/O systems in C++ cout & cin uses streams as data format
* A stream is a sequence of bytes
* It acts either as a source from which the input data can be obtained \(Input Stream\) or as destination to which the output data can be sent \(Output stream\)
* In cin cout &lt;&lt; &gt;&gt; operators are overloaded to provide stream of data functionality
* Cin has get and put function to handle single character I/O. Likewise - cin.getLine\(lineStr, size\), write function displays entire line. cout &lt;&lt; setw\(5\) or setprecision\(5\)

```text
cout << setfill('A') << std::right << setw(5) << "";     //or if in end "PP"
//output AAAAA & AAAPP
```

> \#include is required - The header file iomanip provides a set of functions called manipulators which can be used to manipulate the output formats ios is parent class derrives - istream and ostream class

```text
char c;
cin >> c;       //This will skip white space
cin.get(c);
```

```text
#include <fstream.h>

ofstream oS("file.txt");
oS << "OUT" << endl;
oS.close();

ifstream iS("file.txt");        //We can also create fstream object
// or ifstream iS; ifstream.open("...", mode); like ios::in | ios::nocreate
while(iS)       //Detects end of file
{
    iS >> str;
    cout << str << endl;
}
iS.close();
```

Different modes:

* ios::app - Append to end of file
* ios::ate - Go to end of file on opening
* ios::binary - Binary file
* ios::in - opening file in reading only
* ios::nocreate - open fails if file doesn't exists
* ios::noreplace - open fails if file already exists
* ios::out - open file for writing only
* ios::trunc - delete the content of file if it exists

```text
iS.seekg(0, ios::beg)       //Moves get pointer
iS.seekp(0, ios::beg)       //Moves put pointer
iS.tellg()                  //Moves get pointer
iS.tellp()                  //Moves put pointer
ios::beg                    //start of file
ios::cur                    //cur pos of pointer
ios::end                    //end of file
```

> iS.get\(\), put\(\), read\(\), write\(\) - Read and Write performs in binary format

```text
infile.read((char*) &val, sizeof(val));
```

Error Handling in I/O

* File not exists
* File name for saving already exists
* Invalid operation such as reading past the end of file
* No space in disk to save file

```text
if (iS.bad()) //report it's bad
else iS.clear();
```

move the pointer by 15 positions backwards from current - fout.seekg \(-15,ios::cur\);

#### Command line arguments

```text
int main(int argc, char *argv[]) { return 0; }
//argc is argument count argv is argument vector
//like we enter g++ -g main.cpp && ./a These are goddamn arguments then 4 arguments will go there
```

* argv\[0\] contains name of the program. argv\[1...argc\] contains our arguments

> What is the difference between opening fule through constructor or open function? It's that through constructor say we get out of scope then automatically destructor will be called so we don't have to worry about calling close but using open function we do have to close it to avoid memory leaks. Other difference is we provides modes using the function

Advantages of using binary format in files

* Compact size
* Faster to process as no conversion
* Others cannot easily interpret it
* Some of the encryption or compression algoritms are more easy to implement

#### Templates

* Templates is a new feature in C++
* It enables us to define generic classes and functions & thus providing support for generic programming
* Generic programming is an approach in which generic types are used as parameters
* Avoids code duplication
* It works like macros during run time gets copy pasted

```text
template<class T> //or template<typename T> or multiple can also be assigned template<class T1, class T2>
class MyClass
{
    T something;
};
//or even function overloading - we can use T as parameter or even return type too
template<typename T>
void f(T s)
{
    std::cout << s << '\n';
}
int main()
{
    MyClass<SomeClass> obj;
    return 0;
}
//Even non template type can be used as argument to template
template<typename T, int size>
void f(T s)
{
    int arr[size];
    ...
}
```

Difference between templates & macros

* Templates obey C++ syntatically
* Templates are more powerful
* Templates are slower as say it has functionality such as during recursive calls it will still act generic and thus macros are limited to single expansion
* Syntax difference
* macros are preprocessor means it gets processed before actual code

#### Exception Handling

An exception is a situation, which occured by the runtime error. In other words, an exception is a runtime error. An exception may result in loss of data or an abnormal execution of program.

* Find the problem \(Hit the exception\)
* Inform that an error has occured \(Throw the exception\)
* Recieve the error information \(catch the exception\)
* Take corrective actions \(Handle the exception\)

```text
try
{
    if (b != 0) cout << a/b << endl;
    else throw(x);
}
catch(int i)
{
    cout << "Exception " << i << endl;
}
catch(int i) { ... } //multiple catch can be there
```

Try is to used to invoke a function that contains an exception

Class Type Exception

```text
class Error
{
    ...
};
try
{
    throw Error("Shitty Error", 5000);
}
catch(Error e)
{
    ...
}
//catch(...) will catch all exceptions
//using throw inside catch is rethrowing an exception
```

> strcpy\(s1, s2\) function to copy string, int n = strlen\(str\) to get string length

* Threads are smaller part of a process.
* In current generation of mult core CPU we can perform many task at a time so in order to utilize it we use Multi threading

```text
#include <thread> 
...
int main()
{
    thread thr1(func, params);
    thr1.join();
    return 0;
}
```

Thread synchronization is the concurrent execution of two or more threads that share critical resources. Threads should be synchronized to avoid critical resource use conflicts. Otherwise, conflicts may arise when parallel-running threads attempt to modify a common variable at the same time.

#### Java

```text
import java.io.*
public class Test
{
    public static void main(String args[])
    {
        Scanner S = new Scanner(System.in);
        int a = S.nextInt();
        system.out.println(a + "~");
    }
}
```

* In C++ to support backward compatibility with C can still follow non OOP standards but In Java, everything is Object Oriented.
* It is platform independent as it runs on a virtual platform JVM \(Java Virtual Machine\) unlike C++ which runs on native machine and hence if machine doesn't support C++ it won't compile.
* Java is more secure since there is no pointer concept so no explicit pointers or memory leaks harming performance.
* There are some inbuilt exception handling & memory management which makes java more robust.
* Applets allows us to even run java program in a web browser.
* Java supports call by value only \(No call by reference\).
* Java has built in multi threading support.
* Inbuilt Java Swing functionality allows user to create window-based applications which is not possible in C++ without third party libraries.

> Java bytecode is the instruction set for the Java Virtual Machine.

> Servlet is Java EE server driven technology to create web applications in java.

> JRE \(Java Runtime Environment\) are set of tools required for java development it include JDK \(Java Development Kit\) & Some support libraries

> Template class & Class template are not the same. Template class is wrong it's only class template or function template because classes do not define templates, templates define classes \(and functions\).

* 
