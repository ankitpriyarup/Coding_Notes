# C++

### Object Oriented Language

Data representation in programming should have - correctness, maintainability, reusability, openness and interoprability, portability, security, integrity and user friendliness.

Softwares have evolved from - Machine Language \(01\), Assembly Language \(CPU instructions MOV - move instruction, LSR - logical shift right, ROR - rotate right\), Procedure orientate \(C - function based\), Object oriented.

**In procedure orientated langage:**

1. Emphasis is on doing things \(algorithms\)
2. Large programs are divided into smaller programs known as functions.
3. Most of the function share global data.
4. Data move openly around the system from function to function.
5. Function transform data from one form to another.
6. Employs top-down approach in program design.

**However, in Object Oriented language:**

1. Emphasis is on data rather than procedure.
2. Programs are divided into what known as objects.
3. Data structure is designed such as they categorize the objects.
4. Functions that operate on data of an object are tied together in the data structure.
5. Data is hidden and cannot be accessed from external source.
6. Object may communicate with each other through functions.
7. New data and functions can be easily added whenever necessary
8. Employs bottom-up approach in program design.

bottom up means we analyze objects which are entities \(lower part\) then that performs task assosiated linked in object class. In top down it's a simple function performing lineraly.

Basic concepts of OOPS- Objects, Classes, Data abstraction \(abstracting or summarising data which are required only\) and encapsulation \(setting getter and setter for private variables providing data protection - encapsulation from outside\), Inheritence \(using members from some other parent class\), Polymorphism \(ability to display in more than one form like function, operator overloading or function overriding - overriding is using function from parent class\), Dynamic binding or Dynamic Dispatch \(It is also known as late binding it basically binds data or object with methods or implementation defined in it at runtime examples are virtual functions\), Message passing

**Benefits Of OOPS:**  


1. Elimination of redundant and duplicate code through inheritence.
2. We can build programs from the standard working module which will communicate with one another in order of task completion. In this way debugging any operation will also get easier.
3. Data encapsulation provides unenesessary data editing.
4. Software complexity can be easily managed.
5. Upgradation of system can be easilty done.

### Introduction To C++

Using C++ we can literally control any instruction of CPU. Other languages like C\# and Java differs because they run on a virtual machine that's why many platforms applications can be developed through C++. It's native so it's fast but a bad C++ code will be slower whears in C\# and Java there are optimizations done by their compilers which makes them handle fast without worrying much about code.

**Tokens**  
Smallest individual units in a program are known as token. C++ has

* Keywords \(asm, auto, break, case, catch, char, const, continue, default, delete, do, double, else, enum, extern, float, for, friend, goto, if, inline, int, long, new, operator, private, protected, public, register, return, short, signed, sizeof, static, struct, switch, template, this, throw, typedef, union, unsigned, virtual, void, volatile, while, bool, const\_char, dynamic\_cast, explicit, export, false, mutable, namespace, reinterpret\_cast, static\_cast, true, typeid, typename, using, wchar\_t\)
* Identifiers \(Name of the variables, functions, array, classes, etc.\)
* Constants
* Strings
* Operators \(:: Scope resolution operator, ::\* Pointer-to-member declaration, -&gt; Pointer-to-member operator, .\* pointer-to-member operator, delete, endl, new, setw, &lt;&lt;, &gt;&gt;\)

**\#statements** is a pre processor statement it preprocesses before the actual process. main function is the entry point. Even though int main we don't need to return any value for it. it assumes that we are returning value 0.  
  
Header files are not compiled they are included C++ files are compiled only. Every C++ file gets compiled individually and converted to an object file. Linker takes all obj files of c++ and makes an executable.  
  
Declaration is like telling compiler that hey this function exists. And defination is telling that this is the body of this function.

```cpp
//Defination
void Log(const char* msg)
{
    cout << msg << endl;
}

//Declaration
void Log(const char* msg);
```

To call a function from other c++ file just add declaration. While compiling compiler will trust that the function exists and if the function doesnt exists compiler won't give any error linker will give error. Linker job is to link up the functions.

**\#include** copy pastes data from header file to our's  
**\#define** find and replaces  
**\#if** for conditions

```cpp
//setw
cout << setw(10) << "Basic:" << setw(10) << basic << endl;
cout << setw(10) << "Allowance:" << setw(10) << allowance << endl;
cout << setw(10) << "Total:" << setw(10) << total << endl;

//output
//    Basic     940
//Allowance      50
//    Total      12
```

### Compilation

Functions linking done through linker even if it's a project of one c++ file linker job is to define the entry point. If a file with no main function only present no compiler error but linker error. functions have signatures which are used for linking.

Static are called only for that file and if it is included in some other file then it won't work. Inline makes the compiler replace the code block into a line where it is used so making execution time faster.

```cpp
class Item
{
public:
    void getData(int a, int b);
};
inline void Item::getData(int a, int b)
{
    ...
}
```

**int** 2 bytes \(32 bit\) 4 bytes \(64 bytes\) compiler based  
1 bit is for sign 2^31 maximum number that can be stored.  
**unsigned int** means only positive  
**char** 1 byte  
**short** 2 byte  
**long** 4 byte  
**long long** 8 byte **float** 4 byte with f prefix **double** 8 byte **bool** 1 byte it uses 1 bit only for 0 or 1 but to address memory 1 byte has to be occupied.

For creating function a stack for the function is created then all parameters are pushed there also return address is pushed to the stack. Jumping from one memory instruction to another for function then return back to the original. So all of this slows down program aswell.

Headers are basically used to store declarations. Since declarations are not required in modern java and C\# programs no header files in it. in header file \#pragma once it tells compiler to include the header file only once in a translation unit.

```text
One way to replace pragma once is 
#ifndef _BOOL
#define _BOOL
....
#endif
```

Basically it will check if **\_BOOL** is not defined then it will define and end the condition. Angle brackets are used to include header files that are in the system include directory otherwise for custom we use "

Quotes can be also used for include director aswell C++ header files like iostream don't have .h extension while math.h kind of has because to distinguish between C++ header files and C header files by the developer

Constant folding: During debug mode it doesn't happen but in release mode when compiler optimizations take place compiler just reduce code for constant aritmetic operations simplifing the program.

Compiler also improves many operations such as one given below to bitshift operations which are faster in performance.

There are some compiler centric definations like \#if \_DEBUG in Visual Studio means the debug configuration condition of the program.

In a class 'this' keyword is used to reference current instance of the class

```cpp
class Entity
{
public:
	int x, y;
	Entity(int x, int y)
	{
		Entity* e = this;
		e->x = x;
	}
};
```

![](../.gitbook/assets/image%20%28261%29.png)

**Function Prototyping:** declaring a function like the way headers do  
void func\(int a, int b = 5\) this function can also be called with a value only. This is called default argument.

### Memory Allocation

Dynamic Allocation is allocation at runtime  
Stack Allocation is allocation at compiletime

```cpp
//creates a char and stores it address.
char* buffer = new char[8];
//assigns 8 zeroes in buffer address memory
memset(buffer, 0, 8);
//delete buffer from memory
delete[] buffer;
```

Pointers also gets stored in stack so pointer to pointer is meaningful.

We can use void\* ptr \(null pointer\) but then derefrencing and adding value will be a problem.

```cpp
void increment(int& value)
{
    value++;
}
```

This function when executed from main function will reference the parameter inputted and make changes in it so basically it is a cleaner way for pointer implementation.

Once reference declared it cannot be changed

```cpp
int a = 5;
int b = 8;
int& ref = a;
ref = b;
```

In this case ref will not become b but a's value will get to b  


> A pointer can be re-assigned whereas a reference cannot be reassigned. A pointer has it's own memory address and size on stack whereas reference shares same memory address but new size on stack. Pointers can be null pointers.

Array created through heap -

```cpp
int* array = new int[5];
delete[] another;
```

normal array is created on stack and hence will get removed once memory is free by moving out of function heap created will be alive and need to be manually deleted.

Arrays occupies on contiguous block of memory.

![](../.gitbook/assets/image%20%28210%29.png)

A 2D array is also stored linearly. i.e. \(0,n\) & \(1,0\) will have address gap of 4 bytes aswell.

![](../.gitbook/assets/image%20%28257%29.png)

This type of memory allocation in 2D array where \(0, n\) & \(1, 0\) have a gap of 4 bytes is called row major. There is column major form aswell in which \(n, 0\) & \(0, 1\) will have a gap of 4 bytes.  
2D Arrays can also be used to evaluate polynomial expressions by creating matrix and solving equations.

New object will be created in heap so it will be stored unless manually deleted stacks are used basically in heap we can have memory leaks if we forget to free memory so heaps are used when we need to extend visibility of objects. smart pointers are something which can be created on heap and also deleted.

finding memory allocation in memory say when we declare an int 4 bytes of memory needed to be searched in memory. There's a free list in memory which tracks pointer of free memory.

```cpp
malloc(bytes of memory)
//returns a pointer of heap memory it is simmilar to new keyword of C++
calloc(elementsCount, sizeOfEachElement)
//Systax is different in calloc but it also returns a pointer to heam memory however it also initializes it to ZERO

int* temp = (int*) malloc(sizeof(int));
int* temp = new int;
```

### 

### Derrived Data Types:

```cpp
class Player
{
public:
    int x, y;
    int speed;

    void Move(int xa, int ya)
    {
        x += xa * speed;
        y += ya * speed;
    }
};

int main()
{
    Player player;
    player.Move(5, 9);
}
```

Functions inside a class are called method.

Struct and class has no difference except default it's private in class but it's public in struct. It exists because of compatibility for C in C++ because C has no class.

static in C++ has two meanings for OOPS based it is same for all instance while for non OOPS it means that linker will not look for that variable while linking and that variable is for that translation unit only.

```cpp
//main.cpp
int myvar = 5;
//new.cpp
int myvar = 6;
```

This will give a linker error If static int myvar = 6 then no linker error and value of main.cpp myvar will be 5.

```cpp
//main.cpp
extern int myvar;
```

This makes it 6 as it will look for external linking for the variable.

```cpp
class Entity
{
    static int x, y;
public:
    int size;
};

int Entity::x;
int Entity::y;

int main()
{
    Entity e;
    Entity::x = 2;
    Entity::y = 5;
    e.size = 5;
}
```

```cpp
void Function()
{
    //It will log 1 per function call
    //If it was static int i = 0 then it would have logged 1, 2, 3, 4
    int i = 0;
    std::cout << ++i << std::endl;
}
int main()
{
    Function();
    Function();
    Function();
    Function();
}
```

Exception handling can be done using try catch block

```cpp
try
{
    x = new float[1000];
}
catch (bad_alloc e)
{
    throw "Out of memory exception!";
    exit(1);
    //Exit will exit the entire program. It is used to debug that something wrong has happened.
}
```

There are some defined exceptions: std::exception, std::bad\_alloc, std::bad\_cast, std::bad\_exception, std::bad\_typeid, std::logic\_error, std::domain\_error, std::invalid\_argument, std::length\_error, std::out\_of\_range, std::runtime\_error, std::overflow\_error, std::range\_error, std::underflow\_error

A new exception can be even defined like this

```cpp
struct MyException : public exception
{
    const char * what () const throw ()
    {
        return "C++ Exception";
    }
};
//Fun fact the default return type of a function in c++ is int so instead of int main() we can have just main() however this might give a warning.
```

```cpp
enum level
{
    A, B, C
};
level cur = A;
```

Here A=0, B=1, C=2  
if A=0, B, C  
B and C will automatically get 1, 2  
enum names doesn't mean anything in C++

```cpp
class Entity
{
public:
    int x, y;

    Entity()
    {
        x=0;
        y=0;
    }
};
```

There's a default constructor in C++ if we have

```cpp
class Foo
{ 
public:
    Foo() = delete; 
};
```

that constructor is also deleted meaning we cannot create instance of the method however we can still do it from the class's member function:

```cpp
class Foo
{ 
  private: 
    Foo() {}     
  public:
    static void foo();
};

void Foo::foo()
{
   Foo f;    //legal
}
```

```cpp
#include <bits/stdc++.h>
using namespace std;

class MyClass
{
public:
    MyClass() = delete;
    MyClass(int a)
    {
        cout << a << endl;
    }
};

int main()
{
    MyClass test(5);
    return 0;
}
```

If we throw an exception inside a constructor then the constructor won't call hence desrupting the creation of the object. Exceptions are used to signal the occurence of an error.

```cpp
throw "Incorrect parameter exception";
```

**Copy Constructor** It initializes an object with the constructor parameters of some other object.

```cpp
class Point 
{ 
private: 
    int x, y; 
public: 
    Point(int x1, int y1) { x = x1; y = y1; } 
  
    // Copy constructor 
    Point(const Point &p2) {x = p2.x; y = p2.y; } 
  
    int getX()            {  return x; } 
    int getY()            {  return y; } 
};
int main() 
{ 
    Point p1(10, 15); // Normal constructor is called here 
    Point p2 = p1; // Copy constructor is called here 
    return 0;
}
```

A constructor which assigns memory dynamically at runtime using new/calloc/malloc that constructor is called Dynamic Constructor.

A destructor method will call after the object will destroy.

```cpp
~Entity()
{
    x=0;
    y=0;
}
```

```cpp
int main()
{
    {
        Function();
        void* ptr = new Function();
    }
}
```

Here the function's address is stored in stack which will be cleared right after the code exits from a brace \(if condition, function end, loop body, etc...\)

However using new operator for creating will create it in heap memory so it won't get cleared unless **delete ptr;** is called.

```cpp
class sub : public parent
{
    ...
};
```

Inherting will give access to all parent class member. If we do sizeOf\(sub\) it will be size of all variables in sub and parent. Virtual method in parent class can be overrided from sub class

```cpp
int main()
{
    Entity* e = new Entity();
    std::cout << e->GetName() << std::endl;
}
//cout << something << something_else; this appending to stream is called cascading
```

Virtual functions do something called dynamic dispatch it is implemented by v table. V table is a table that contains mapping for all the virtual tables.

```cpp
std::string GetName() override
{
    return name;
}
```

Virtual functions has memory cost first at the time of initialization of v table and then at every time while looking through the table

pure virtual function or interface in C++ they are abstract functions in Java & C\#

```cpp
class Entity
{
public:
	virtual std::string GetName() = 0;
};
```

GetName is a virtual abstract function means it will have implementation in sub class only and also instance of Entity class cannot be created. All abstract functions are needed to be implemented in sub class otherwise an error will show.

**Private:** visible to current and friend class  
**Public:** visible to all  
**Protected:** visible to current and sub class only

```cpp
int main()
{
	const char* name = "che\0rno";
	std::cout << strlen(name) << std::endl;
	std::cin.get();
}
```

Here it will give 3

```cpp
/*
wchar_t char16_t char32_t all are used to represent strings. wchar_t  represent distinct codes for all members of the largest extended character. char16 is smaller char32 is bigger than that. char* is the smallest.
*/

const char* name = u8"Cherno";
const wchar_t* name2 = L"Cherno";
const char16_t* name3 = u"Cherno";
const char32_t* name4 = U"Cherno";
```

Const keyword: It is like a promise to the compiler that this won't change it will stay constant then the compiler performs compilation performance improvements based on that. We can break this promise however by changing the value from memory resource manager but it's not good.

```cpp
const int MAX_AGE = 90;
const int* a = new int;
*a = 2;                 //ERROR since a is constant
a = (int*)&MAX_AGE;     //No Error now
//Can't change content of the pointer here but can change pointer address

const int MAX_AGE = 90;
int* const a = new int;
//or int const* a = new int;
*a = 2;                 //No Error here
a = (int*)&MAX_AGE;     //ERROR since pointer of a is constant
//Can't change the pointer address here

const int* const a = new int;
//can't change anything now
```

Const in a method means that it can only read variables and not change them however a mutable variable can be still changed from inside of the const method

```cpp
class Entity
{
private:
    int m_x, m_y;
    int GetX() const
    {
        return m_x;
    }
};
```

Member Initializer Lists in C++ are used to initialize values in class constructor. It should be used for style purpose as it makes things in one line and also because normally initializing variables inside class will create a new object and then remove previous objects so using this is more optimized.

```cpp
class Entity
{
private:
    int a, b;
public:
    Entity() : a(0), b(0)
    {
        Init();
    }
};
```

Must be initialized in proper sequence so b\(0\), a\(0 is not correct.

```cpp
int main()
{
    Entity a("abc");
    //is simillar to
    Entity a = "abc";
    //implicit conversion is happening
}

//if a function
void Function(Entity& entity)
{
}

int main()
{
    Function("abc");
    //this will work because string is a valid constructor for Entity object and hence implicit coversion will take place
}
```

If there is explicit keyword before constructor then implicit conversion won't take place

```cpp
Entity
{
private:
    std::string m_Name;
public:
    explicit Entity(std::string& name) : m_Name(name) {}
};
```

There's function overloading in which we have two functions with same name but different parameters and different implementations.

Operator overloading can be done here we have a struct for maths vector which we want to work with cout.

```cpp
struct Vector2
{
    float x, y;
    Vector2(float _x, float _y) : x(_x), y(_y) {};
    Vector2 Add(const Vector2& other) const
    {
        return Vector2(x+other.x, y+other.y);
    }
    Vector2 operator+(const Vector2& other) const
    {
        return Add(other);
    }
};

std::ostream& operator<<(std::ostream& stream, const Vector2& other)
{
    stream << other.x << ", " << other.y;
}

int main()
{
    Vector2 posA(5.0f, 7.0f);
    Vector2 posB(9.0, 10.0f);
    cout::cout << (posA + posB) << cout::endl;
}
```

### Advanced Pointers Concepts:

Member Function Pointers:

```cpp
int main()
{
    CPoint p1(6.0, 5.0);
    CPoint p2(2.0, 2.0);
    cout << p1.distance(p2) << endl;
    
    //Another way using member pointer
    double (CPoint::*pDistance)(CPoint&);
    pDistance = &Cpoint::distance;
    cout << p1.*pDistance(p2) << endl;
    return 0;
}
```

```cpp
#include <bits/stdc++.h>
using namespace std;

class CMessage
{
public:
    CMessage() {}
    void Print1() { cout << "#1"; }
    void Print2() { cout << "#2"; }
    void Print3() { cout << "#3"; }
};

int main()
{
    void (CMessage::*pfnArray[3])();
    pfnArray[0] = &CMessage::Print1;
    pfnArray[1] = &CMessage::Print2;
    pfnArray[2] = &CMessage::Print3;

    CMessage func;
    for (int i = 0; i < 3; ++i)
        (func.*pfnArray[i])();
    return 0;
}

//Output: #1#2#3
```

Smart pointer is allocated on heap but it gets automatically deleted after specific time.

1. Unique pointer :

```cpp
class uniquePtr
{
private:
    Entity* m_Ptr;
public:
    uniquePtr(Entity* ptr) : m_Ptr(ptr) {}
    ~uniquePtr(Entity* ptr)
    {
        delete m_Ptr;
    }
};

int main()
{
    {
        uniquePtr e = new Entity();
    }
}
```

This heap allocation will get destroyed once out of the scope

OR

```cpp
#include <memory>
int main()
{
    {
        std::unique_ptr<Entity> e = std::make_unique<Entity>();
		//OR
        std::unique_ptr<Entity> e(new Entity());
        
        e->ANYFUNCTION();
    }
}
```

Using new Entity\(\) from constructor is not preffered using make\_unique is better because of exception safety. It is slightly safer if constructor happens to throw exception it will end up with dangling points with no reference and hence memory leaks.

```cpp
//Copying unique_ptr is not possible
std::unique_ptr<Entity> e1 = std::make_unique<Entity>();
//It will give error
```

1. Shared\_Ptr:

```cpp
#include <memory>
int main()
{
	{
		std::shared_ptr<Entity> e0;
		{
			std::shared_ptr<Entity> sharedEntity = std::make_shared<Entity>();
			e0 = shared_ptr;
		}
	}
}
```

It gives copying ptr ability and once all copy gets out of scope then the memory gets free. First shared ptr dies after end of first scope but it gets copied to e0 shared\_ptr so when that scope dies then only Entity is destroyed

1. Weak\_Ptr:

```cpp
Same as shared pointer but it doesn't matter if it is alive it won't keep Entity alive
#include <memory>
int main()
{
    {
        std::weak_ptr<Entity> e0;
        {
            std::shared_ptr<Entity> sharedEntity = std::make_shared<Entity>();
            e0 = shared_ptr;
        }
    }
}
```

Here now entity gets destroyed after first scope. We can use entity object from e0 but it's presence won't keep entity alive.

Smart pointers are very useful it is very memory managed so it can be used any time. it doesn't replace new and delete it is a smart defined way to handle heap allocations.

```cpp
struct Vector2
{
    float x, y
};

int main()
{
    Vector2 a = { 1.0f, 2.0f };
}

memcpy(m_Buffer, string, m_Size);
m_Buffer & string are of type char*

class String
{
private:
    char* m_Buffer;
    unsigned int m_size;
public:
    String(const char* string)
    {
        m_Size = strlen(string);
        m_Buffer = new char[m_Size + 1];
        memcpy(m_Buffer, string, m_Size);
        m_Buffer[m_Size] = 0;
    }

    ~String()
    {
        delete[] m_Buffer;
    }

    char& operator[](unsigned int index)
    {
        return m_Buffer[index];
    }

    friend std::ostream& operator<<(std::ostream& stream, const String& string);
};

std::ostream& operator<<(std::ostream& stream, const String& string)
{
    stream << string.m_Buffer;
    return stream;
}

int main()
{
    String string = "Cherno";
    String string2 = string;
    string2[2] = 'a';

    std::cout << string << std::endl;
    std::cout << string2 << std::endl;
}
```

Because of the function operator &lt;&lt; of cout declared as friend of String class it can access it's private variable m\_Buffer In this case both string will output will be Charno and at the end of scope an error will occur. It is because while copying the exact pointer gets coppied so when destructor is called m\_Buffer is deleted and then the next class also tries to delete the already deleted m\_Buffer which gives the error so we need that when we copy the class the pointer gets a new block of memory this is called deep copying

inside class

```cpp
String(const String& other) = delete 
this will make it un coppiable like in unique pointer
String(const String& other) : m_Size(other.m_Size)
{
    m_Buffer = new char[m_Size + 1];
    memcpy(m_Buffer, other.m_Buffer, m_Size + 1);
}
```

Usages Of Friend Functions & Friend Class:

```cpp
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

```cpp
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

```cpp
#include <bits/stdc++.h>
using namespace std;
 
class ABC;
 
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
    friend void getXYZData(XYZ);
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
 
void getXYZData(XYZ xyz)
{
    cout << xyz.data << endl;
}
 
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

### Extra Concepts:

```cpp
Struct Vector3
{
    float x, y, z;
};

int main()
{
    int offset = (int)&((Vector3*)nullptr)->z;
}
```

offset is 8 for z 4 for y 0 for x

Standard Template Library \(STL\) is a set of C++ template classes to provide common programming data structures and functions such as lists, stacks, arrays, etc. It is a library of container classes, algorithms and iterators. It is a generalized library and so, its components are parameterized.

It containts - Sorting, Searching, reversing, max element, min element, accumulate which is sum of elements of an itterator, count in itterator, find, binary\_search, lower\_bound which is the itterator, first or lower occurence of the element in the itterator, upper\_bound, erase, distance which is distance between two iterators, next\_permutation, prev\_permutation.

```cpp
int myints[] = {1,2,3};
do
{
    std::cout << myints[0] << ' ' << myints[1] << ' ' << myints[2] << '\n';
}
while (next_permutation(myints,myints+3));
```

5 10 15 20 20 23 42 45  
Vector after performing next permutation:  
5 10 15 20 20 23 45 42  
Vector after performing prev permutation:  
5 10 15 20 20 23 42 45

Array Algorithms include all\_of\(\) which is like running a loop on the array.

```cpp
all_of(ar, ar+6, [](int x) { return x>0; })?
          cout << "All are positive elements" :
          cout << "All are not positive elements";
```

any\_of\(\) it is same as all\_of except it will return after finding any true case, none\_of\(\)

copy\_n\(\) to copy arrays

```cpp
copy_n(ar, 6, ar1);
```

iota\(\) it assigns constant values to the array. like - 20 21 22 23 24 25

```cpp
iota(ar, ar+6, 20);
```

There are helpers for - vector, list, deque, arrays, forward\_list, queue, priority\_queue, stack, set, multiset, map, multimap, Functors, Iterators, pair.

Foreach loops are a short hand

```cpp
std::vector<Vertex> vertices;
vertices.push_back({1, 2, 3});

for(Vertex& v : vertices)
    std::cout << v << endl;
```

vector will resize like 1 then 2 then 4 then 8 so if we are planning to have atleast 4 elemets then we can reserve it in that way unecessary copying and deletion of previous memory allocation can be avoided. std::vertices.reserve\(3\);

Second optimization strategy is that using push\_back results in copying of object to the vector memory which can be avoided by vertices.emplace\_back\(1, 2, 3\);

Linking projects with external libraries. Static linking is in which the source code gets included in the final exe whearas in dynamic linking, linking libraries is done at runtime. \(DLL file - Dynamic Linking Library\) Static linking is faster.

To return multiple values from the function we can return a struct with those values, or we can return a vector containing that values or pointer to the values, or we can return a tuple

```cpp
#include <utility>
std::tuple<std::string, int> returnsomevalue()
{
    return std::make_pair("as", 12);
}

int main()
{
    //std::tuple<std::string, int> sources = returnsomevalue();
    auto sources = returnsomevalue();
    std::string a = std::get<0>(sources);
    int b = std::get<1>(sources);
}
```

same way we can use pair it will have 2 non simmilar values only. then we can use

```cpp
auto sources = returnsomevalue();
std::string a = sources.first;
```

or sources.second

struct is more better because it is much clearer and easy to read instead of first second we can use direct variable name.

Function pointers are void\*

```cpp
void HelloWorld()
{
    std::cout << "Hello World" << std::endl;
}

int main()
{
    auto function = HelloWorld;
    //or void(*functionName)() = HelloWorld;
    function();
    function();
}
```

This gets meaningful if we want to pass function to another function. This will retrieve the memory location of the function

```cpp
void ForEach(const std::vector<int> &values, void(*func)(int))
{
    for (int value : values)
        func(value);
}

int main()
{
    std::vector<int> values = { 1, 2, 3, 4 };
    ForEach(values, [](int value) { std::cout << "Value: " << value << std::endl; });
}

Inline functions which is to replace function pointers are the lambda functions

std::vector<int> values = { 1, 5, 4, 2, 3 };
auto it = std::find_if(values.begin(), values.end(), [](int value) { return value > 3; });
std::cout << *it << std::endl;
//5
```

Threads in C++

```cpp
static bool s_Finished = false;

void DoWork()
{
	using namespace std::literals::chrono_literals;
	while (!s_Finished)
	{
		std::cout << "Working...\n";
		std::this_thread::sleep_for(1s);
	}
}

int main()
{
	std::thread worker(DoWork);
	std::cin.get();
	s_Finished = true;
	worker.join();
	std::cin.get();
}
```

Output-  
Working...  
Working...  
Working...  
Working...  
unless something is input by user then the thread get's completed.

Timer functions to precisely calculate time of the system is very important for example to find the duration of time taken by an algorithm.

```cpp
#include <iostream>
#include <chrono>
#include <thread>

int main()
{
    using namespace std::literals::chrono_literals;
	
    auto start = std::chrono::high_resolution_clock::now();
    std::this_thread::sleep_for(1s);
    auto end =  std::chrono::high_resolution_clock::now();
	
    std::chrono::duration<float> duration = start - end;
    std::cout << duration.count() << "s" << std::endl;

    std::cin.get();
}
```

```cpp
int** a2d = new int*[50];
for(int i=0; i<50; ++i)
    a2d[i] = new int[50];

int*** a3d = new int**[50];
for(int i=0; i<50; ++i)
{
    a3d[i] = new int*[50];
    for(int j=0; j<50; ++j)
        a3d[i][j] = new int[50]
}

a2d[0][0] = 1;
a3d[0][0][0] = 1;

for(int i=0; i<50; ++i)
    delete[] a2d[i];
delete[] a2d;

for(int i=0; i<50; ++i)
{
    for(int j=0; j<50; ++j)
        delete[] a2d[i][j];
    delete[] a2d[i];
}
delete[] a2d;
```

Here we are reserving different block of memory for different rows or columns that we are making and hence it can be far in the memory address. Fetching those far memories can result in slow speed hence we should essentially make our own allocator that can allocate memory for us nearer.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <functional>

int main()
{
	std::vector<int> values = { 3, 5, 1, 4, 2 };
	std::sort(values.begin(), values.end(), [](int a, int b)
	{
		return a < b;
	});
}
```

Output: 1 2 3 4 5

instead of lambda we can pass inbuilt functions from functional std::greater\(\)

compare function if return true then a will be placed before b otherwise opposite.

struct or class occupies sum of memory occupied by individuals however a union occuies memory of the most memory costly item.

```cpp
struct Vector2
{
	float x, y;
};

struct Vector4
{
	union
	{
		struct
		{
			float x, y, z, w;
		};
		struct
		{
			Vector2 a, b;
		};
	};
};
```

Now here if we set b value of the vector4 struct then z also get's changed to the same

```cpp
class Base
{
    Base() { std::cout << "Base Constructed\n;" }
    ~Base() { std::cout << "Base Destructed\n;" }
}
class Derrived : public Base
{
    Derrived() { std::cout << "Derrived Constructed\n;" }
    ~Derrived() { std::cout << "Derrived Destructed\n;" }
}

int main()
{
    Base* base = new Base();
    delete base;
	
    //Base Constructed
    //Base Destructed

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
}
```

making destructor virtual of the base class will make polymorphic function call destructor as well hence avoiding memory leaks.

Type casting done explicitly by paranthesis is the c style type casting whereas C++ type castings are - static\_cast, dynamic\_cast, const\_cast, interpret\_cast

```cpp
double s = static_cast<double>(integer);
```

dynamic cast will perform type casting at runtime if it is possible. value gets null if the conversion is not possible.

C++ type casting makes code readiblity better. C style and C++ both are equivalent

