# CPP Video Note
- [CPP Video Note](#cpp-video-note)
  - [CMake](#cmake)
  - [Compiler](#compiler)
  - [Linking](#linking)
  - [Variable](#variable)
  - [Function](#function)
  - [Header Files](#header-files)
  - [debug issues](#debug-issues)
  - [Condition](#condition)
  - [Loops](#loops)
  - [Control Flows](#control-flows)
  - [Pointers](#pointers)
  - [References](#references)
  - [Class](#class)
  - [Class and Struct](#class-and-struct)
  - [Class (deeper)](#class-deeper)
  - [Static(outside)](#staticoutside)
  - [Static(inside)](#staticinside)
  - [Local Static](#local-static)
  - [Enum](#enum)
  - [Constructor](#constructor)
  - [Destructor](#destructor)
  - [Inheritance](#inheritance)
  - [Virtual Functions](#virtual-functions)
  - [Pure Virtual Function](#pure-virtual-function)
  - [Visibility](#visibility)
  - [Arrays](#arrays)
  - [Strings](#strings)
  - [String Literals](#string-literals)
  - [Const](#const)
  - [Mutable](#mutable)
  - [Initializer List](#initializer-list)
  - [Ternary Operators](#ternary-operators)
  - [Create Object](#create-object)
  - [New](#new)
  - [Implicite and Explicite](#implicite-and-explicite)
  - [Operator and Overloading](#operator-and-overloading)
  - [this](#this)
  - [Object Lifetime (on Stack)](#object-lifetime-on-stack)
  - [Smart Pointers](#smart-pointers)
  - [Copying and Copy Constructor](#copying-and-copy-constructor)
  - [Arrow Operators](#arrow-operators)
  - [std::vector](#stdvector)
  - [std::vector optimization](#stdvector-optimization)
  - [Static Library](#static-library)
  - [Dynamic Libraries](#dynamic-libraries)
  - [Create and Use Libraries](#create-and-use-libraries)
  - [Multiple Return](#multiple-return)
  - [Templates](#templates)
  - [Stack and Heap](#stack-and-heap)
  - [Macros](#macros)
  - [Auto](#auto)
  - [std::array](#stdarray)
  - [Function Pointers](#function-pointers)
  - [Lambda](#lambda)
  - [Why NOT `using namespace std`](#why-not-using-namespace-std)
  - [Namespace](#namespace)

## CMake
this is how you use `Cmake` if every thing is done:
```BASH
mkdir build
cd build
cmake .. # make, cmake will find CMakeLists.txt in .. and do make
cmake --build . # generate executable file
cd bin
./* #run all executable file
```

## Compiler
+ files are just ways to feed compilers msg, files have no meaning for cpp
+ 1 cpp file $\to$ 1 translation unit $\to$ 1 obj file in Windows
+ **pre-process** is even earlier than compiling, e.g.,`# include`, it just copy
    you can even do this:
    ```C++
    # include <iostream>
    void Log(const char * msg){
        std::cout << msg << "—— yukicr" << std::endl;
    //}   //so no '}' in this func
    # include "a.h"
    ```
    in `a.h`:
    ```
    } // done
    ```
    and it **works** for cpp!
    likely, `#define A B` replace A with B directly
+ good way to wirte conditional compile:
    ```C++
    #if isCompile
    ...
    #endif
    ```
+ `CMake` provides different compiling variant, `debug` is suitable for debugging as no optimize is done during compiling

## Linking 
+ .obj files need to cooperate with each other, even if you have one file, you still need do find where the `main` function is.
+ typically, `main` is the entry point, but the entry point can be sth. else.
+ if we use a func in another file, we'll need to **declare** that there is a such a func.
+ cpp does not link things form other **translation unit** if we never use it, yet it links things in current **translation unit** cause it may be used in another unit, add `static` may avoid this
+ common linking errors:
  + undefined ... : cannot find what to link, error starts with LNK
  + already defined/multiple definition : multiple definition so linker has no idea which to link
    + multiple definition will happens easily if we are not careful enough, ways to handle: **1.** use `static` so link in local unit, **2.**  `inline`, replace the function call with the function body **3.** put the definition in cpp file **4.** a library with only head file is a cool thing, this is what matplotlibcpp.h does: 
    ```C++
    #pragma once
    ```
    a concise version for `#ifndef #def...`, note: `#pragma` is not standard c/c++ feature
    + ...
+ ...

## Variable
+ what matter's for var is **size**
+ int : ~ -2 billion to 2 billion
+ unsigned int
+ char 1 byte: you are telling c that you want to treat this var as a charactor by using char
+ short 2 byte
+ int 4 byte
+ long 4 byte, depends on compiler
+ long long 8 byte
+ float 4 byte
+ double 8 byte
+ bool 1 byte, we can only address 1 byte
+ `sizeof()` to check size
+ ptr: * and &
+ ...

## Function
+ useful and takes cost

## Header Files
+ just declarations
+ `#pragma once` preprocess, only include once, this provides include sth. in **the same** translation unit multiple times so a duplicate error would occur
+ the classical way:
    ```C++
    #ifndef _HEADERFILE_H
    #define _HEADERFILE_H
        ...
    #endif
    ```
+ we have `" "` and `< >`, the former for relative path, the latter just has to be in one of the include directories.
+ ...

## debug issues

## Condition
+ 0 -> false, else -> true
+ fasten your code by replacing condition with calculation
+ ...

## Loops
+ for
+ while 
+ do while
+ ...

## Control Flows
+ continue
+ break
+ return

## Pointers
+ a pointer is an interger that stores a memory dress, a pointer for all types is just such an int (types do not matter too much)
+ memory is a linear 1 dim line
+ 0 is not a legal ptr
+ you can also use `NULL` or `nullptr`
+ & and *
+ when one use ptr for read and write, 2 things matter, **1**. where does the memory start(the value of ptr) **2**. how long the memory block and how to interprete the memory(the type of the ptr)
+ malloc and free memory:
    ```C++
    char* buffer = new char[8];
    memset(buffer, 0, 8);
    delete[] buffer;
    ```
+ ...

## References
+ ref are pointers in disguise, it's an alias. actually, reference is not a var incompiling.
  ```C++
  int a  = 1;
  int& ref = a; // create a reference for a
  // use ref as if it is a 
  ref = 2;
  ```
+  this is how you write a var in func with reference
    ```C++
    #include <iostream>

    void Increment(int& x){ // NOTE int&
        x++;
    }

    int main(){
        int a = 1;
        Increment(a);
        std::cout << a << std::endl;
    }
    ```
+  so reference like syntax sweets
+  notes:
   1. must assign a reference when it's created
   2. can't change a reference after it's created
+  ...

## Class
+ example:
    ```C++
    class Player{
        public: // private by default
            //attribute:
            int x,y;
            int speed;

            //method:
            void Move(int xa, int ya){
                x += xa * speed;
                y += ya * speed;
            }
    };

    int main(){
        Player player;
        player.x = 5; // can access only if this attr is public
        player.Move(1,2);
    }

    ```
+ "class are just for make our lives easier"


## Class and Struct
+ struct would take public and public inheritance by default, class would take private by default
+ struct and class is quite similar, it's a matter of style which one to use.
+ usually, `struct` for POD, `class` for more complecated purpose; class for inheriant

## Class (deeper)
+ an example `Log` class
    ```C++
    class Log{

        private:
            int m_Loglevel = Trace;

        public:
            const int Warning = 1;
            const int Error = 0;
            const int Trace = 2;

            void Setlevel(int level){
                m_Loglevel = level;
            }

            void warn(const char* msg){
                std::cout << "[Warn]:" << msg << std::endl;
            }
    };

    int main(){
        Log log;
        log.setLevel(log.Warning);
        log.warn("warnMsg");
        return 0;
    }
    ```
+ .

## Static(outside)
+ 2 meanings: 
  1. inside the class: a var that **all** instances of a class shares
  2. outside the class: linkage will be **internal**
+ eg:
    ```C++
    \\in exter.cpp
    static int var = 5;

    \\in main.cpp
    int var = 10;
    int main(){
        std::cout << var << std:endl;
    }

    //out: 10
    ```

    ```C++
    \\in exter.cpp
    int var = 5;

    \\in main.cpp
    extern int var;
    int main(){
        std::cout << var << std:endl;
    }

    //out: 5
    ```
+ a static variable is not available outside the translation unit that decalred this var
+ do more `static` and avoid `global` as much as possible
+ .

## Static(inside)
+ static variable: there will be only **one** instance of a static var across **all** instances of a class
    ```C++
    struct Entity{
        static int x=1, y=1;
    }
    
    int Entity::x,y; // have to declare

    int main(){
        Entity e;
        // so, instead of using
        std::cout << e.x << std::endl;
        // use this would be better
        std::cout << Entity::x << std::endl;
    }
    
    
    ```
+ static method: you cannot refer to the class instance with static method, cause it does **NOT** have an instance.
+ but if you input the instance to static method, it would works

## Local Static
+ lifetime
+ scope
+ local static: a var that lives during the whole program, yet it can only be accessed in the local scope
+ .

## Enum
+ a way to name values
+ enum must be `int`
+ example:
  ```C++
  enum Example{
    A = 0, B = 2, C = 6
  }

  // if like this
  enum E{
    A,B,C
  }
  // than A == 0, B ==1, C==2;

  // specify what kind of var you are using
  enum Enum : char{
    ...
  }
  ```
+ enum is not actually a namespace, if you create a enum `E` in Class `C`, you'll need to call it by C::(the enum name)

## Constructor
+ do init when create a instance
    ```cpp
    class C{
        C(){ //the constructor

        }
    }    
    ```
+ C++ provides default init that does nothing;
+ you have to init an instance **manually**
+ do not allow user to create an instance:
  ```CPP
  class C(){
    public:
        C() = delete;
  }
  ```
  or set the default constructor to be `private`
+ you can **overload** the constructor

## Destructor
+ it's `__del__()` for C++
+  example:
  ```C++
  class C{
    public:
        C(){ // constructor

        }

        ~C(){ //destructor

        }
  };
  ```
+  call destructor manually:
    ```C++
    C c; // create
    c.~C(); //destruct

    ```
+  .

## Inheritance
+ example:
    ```C++
    class Entity{ //base class
        public：
            float X,Y;
            void Move(double dx, double dy){
                X += dx;
                Y += dy;
            }
            
            Entity(){
                X = Y = 0.0;
            }

            void print(std::string s){std::cout << s << std::endl;}
    };

    class Player: public Entity{ // sub class
        public:
            const char* name;
            void print(std::string s){std::cout << s << "in player" << std::endl;}
    }

    // in int main()
    player p;
    p.name = "name";
    p.Move(2,3); //player is always a superset of `Entity` 
    ```
+ 1

## Virtual Functions
+ v-func allow us to overwrite methods of a subclass
+ if we put a `Player*` pointer p to a func `void f(Entity* e){e->print}`, the method of `Entity` will be called ( we want it call the method with same name of `Player`).
+ use v-func to fix this
    ```C++
    class Entity{ //base class
        public：
            float X,Y;
            void Move(double dx, double dy){
                X += dx;
                Y += dy;
            }
            
            Entity(){
                X = Y = 0.0;
            }

            // use virtual
            virtual void print(std::string s){std::cout << s << std::endl;}
    };


    class Player: public Entity{ // sub class
        public:
            const char* name;
            // use override
            void print(std::string s) override{
                std::cout << s << "in player" << std::endl;
            }
    }
    ```
+ v-func has relatively big cost

## Pure Virtual Function
+ define a unimplemented function in base class, and force subclass to implement it,this is also called **interface**
+ the pure v-func has to be implemented before create a instance
+ by using pure virtual function, we can garantee that a subclass have a perticular method

## Visibility
+ private: only class itself and its `friend` can access, others(even its subclass) can't do this;
+ protected: class and **all subclass** can access
+ public: access for all

## Arrays
+ example:
    ```C++
    int a[5];
    a[0] = 5;

    //we can do this
    int * ptr = a;
    ```
+ safety checks are important;
+ arrays store data continously;
+ create array on heap:
    ```C++
    int* ptrOfIntArr = new int[5];
    // this memory will be around until you delete it
    delete[] ptrOfIntArr；
    ```
+ **cannot** check the length of an array created on heap, but for those created on stack:
  ```C++
  int a[5];
  int asize = sizeof(a) / sizeof(int);
  // or
  asize = sizeof(a) / sizeof(a[0]);
  // or 
  asize = sizeof(a) /  sizeof(typeid(a[0]).name)
  asize = sizeof(a) / sizeof(decltype(a[0]))
  ```
  > it's interesting that the output of `decltype` of a `pointer` is a `reference`
+ if pass the an array to function, it is actually a pointer, so you'll need to maintain size of your array by yourself(kind of sucks),use `std::array` in practice would be easier as includes boundary checking and can check size
+ use array
  ```C++
  static constexpr int length = 5; // need const to fix the size
  //OR
  static const int length = 5;
  // constexpr is fixed during compiling 
  int a[length];
  ```

## Strings 
+ a group of characters
  ```C++
  // C style
  const char * name = "Yuki"; // do not use `delete`
  // CANNOT do this
  name[2] = 'U';
  ```
+ Cpp find where a char ends by finding 00(HEX), 
+ by hand
    ```C++
    char name[5] = {'Y', 'u', 'k','i','\0'};    
    ```
+ std::string is templete specilization of basicString, use this in practice
+ example
  ```C++
  #include <string>
  std::string name = "Yuki"; // input char* or const char*
  std::cout << name; 
  int sizeofname = name.size();
  name += " hello!";
  bool contains = name.find("aaa") != std::string::npos;
  ```
+ pass string to function
  ```C++
  //avoid copy by const reference
  void printStr(const std::string & string){
    ...
  }
  ```
+ 1

## String Literals
+ "Yuki" is a string literal, add'\0' implicitly
+ allow
  ```C++
  char name[] = "name"; // init with const char[]
  name[1] = A;
  char * Name = "name";
  ```
+ no not allow (although may be feasible)
  ```C++
  Name[2] = 'A'; // change a read-only memory, this is undefined behavior
  //this change may not work in Release mode
  //so actually init with `const char* Name = "name"`

  name = "NAME"; // change a char[] with const char[]
  ```
+ other chars:
  ```C++
  const wchar_t* name = L"name"; // 2byte
  const char16_t* name = u"name"; // 2byte
  const char16_t* name = U"name"; //4byte
  ```
+ other
  ```C++
  using namespace std::string_literals;
  std::string name0 = "Yuki"s + "hello";
  //s is actually a function

  ```
+ example
  ```C++
  const char * example = R"(Line1
                            Line2
                            Line3)";
  ```

## Const
+ "fake" key word
+ promise it can't be changed, **but** you may change it
+ example
  ```C++
  const int MAX_AGE = 90;
  ```
+ ptr with const
  ```C++
  // bypass the const promise
  int * a = new int;
  a = (int*)&MAX_AGE；
  
  delete a;
  ```
  ```C++
  const int * a = new int; // a pointer points to const int
  //so now we can't do this
  *a = 2;
  // but we can
  a = &MAX_AGE;
  delete a;
  ```
  ```C++
  int* const a = new int; //a const pointer points to int, 
  //can do 
  * a = 2;
  //cannot do 
  a = (int*)&MAX_AGE;
  ```
  `const int * a` is the same as `int const * a`, if we don't want to change the ptr, put `const` after `*`, i.e., `int* const a`
+ const with class
  ```C++
  class Entity{
    public:
        int x, y;
        mutable int k;

        int GetX() const { // use const to promise that we'll not change the instance
            // use const in getter is good
            return x;
            // can't do 
            // x = 1;

            //CAN do 
            k = 3;
            //because k is mutable
        }

        // use 3 const in 1 line:
        int * ptr, fakeptr;
        // actually fakeptr is an int

        const int* const GetPtr() const {
        // return a unchangable ptr which points to a unchangable int memory ; and we promise we'll not change the instance in this method
            return ptr;
        }

  }
  ```
+ mathods with const is useful, you can only call const method with const reference
  ```C++
  void printEntity(const Entity& e){//input a const ref of class entity
    // we can call this methpd as it has const
    // if a method is not not marked with const, then is may change the instance, BUT what we got is a const ref, so error will occur
    std::cout << e.GetX() << std::endl;
  }
  ```
+ BUT BUT we can change `mutable` var in const method:1
  ```C++
  
  ```

## Mutable
+ `muatable` kind like reverse the meaning of const
  ```C++
  class Entity{
    private:
        std::string name;
        mutable int cnt = 0;
    piblic:
        const std::string& Getname() const{
            cnt++; // can do this as it is mutable
            return name;
        }
  }
  ```
+ `muable` in `lambda`
  ```C++
  int x = 9;
  auto f = [=]() mutable { // value [&] for reference
    x++; // a copy
    std::cout << x << std::endl;
  }
  // x is still 9 here
  ```

## Initializer List
+ init memers with it
  ```C++
  class Entity{
    private:
        std::string name;
    public:
        Entity(){ // the default constructor
            name = "UnNamed";
        }

        Entity(const std::string& Name){
            name = Name;
        }

  }

  //OR 
  class Entity{
    private:
        std::string name;
        int number;
    public:
        Entity()
            : name("UnNamed"), number(0); // DO write IN ORDER
        { }// the default constructor
        

        Entity(const std::string& Name)
            : name(Name)
        {}

  }
  ```
+ why this ? with `initializer list`, we are creating the member only once, with `=` in constructor, we do **1.** construct this member with default constructor **2.** construct a **new** member with input and replace the latter member 
+ you should use `initializer list` EVERYWHERE, it has functional difference

## Ternary Operators
+ `? :`
+ example
    ```C++
       Speed = condition ? A : B; // A if true and B if false 
    ```
+ cleaner and a little faster

## Create Object
+ where to instaniate? 
+ usually `heap` and `stack`
+ things in stack will be freed out of scope
+ things in heap will be freed until **you** decide it need to be deleted
+ example
  ```C++
  Entity e; // construct with default constructor
  Entity e1("AAA"); // construct with a constructor whose input is a string
  ```
+ init on stack if you can
+ but there will be occations where we need to use heap
  1. make the instance live longer
  2. the instance is too big that init it cost a lot, but the stack is **small**
+ init on heap:
  ```C++
  Entity* e2 = new Entity("name"); //mallocate on heap

  //use 
  (*e2).method();

  //or 
  e2->method();

  // we are now RESPONSIBLE for manage this memory
  delete e2;

  ```
+ 1

## New
+ `new` is for allocate memory on heap
+ new will ask the std library to find a specific size of **continous** memory, and return the pointer of the memory
+ example
  ```C++
  int* b = new int;
  int* b = new int[50];
  Entity* e = new Entity(input);
  ```
+ new does not only allocate memory, but also call the constructor
+ usually, calling new calls `void * ptr = malloc(SIZE)`, `new` = call the malloc and call the constructor of a class;
+ MUST use n`ew` AND `delete`
+ delete calls `free` and `destructor`
+ use `delete[]` to free memory allocated for array
+ placement new: `new(ptr) ...`uses the memory allocated for ptr

## Implicite and Explicite
+ example
  ```C++
  class Entity{
    public:
      int age;
      std::string name;

      explicit Entity(const std::string& Name)
        : age(-1), name(Name), 
  }


  int main(){
    //we can do this
    Entity e = "NAME";
    // it actually calls the constructor implicitely to get an entity instance
    // ONLY 1 time it works
  }
  ```
+ `explicite` key word is used to forbid implicite conversion
+ example
  ```C++
  class Entity{
    public:
      int age;
      std::string name;

      Entity(const std::string& Name)
        : age(-1), name(Name), 
  }


  int main(){
    //we can NOT do this any more because `explicit`
    Entity e = "NAME";
    // it actually calls the constructor implicitely to get an entity instance
    // ONLY 1 time it works
  }
  ```

## Operator and Overloading
+ operators are common
+ operators are functions actually
+ example
  ```C++
  struct Vec{
    float x, y;

    Vec(float x, float y)
      :x(x), y(y) {}

    // overload +,
    // v1 + v2 = v1.operator+(v2)
    Vec operator+(const Vec& other) const{ // add const to promise not to change the class
      return Vec(x + other.x, y + other.y);
    }

    //we can call the operator as well
    Vec add(const Vec& other) const{
      return *this + other;
      // or this, operator+() is the func we defined earlier
      return operator+(other);
    }

    // must use friend, 
    // so it will be operator<<(stream, other), rather than stream.operator<<.(other)
    // if we don't want to use friend, we'll need to define this outside the struct
    friend std::ostream& operator<<(std::ostream& stream, const Vec& other){
      stream << other.x << ", " << other.y;
      return stream;
    }

    bool operator==(const Vec& other) const{
      return other.x == *this.x && other.y == *this.y;
    }
  };

  int main(){
    Vec v1(1,2);
    Vec v2(3,4);

    //after overloading +, we can
    Vec v3 = v1 + v2;

    //after overloading <<, we can
    std::cout << v3 << std::endl;
  }
  ```

## this
+ this is a pointer which points to the current instance
  ```C++
  class Entity{
    public: 

    int x, y;

    Entity(int x, int y)
      : x(x), y(y) {}

    //OR
    // use this to clarify which x we are refering to 
    Entity(int x, int y){
      Entity* const thisptr;

      thisptr->x = x;
      this->y = y;
    } 
  }
  ```

## Object Lifetime (on Stack)
+ every object decalred in stack will **gone** at the end of the scope
+ examples
  ```C++
  // this does NOT work as arrary is cleared outside the func
  int* CreArr(){
    int array[50];
    return array;
  }
  ```
  automatically delete a var on heap
  ```C++
  class ScopedPtr{
    private:
      Entity * Ptr;
    public: 
      ScopedPtr(Entity* Ptr)
        : Ptr(Ptr) {}
      
      ~ScopedPtr(){
        delete Ptr;
      }
  }

  int main(){
    {
      ScoptedPtr e(new Entity());
    } // at the end of the scope, e is cleared, so the destructor is called to delete the pointer
  }
  ```
+ for example, you can write a **timer** which stops timming at the end of the scope
+ function lock

## Smart Pointers
+ a wrapper of a real pointer
+ `unique_ptr` delete at the end of a scope, you **cannnot** copy unique_ptr
  ```C++
  #include <memory>
  int main(){
    std::unique_ptr<Entity> e(new Entity()); //forced to call explicitely
    //OR this can be better, slightly safer
    std::unique_ptr<Entity> = std::make_unique<Entity>();
  }//unique_ptr will delete at the end of the scope
  ```
+ shared_ptr: can be copied, accomplished with **reference counting**, delete the memory until the last reference is died
  ```C++
  //a bit less efficient by using make_shared, cause shared pointer needs to allocate a block of memory called `control block`
  std::shared_ptr<Entity> sE = std:make_shared<Entity>();
  ```
+ week_ptr: will **not** increase the ref count if you copy it, `week_ptr` will **not** keep the memory alive， yet you can use `.shared()` to check if it is expired 
+ use `unique_ptr` as much as you can as it has less overhead, use `shared_ptr` if you can't use `unique_ptr`

## Copying and Copy Constructor
+ copy is useful and takes time
+ you are always copying by using `=` except reference(reference is alias)
+ you may do `shallow copy` by copying the pointer pointing to the same memory
+ `deep copy` is we need, this is what copy constructor does, which would be called if we assign a class instance to anpther instance of the same class 
  ```C++
  class String{
    public: 
      //...


      //the copy constructor
      //Cpp provides us the copy constructor by default which does shallow copy:
      //memcpy(this, &other, sizeof(String));
      String(const String& other)
        : m_Size(other.m_Size)
      {
          // do deep copy
          buffer = new char[m_Size + 1]; // allocate new memory
          memcpy(m_Buffer, other_m_Buffer,m_Size);
      }

  }
  ```
+ **always** use const reference to avoid unecessary copy

## Arrow Operators
+ use `->` for pointers of class or struct
+ example
  ```C++
  class ScopedPtr{
    private: 
      Entity * Obj;
    public:
      ScopedPtr(Entity* entity): obj(entity) {}
      ~ScopedPtr() {delete obj};

      //override ->
      //p->m is interpreted as (p.operator->())->m
      Entity* operator->(){
        return Obj;
      }
      //now we can use-> for this class as if it is a ptr

      //const version
      const Entity* operator->() const{
        return Obj;
      } // 
  }
  ```
+ p->m is interpreted as **(p.operator->())->m**， `->` must return a pointer or an property of a class
+ see the offset of a struct
  ```C++
  struct vec3{
    double x, y, z;
  }

  int main(){
     int offsetX = (int)& ( (vec3*)nullptr -> x );
     //1. cast nullptr to vec3*
     //2. get the x component of the vec3 ptr
     //3. get the address of the x component
     //4. cast it into int
  }
  ```

## std::vector
+ a dynamic array
  ```C++
  #include <vector>

  std::vector<int> ints;

  ints.push_back(1);
  ints.push_back(2);
  //...
  int size  = ints.size();

  for (int& num : vertices) // do not copy data
      std::cout << num << std::endl;

  ints.erase(ints.begin() + 1); //remove the index 1 , number 2
  ```
+ std::vector store things **in a line**. So it's a bit optimal to store instances(not pointers to them) in vector in terms of speed;

## std::vector optimization
+ optimization is based on knowing how it works
+ when `push_back()`, cpp allocate a new memory which is sufficient for all element in the vector, copy the former memory and delete it
+ reallocate leads to BAD performance
+ exmaple
  ```C++
  class Vertex{
    public:
     int a;
    
     //constructor
     Vertex(int a): a(a) {}

      // copy constructor
     Vertex(const Vertex& other): a(other.a){
        std::cout << "copied" << std::endl;
     }
  };


  int main(){
    std::vector<Vertex> v;
    v.push_back(Vertex(1)); 
    // this takes ONE copy, we init Vertex(1) in main stack and COPY it to v,
    // so we can init the vertex in the memory where v allocated, OPT1
    v.push_back(Vertex(2));
    //this takes TWO copy, one is to move Vertex(2) from main to v, another is to reallocate v and copy memories of Vertex(1)
    // so we can actually preallocate enough memomry to avoid reallocate, OPT2



    // optmize
    std::vector<Vertex> V;
    V.reserve(3); // preallocate enough memory for 3 Vertex instance, OPT2
    // if we do not reserve, cost will grow exponantially
    V.emplace_back(1);
    // take the arguments and construct directly in memory of V, OPT1
    V.emplace_back(2);
  }
  ```

## Static Library
+ only binary lib is considered here
+ static and dynamic: static complies your lib into the exe file(only exe file), dynamic link the lib at run time(need exe and dll/so)
+ static linking is faster as compiler can do optmization 
+ inlcude the path of the header file
  ```Cmake
  target_include_directories(MyPrj PRIVATE "include/")
  # or global
  include_directories("include/")
  ```
  use `"*.h"` if the header is in the proj and `<*.h>` if it is not
+ but the compiler can **NOT** find where are your functions declared in .h files (if .h file does not have the definition of the functions)
+ so we need to **LINK** to the lib to find the definition
  ```CMake
  ### add a library with files in MY_HEADER
  add_library(MyLib ${MY_HEADER})
  ### link it to executable MyPrj
  # if not, exe knows there's some funcs (cause it can access headers in include),
  # but it can't find where the things are
  target_link_libraries(MyPrj PRIVATE MyLib)
  ```
+ in general, we need **header files** for **declaration** and **library files** for **definition** (can put definition to header files as well)

## Dynamic Libraries
+ linking that happens at run time
+ 2 types, **1.** exe knows the declaration but not the definition before it runs **2.** exe don't even know the declaration before it runs
+  the dir of the exe file is the automatic searching path of dll, so it works if you put the dll file with the exe file
+  

## Create and Use Libraries
+ see video and CMakeLists.txt

## Multiple Return
+ can pass reference to the function (or a pointer so you don't need pass an var)
+ or return an array by address (DO allocate the var on heap so the var won't be cleaned at the end of the scope)
+ can NOT return arr by value(like `int a[3] = {1,2,3,}; return a`), this is actually returning an pointer
+ return std::array, std::array will keep the value and will not become pointer when returning
+ return std::vector (array is created on stack yet vector is created on heap), so technically return an array is faster
+ when is comes to 2 vars may or may **not** have the same type: **tuple** and **pair**
+ **tuple** is a class with X vars and it does not care about the type
  ```C++
  #include <tuple>

  std::tuple<int, float, std::string> my_function() {
      int i = 42;
      float f = 3.14;
      std::string s = "Hello";
      return std::make_tuple(i, f, s);
  }

  int main() {
      auto result = my_function();
      int i = std::get<0>(result);
      float f = std::get<1>(result);
      std::string s = std::get<2>(result);
      // do something with i, f, and s
      return 0;
  }

  ```
+ `pair` is quite the same:
  ```C++
  #include <utility>

  std::pair<int, float> my_function() {
      int i = 42;
      float f = 3.14;
      return std::make_pair(i, f);
  }

  int main() {
      auto result = my_function();
      int i = result.first;
      float f = result.second;
      // do something with i and f
      return 0;
  }
  ```
  c++17 provides this:
  ```C++
  auto t = std::make_tuple(1,0.5,"111");
  auto [a,b,c] = t; // C++ 17 only
  std::cout << ++a << std::endl;
  ``` 
+ can also use struct, which have more comprehensive expressions

## Templates
+ `generics` in other languages
+ let compiler write code for you based on given rules
+ a bit like `macro`
+ example
  ```C++
  template<typename T> // declare that this a template, T is template var name
  //can replace `typename` with `class`
  void Print(T value){
    std::cout << value << std::endl;
  }
  // this REAL function is created when it is called
  // it is not even exist until we call it

  //then, we can 
  int main(){
    Print(5); //int
    Print<int>(5); // call Print with type int, <.> is not needed if the type can be deduced
    Print(.5); //double
    Print("string"); //const char *
  }
  ```
  ```C++
  template<typename T, int N>
  class Array{
    private:
      T arr[N];
    public:
      int GetSize() const {
          return N;
      }
  }

  //use
  Array<5> arr;
  ```
+ where to use and where not: useful but DO NOT go too far

## Stack and Heap
+ these two areas are both in memory, stack is usually about 2M big
+ vars allocated in stack are quite close, the stack pointer **moves** certain length when new memory is allocated, as a result stack allocation is quite fast
+  `new `calls `malloc`, `malloc` find the free memory be means of `free list`, 
+  alllocate in heap is a **whole** thing, yet allocate in stack is **just** a CPU instrucation
+  as vars in stack are quite near, they can be put into the CPU Cache Line
+  use heap only if you can't use stack 
+  it is the `new` process makes activities on heap slow, usually `cache miss` is neglegable
+  

## Macros
+ macro does preprocess before compiling
+ pure text replacing
+ example
  ```C++
  #define WAIT std::cin.get() // bad, you don't want to confuse others

  #ifdef DEBUG
  #define LOG(x) std::cout << x << std::endl
  #else
  #define LOG(x) // LOG(x) will be replaced by blank
  ```
  this is good to combine with make
  ```CMake
  # DEBUG will be defined in Debug mode 
  if(CMAKE_BUILD_TYPE MATCHES Debug)
      add_compile_definitions(DEBUG)
  else()
      add_compile_definitions(RELEASE)
  endif()
  ```
  **NOTE**: set DEBUG true or false will be more safe
  ```C++
  #if DEBUG == true
    #define LOG(x) std::cout << x << std::endl
  #else
      #define LOG(x)
  #endif
  ```
  ```CMake
  # DEBUG will be set true in Debug mode 
  if(CMAKE_BUILD_TYPE MATCHES Debug)
      add_compile_definitions(DEBUG=true RELEASE=false)
  else()
      add_compile_definitions(RELEASE=true DEBUG=false)
  endif()
  ```
+ use macro to comment:
  ```C++
  #define dbg true
  #if dbg:
  ...
  #endif
  ```
+ use back slash `\` to continue define in a new line
  ```C++
  #define DBG \
    true
  ```

## Auto
+ `auto` will deduce the type of the var
+ example
  ```C++
  std:vector<std::string> s;
  s.push_back("apple");
  s.push_back("banana");

  for (std::vector<std::string>::iterator it = s.begin(); 
      it != s.end(); it++ ){
        std::cout << *it << std::endl;
      }

  // auto instead
  for (auto it = s.begin(); it != s.end(); it++ ){
        std::cout << *it << std::endl;
      }
  ```
+ on the one hand , the api can do little change by using auto, on the other hand, this may inflect the following code usin the features of what api returns
+ use auto if there is looong type or the type is clear enough

## std::array
+ a **static** array
+ example
  ```C++
  std::array<int, 5> data;
  ```
+ std::array is very like old arr[], they have the same style
+ C array do not manage its size it self, but for std::array, we can use `arr.size()`
+ input an array with out giving the size : **template**
+ `arr.begin()` `arr.end()`, `std::sort()`
+ std::array on stack, std::vector on heap
+ std::array provides bounds check in **debug mode** (not release mode)
+ `arr.size()` returns the template argument you give to it **directly**
+ use std::array instead of origin array

## Function Pointers
+ "raw" style form C first
+ assign func to a var
+ pointers have the address of the CPU instructions in the func
+ example
  ```C++
  void func(){
    std::cout << "Hello World!" << std::endl;
  }

  void funcN(int a){
    for(int i=0; i<a; i++)
      std::cout << "Hello World!" << std::endl;
  }

  int main(){
    auto funcptr = func; //call with (), get the address without ()
    // do not need to use &func as an implicite conversion is done here
    //OR
    auto funcPtr = &func; // use & to clarify it is an function ptr
    //OR with explicite type
    void(*funcName)() = func;
    // since this is a little wierd, use `typedef` instead
    typedef void(*FuncPtr)();
    //or 
    using FuncPtr = void(*)();
    FuncPtr f = func;

    // call function with the pointer
    funcptr();


    // for func with input:
    using funcNptr = void(*)(int);
    funcBptr funcHandle = &funcN;
    //call
    funcHandle(5);
  }
  ``` 
+ useful example
```C++
#include <vector>

void PrintValue(int value){
  std::cout << "Value: " << value << std::endl;
}

// a really flexible func
template<typename T> 
void ForEach(const std::vector<int>& values, T funcPtr){
  for (int value : values){
    funcPtr(value); //a funcPtr type whose input is 1 int
  }
}


int main(){
  std::vector<int> values = {1,2,3,};
  ForEach(values, &PrintValue);

  // PrintValue is a bit clunky here, use lambda
  ForEach(values, [](int value){std::cout << "Value: "<< value << std:endl; });
}
```


## Lambda
+ you can use lambda when you need a function pointer
+ example
  ```C++
  [](int value){   }
  ```
+ [] for capture, **how** we pass vars(in the scope) to func
  ```C++
  []      // 沒有定义任何变量。使用未定义变量会引发错误。
  [x, &y] // x以传值方式传入（默认），y以引用方式传入。
  [&]     // 任何被使用到的外部变量都隐式地以引用方式加以引用。
  [=]     // 任何被使用到的外部变量都隐式地以传值方式加以引用。
  [&, x]  // x显式地以传值方式加以引用。其余变量以引用方式加以引用。
  [=, &z] // z显式地以引用方式加以引用。其余变量以传值方式加以引用。
  [this]() { this->someFunc(); }(); // use this for class
  ```
+ the type of lambda: `std::function<outputType(inputType)>`, you can also turn C style function pointer to std::function (`#include <functional>` first)
+ find if an element of an vector satisfy a lambda expression
  ```C++
  std::vector<int> values = {1,2,3,};
  auto it = std::find_if(values.begin(), values.end(), [](int value){ return value > 2 } )
  // *it == 3
  ```

## Why NOT `using namespace std`
+ `using namespace std` makes it hard to know which library the symbol is in 
+ this may lead to **silent** runtime error
+ **DO NOT** use `use namesapce` in header file, it would be really hard to track
+ minimize the scope where you use `using namespace`

## Namespace
+ tackle identical signiture problem
+ example
  ```C++
  namespace yourSpace{
    ......
  }

  //use
  yourSpace:: ...
  ```
+ class is a namespace aswell
+ example
  ```C++
  //MINIMIZE ITS SCOPE

  using namespace ... ;
  using apple::print;

  namespace a = apple;
  a::print();
  ```

