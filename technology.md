---
layout: post
title: Interface Vision - The Technology
tagline: Programming Without Coding
---
{% include JB/setup %}

{% assign posts_collate = site.posts %}

# About Our Technology

## The Vision - Visual Programming

> Eventually, programming will be done by composing software visually: not through coding. This is inevitable.

## Part 1 - Interface Vision and SIP

### Introduction

Interface Vision is a visual object language and fully [composable](http://en.wikipedia.org/wiki/Composability) object framework. Instead of writing [source code](http://en.wikipedia.org/wiki/Source_code), programmers visually connect systems/objects [(1)](#id-1) to each other: specifically their <a href="http://en.wikipedia.org/wiki/Property_(programming)">properties</a>.

The visual object language and framework are being developed using a [programming paradigm](http://en.wikipedia.org/wiki/Programming_paradigm) we developed called Simple Interface Programming (SIP).

Our framework is written in C# using [Mono](http://www.mono-project.com/Main_Page), [Monodevelop](http://monodevelop.com/), [Mono Touch](http://xamarin.com/monotouch) and [Visual Studio](http://www.microsoft.com/visualstudio/eng).

So, before we delve into Interface Vision, let's talk about the programming paradigm SIP.

### What Makes SIP Different?

SIP does not use functions or <a href="http://en.wikipedia.org/wiki/Method_(computer_science)">methods</a>. Yes. Really. Our framework has tens of thousands of lines of source code and we have **one** function:

    static int main { ... }

Wait. If we have no functions or methods, then how do we:

* **define** the behavior of a system?
* **pass** external information to a system?
* **initialize** a system?

Want to know more?

* [Why We Only Use Properties](http://blog.interfacevision.com/technology/only-properties/) - Explains why we decided subroutines with parameters are bad.

#### Where We Put Behavior

SIP use <a href="http://en.wikipedia.org/wiki/Property_(programming)">properties</a>, instead of methods, to define system behavior. Notice, in [Source 1.1](#id-s1-1), withLong contains the specific task to add:

    ...
    public long withLong {
    	get { return opLeft.withLong + opRight.withLong; }
    }
    ...

Stated another way, we do have subroutines (methods, functions, etc.) but we don't allow them to receive parameters: no [method signatures](http://en.wikipedia.org/wiki/Method_signature).

Want to know more?
    
* [Methods, Properties and Abstractions](http://blog.interfacevision.com/technology/methods-and-abstraction/) - Explains how methods and properties differ as abstractions.


#### How We Pass Information To Systems 

In SIP, we don't really pass external information into a system. Instead, we "pull" that information into a system. How do we do this?

Traditionally, properties and <a href="http://en.wikipedia.org/wiki/Attribute_(computing)">attributes</a> contain information internal to a system (object/class/module/etc.). External information is provided via methods with parameters. However, within SIP, a property can contain a reference to external information.

This "reference" is actually a specific type of system/class which is able to locate information contained within [Composite Centric Memory (CCM)](#id-composite-centric-memory). We call these locators and each locator is able to find information within an associated data structure. For example, a HashTable would have a HashTableLocator.

Want to know more?

* [Locators](http://blog.interfacevision.com/technology/locators/) - Explains how locators are used to find information within CCM.

#### How Do We Initialize A System?
Since Interface Vision is a [visual programming language](http://en.wikipedia.org/wiki/Visual_programming_language), users drag and drop a system/class to create instances. Usually, a user will attach a new instance to a property of an existing instance.

We store and load all of these attached instances in a configuration file using [serialization](http://en.wikipedia.org/wiki/Serialization). An example of just such a configuration is shown in [Source-1.5](#id-s1-5)

However, it is possible to code the configuration as shown in [Source-1.3](#id-s1-3).

### An Example Configuration Using Internal Information
Let’s say we want to add two numbers (longs, integers, floats, etc.). Addition takes a left operand and a right operand. Traditionally, add is defined as a function (or a method) as follows:

    long add ( long opLeft, long opRight ) {
      return opLeft + opRight;
    }

    int add ( int opLeft, int opRight ) {
      return opLeft + opRight;
    }

    float add ( float opLeft, float opRight ) {
      return opLeft + opRight;
    }

However, in SIP, you aren't allowed to use parameters. Instead we use properties.

We create an Add system (class) with properties as shown [(2)](#id-2):

    public interface IItem {
    	long withLong { get; set; }
    	int withInt { get; set; }
    	int withFloat { get; set; }    	
    }

    public class Add : IItem {
    	public IItem opLeft { get; set; }
    	public IItem opRight { get; set; }
    	public long withLong {
    		get { return opLeft.withLong + opRight.withLong; }
    	}
    	public long withInt {
    		get { return opLeft.withInt + opRight.withInt; }
    	}
    	public long withFloat {
    		get { return opLeft.withInt + opRight.withInt; }
    	}
    }
###### Source-1.1: The Add system can add two numbers. {#id-s1-1}

The first thing to note is that the properties opLeft (the left operand of add) and opRight (the right operand of add) are not primitive data types. They are of type IItem.

    ...
  	public IItem opLeft { get; set; }
  	public IItem opRight { get; set; }
    ...

This means if we want to add two long primitive data types (or integers or floats), we will need to call the withLong (withInt, withFloat) property of IItem. This is seen within the implementation of withLong, withInt and withFloat of Add.

    ...
  	public long withLong {C
  		get { return opLeft.withLong + opRight.withLong; } /* TODO: Check for overflow */
  	}
  	public long withInt {
  		get { return opLeft.withInt + opRight.withInt; } /* TODO: Check for overflow */
  	}
  	public long withFloat {
  		get { return opLeft.withInt + opRight.withInt; } /* TODO: Check for overflow */
  	}
    ...

This probably means we will need a Long system, Int system and Float system to hold the value of a primitive data type.

    public class Long : IItem {
    	public long value { get; set; }
    	public long withLong {
    		get { return value; }
    	}
    	public int withInt {
    		get { return (int)value; } /* TODO: Check for overflow */
    	}
    	public float withFloat {
    		get { return (float)value; }
    	}
    }

    public class Int : IItem {
    	public int value { get; set; }
    	public long withLong {
    		get { return value; }
    	}
    	public int withInt {
    		get { return value; }
    	}
    	public float withFloat {
    		get { return (float)value; }
    	}
    }
    
    public class Float : IItem {
    	public float value { get; set; }
    	public long withLong {
    		get { return (long)value; } /* TODO: Check for overflow */
    	}
    	public int withInt {
    		get { return (int)value; } /* TODO: Check for overflow */
    	}
    	public float withFloat {
    		get { return value; }
    	}
    }

###### Source-1.2: A Long can contain a long primitive data type.

So, let’s see how we compose addition using C# object initializers:    

    static void main() { 
    	IItem myProgram = new Add {
    		opLeft = new Long { value = 5 },
    		opRight = new Long { value = 7 }
    	};
    	long result = myProgram.withLong;
    }
###### Source-1.3: Our first program simply adds two numbers. {#id-s1-3}
The result of adding two numbers is found by simply calling myProgram.withLong.

    long result = myProgram.withLong;
    
We could use the same composed system to get the result as an integer.

    int result = myProgram.withInt;

We can visually represent our add configuration using a diagram.

<p class="pagination-centered"><img class="img-polaroid" src="/assets/img/technology-system-add-internal-example.png"><img></p>
###### Figure-1.1: An example of adding two numbers internal to a system.

In this example, the information located in the properties is internal to the system. The values are just parts of type Long assigned to properties. Let’s look at an example where the information is external.

### An Example Configuration Using External Information

Figure-1.2 is a form that has two fields. When we press “Add” on that form, two numbers are summed ([3](#id-3)).

<p class="pagination-centered"><img class="img-polaroid" src="/assets/img/technology-system-add-form.png"><img></p>
###### Figure-1.2: A form which adds two numbers.

We are only allowed to compose programs and we can’t provide information via a public method with parameters. So, we need to create a composable system/object type that can locate information on a form called FormValue ([4](#id-4)).

    public class FormValue : IItem {
    	public string nameForm { get; set; }
    	public string nameField { get; set; }
    	public long withLong {
    		get { return Ccm.shared[nameForm].field[nameField].withLong; }
    	}
    }
###### Source-1.4: The FormValue Part is able to retrieve a value from the field of a form.

The example code in Source-1.4 is almost boilerplate except the global object called Ccm (which stands for [Composite Centric Memory (CCM)](#id-composite-centric-memory) ).

    ...
    get { return Ccm.shared[nameForm].field[nameField].withLong; }
    ...

Here is an example usage ([6](#id-6)):

    static void main() { 
      IItem myProgram = new Add {
        opLeft = new FormValue {
          nameForm = “AddForm”,
          nameField = “opLeft”
        },
        opRight = new FormValue {
          nameForm = “AddForm”,
          nameField = “opRight”
        }
      };
      long result = myProgram.withLong;
    }
###### Source-1.4: The values to add are external to the Add part.

A visual representation of this example is shown in Figure-1.3.

<p class="pagination-centered"><img class="img-polaroid" src="/assets/img/technology-system-add-external-example.png"><img></p>
###### Figure-1.3: An example of adding two numbers external to a system.

### Simplified Interfaces, Decoupling and Code Reuse

What is interesting about the examples is that they all have a similar <a href="http://en.wikipedia.org/wiki/Interface_(computing)">interface</a>: they all look the same to an external observer. However, internally, these systems are doing completely different things. 

In the first example, when Add calls withLong of the instance plugged into the opLeft (or opRight) property, a value is simply returned from the Long. However, in the second example, Add ends up using an object that references a form. Two completely different behaviors: but externally they look the exact same.

Since the interface is the exact same, Add is fully unaware of where the values being added are coming from. This means that the system to Add is not “connected” to the system that finds the value on the form for us.

This is a desired feature of software languages and frameworks. A way to [decouple](http://en.wikipedia.org/wiki/Decoupling) systems from each other [7](#id-7). An advantage of decoupling is that sub-systems are more reusable when they are decoupled from other sub-systems.

### Greatest Disadvantage of Traditional Programming

The greatest disadvantage of traditional programming is that communication between systems can not be generalized. Every method or function leads to greater specialization of the framework’s communication system. As the framework grows, so does the complexity of the Universe’s communication system. We can’t help it. This is an intrinsic aspect of traditional programming methodologies: information is communicated through specialized interfaces.

### Generalized Solution

Figure-1.4 is a generalized Simple Interface Object. What is important to note is that the interface contains a single withSystem property used for both input and output. This means **every type** has the exact same interface [8](#id-8). This makes them a lot easier to use than traditional method based objects which have specialized, and thus unique, interfaces.

<p class="pagination-centered"><img class="img-polaroid" src="/assets/img/technology-system-generalized.png"><img></p>
###### Figure-1.3: Figure-1.4: All Simple Interface Objects have the generalized interface.

Using traditional development methods, you end up with thousands of objects: each with their own unique signature/interface. With SIP, you end up with thousands of objects with the same generalized signature/interface.

### Loading and Saving Programs Through Persistence

If programs are created visually, then how are they saved? Are they compiled or interpreted like traditional programs? Programs created using Interface Vision are neither compiled or interpreted. Programs are stored as data in different formats[8](#id-8). The program is loaded from one of the stored data formats and deserialized into objects that form the executable program. The objects have already been written and compiled so there is no need to compile a program you’ve just created. In fact, the program is always running: even while you are developing it.

Here is an example of a program described and stored json (a standard format used to store the state of an object).

    {
      "$type": "Add",
      "opLeft": {
        "$type": "FormValue",
        "nameForm": "AddForm",
        "nameValue": "opLeft",
      },
      "opRight": {
        "$type": "FormValue",
        "nameForm": "AddForm",
        "nameValue": "opRight",
      },
    }
###### Source-1.5: A Configuration using json. {#id-s1-5}

What is really interesting about this json is that it looks very similar to the example C# usage code . In fact, there is an almost one-to-one relationship between the persisted version a program and the program written using C# object initializers.

This one-to-one relationship also greatly simplifies database usage (both Sql and non-sql solutions).  The only difference between traditional objects and functions and relational databases was that traditional objects have methods: they both have properties. Since Simple Interface Objects have no methods, they can also be viewed as relations.

### Software Layers Using SIP

Using SIP, the software layers are differ slight from traditional programming methodologies as seen in Figure-1.5.

<p class="pagination-centered"><img class="img-polaroid" src="/assets/img/technology-system-software-layers.png"><img></p>
###### Figure-1.5: Software layers using SIP.

**Layer 4** is the software framework. It contains all the object types that are available to the developer when programming.  A drawback is that if a new type of object is required, the developer would need to update or add it to the framework.

**Layer 5** is a domain specific program created by visual composition [10](#id-10). The result of the program is a data file in the form of json, xml or other format. This file can be stored anywhere: even in the cloud or in a database. It can be loaded in different environments such as iOS, Android, OSX and Windows.

The domain specific business logic is 100% removed from the programming language and framework. This makes it easier to create cross platform applications (among other advantages).

### Some Advantages
Interface Vision has some of the following advantages over traditional methods of development:

* A much simpler framework for software integrators to work with: especially from a visual standpoint [11](#id-11).
* A reduction in complexity and specialization making it easier to learn.
* Programs can easily work with each other: even if developed by different teams in different companies.
* A one-to-one relationship when persisting. This includes sql and non-sql stores.
* 100% decoupled objects allowing for much greater code reuse.
* Removal of domain specific logic/business logic from source code.
* Programs that are always on: changeable during run-time. This means immediate feedback. Changes are seen instantly. No need to code/compile/test/run.
* Lower development costs.
* Much easier to debug.
* Composing a program is easier for systems analysts. UX designers are also able to mock out actual solutions that can be easily filled out by programmers.
* Parallel processing is easy to configure.
* The ability to inject behavior anywhere within your program: anytime and anywhere. You can “grow” programs.
* For the framework itself, less source code than would be required using traditional programming methodologies.
* Highly scalable framework that is effective in cloud computing environments.

### Composite Centric Memory {#id-composite-centric-memory}
The final “secret” of Interface Vision and SIP is composite centric memory. Composite centric memory has the following key aspects:

* **Data Structures** - Object types are created specifically to store information: data structures. They are usually some type of data structure like a list, hashtable, balanced tree, collection, set, etc.
* **Locator**s - Object types are created specifically to locate information within data structures [12](#id-12). An example of just such a locator is the FormValue type defined in the above example. Every data structure must have at least one object type that can search that data structure.
* A program has a root object that is a hierarchy made up of one or more data structures: this is the root of the CCM.

A developer is required to place object instances they compose in composite centric memory. The format of the CCM is not important: it can be any type of data structure or even a collection of different types of data structures. However, for every program there is a single “root” by which all information is accessible. Locator objects are able to find information within the CCM.

Information in CCMs can be accessed as follows:

* **Global Level** - For information shared across all systems, within a globally accessible static type. This has an associated locator type to find information located within this static CCM.
* **Thread Level** - For each thread, a non-static CCM is available. This has an associated locator type to find information located within this thread specific CCM.
* **System Level** - For each call into a system (with), a message can be passed. This message is a CCM in itself and has an associated locator type.

CCM allows access to all external information required by any system to accomplish a task. Note, this is not similar to a global variable in that a system accesses a CCM via locators. Programmers should not access CCM directly (and really they can’t since they are not allowed to code: they can only compose using existing types).

### Miscellaneous Stuff

#### Thread Safe Code and Re-Entrance
One issue that comes up is multi-threaded programs. It is often necessary for a function or method to be used by different threads at the same time. This means any data shared between these programs, even within the method or function, needs to be “protected” in some way. We don’t want two programs updating the same data at the same time.

**Duplicate a CCM** - Optimally, we want to remove the need for a developer to make their code thread safe. The most desirable solution, then, is taking advantage of the properties of CCMs. A program is composed of different objects. To create a new instance of an entire program, you just duplicate the CCM for that program and run it in a thread. You are assured that everything within that CCM instance is only accessed by the current thread. You don’t have to even consider thread safe code.

**Mutex A System** - A property that needs to be thread safe can be wrapped or decorated with a Mutex type. An example of just such a mutex type is provided below.

    public class Mutex : IItem {
    	public string nameOfMutex { get; set; }
    	public IItem wrappedItem { get; set; }
    	public long withLong {
    		get {
    			long temp = 0;
          Ccm.shared.mutext[nameOfMutex].enter = this;
          temp = wrappedItem.withLong;
          Ccm.shared.mutext[nameOfMutex].exit = this;
          return temp;
    		}
    	}
    }

Here is an example usage that makes our above Add json example thread safe:

    {
      "$type": "Mutex",
      "nameOfMutex": "AddMutex",
      "wrappedItem": {
        "$type": "Add",
        "opLeft": {
          "$type": "FormValue",
          "nameForm": "AddForm",
          "nameValue": "opLeft",
        },
        "opRight": {
          "$type": "FormValue",
          "nameForm": "AddForm",
          "nameValue": "opRight",
        }
      }
    }

Consider for a moment that the example Mutex type is fully re-usable. It can be plugged into any property! The same approach can be used for things like exception handling.

#### Minimizing Memory Allocation During Run Time

Allocating memory can be expensive. Interface Vision is able to minimize memory allocation during run-time. A program is composed solely of objects. These objects are first loaded when the program is deserialized from json, xml, etc. They stay around for the lifetime of the program. This means an entire program can be configured that requires no memory allocation during run time.

Let’s consider a program for logging a user into a web based system. Let’s say we want to have up to 400 people a second logging in and it takes 1 second to log them in. Our program is composed to login a single user. That program is then stored in a Factory type that is able to pre-allocate as many of our log-in programs as we require: 400 in this case. For every login, we ask for a login program from the Factory, run that login based on some varying information (perhaps username and password), and return that program to the factory when we are done. If all 400 programs are in use, the Factory can create a new instance of the entire program which would require memory allocation.

This can lead to faster execution of programs as there is no need to allocate or deallocate memory during run-time.

This also makes for programs that are cloud friendly as they can be easily scaled. We don’t have to worry about our code being thread safe meaning it will run faster [13](#id-13).


## Footnotes

{#id-1} 1. Within this "paper" we look at systems as being "similar" to objects, classes, modules, subroutines and the such.

{#id-2} 2. Interface Vision was written in C#. Note that we are providing a withLong property where, later on in the page, you see it is withSystem. In reality, and for execution speed, every object can implement a with* method where * is a primitive data type like long, int, float, boolean, long, etc.

{#id-3} 3. Note that we do not provide a configuration for the form itself. This was done to keep the example simple.

{#id-4} 4. We could generalize the idea of locating information within our framework.

{#id-6} 6. Note that, to keep the example simple, we do not provide a configuration for the form itself. The Form class would be part of the vision framework. Instantiating and defining the form would be done in the exact same manner as was done with the Add program.

{#id-7} 7. In fact, Interface Vision’s framework has almost 100% decoupling. 

{#id-8} 8. The properties themselves are accessible externally but really only used directly during serialization and deserialization. 

{#id-9} 9. Programs can be stored as json, xml, binary data, and even as key/value pairs in a database.

{#id-10} 10. Remember, the program could also be created by using C# object initializers and then compiled or someone could “program” in json (although this is frowned upon).

{#id-11} 11. The framework is specifically designed to be composed visually: not coded against. However, coding is possible using C# object initializers.

{#id-12} 12. Traditionally, data structure objects support both the structure and the searching of that structure. This does seem to violate the single-responsibility principle.

{#id-13} 13. Code that is thread safe usually requires some kind of operating system call slowing down the code in general: even when it is not being used in a multi-threaded environment.

http://en.wikipedia.org/wiki/Subroutine