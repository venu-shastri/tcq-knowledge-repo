# How to  ensure High Quality of a Software System
>A broad base of **`inexpensive, well-crafted, very fast, regularly maintained, and fully automated unit tests, supported by a selection of useful component tests`**.

## Unit Test
**`refactoring” without tests isn’t refactoring, it is just moving shit around`**.
—Corey Haines (@coreyhaines), December 20, 2013, on Twitter
- A unit test is a piece of code that executes a small part of your production code base in a particular context.
- Unit Test Advantages
	- fixing bugs after software is shipped has been proven to be much more expensive than having unit tests in place
	- Unit tests give an immediate feedback about your entire code base
	- A high coverage with unit tests can prevent time-consuming and frustrating debugging sessions
	- Unit tests are a kind of executable documentation because they show exactly how the code is designed to be used
	- Unit tests can easily detect regressions, that is, they can immediately show things that used to work, but have unexpectedly stopped working after a change in the code was made.

#### What about QA?
A developer  attitude: **`Why should I test my software? We have testers and a QA department, it’s their job`**

Is software quality a sole concern of the quality assurance department?

**`I’ve said this before, and I’ll say it again. Despite the fact that your company may have a separate QA group to test the software, it should be the goal of the development group that QA find nothing wrong`**.
—Robert C. Martin, The Clean Coder [Martin11]
- QA is  second safety net

### How to write production code  for the unit test code?
**`Be Principled`**

#### What Is a Principle?
>A principle is a kind of rule, belief, or idea that guides you. Principles are often directly coupled to values or a value system

- **KISS**
	- Focusing on simplicity is probably one of the most difficult things to do for a programmer. And it is a life long learning experience
- **YAGNI**
	- Always implement things when you actually need them, never when you just foresee that you need them.
- **DRY**
	- Copy and paste is a design error
	- Referred to as “Once And Only Once” (OAOO).
	- applying the DRY principle means that we have to ensure that “every piece of code must have a single, unambiguous, authoritative representation within a system
- **Information Hiding**
	- The principle states that one piece of code that calls another piece of code should not know internals about that other piece of code.
	- information hiding is the basic principle for decomposing systems into modules
	- advantages of information hiding:
		- Limitation of the consequences of changes in modules
		- Minimal influence on other modules if a bug fix is necessary
		- Significantly increasing the reusability of modules
		-  Better test-ability of modules
	- Information hiding is often confused with **encapsulation**, but it’s not the same
	- ex:-
	```C++ 
	class  DeviceConnection { 
	public:
	enum  class  State {
	closed = 1,
	opening,
	open,
	closing
	};
	private:State state;
	// ...more attributes here...
	public:
	State  getState() const;
	// ...more member functions here...
	};
	#include  "DeviceConnection.h"
	int  main() {
	DeviceConnection connection;
	connection::State connectionState = connection.getState();
	if (connectionState == DeviceConnection::State::closed) {
	// do something...
	}
	return  0;
	} ```

>What will happen if the internal implementation of DeviceConnection  must be changed and the enumeration
class State is removed from the class? It is easy to see that it will have a significant impact on the client’s code. It will result in changes everywhere where member function DeviceConnection ::getState() is used

| Information Hiding | Encapsulation |
|--|--|
| Information hiding is a design principle for aiding developers in finding good modules. The principle works multiple levels of abstraction and unfolds its positive effect, especially in large systems | Encapsulation is OO Concept to define role and responsibilities ,often a programming-language dependent, technique for restricting access to the innards of a module ,For instance, in C++ you can precede a list of class members with the private keyword to ensure that they cannot be accessed from outside the class. But just because we use such kind of guards for access control, we are still far away from getting information hiding automatically |


- **Strong Cohesion**
	- cohesion is strong when the module does a well-defined job
	- Shot Gun Anti-Pattern.
		- form of weak cohesion
- Loose Coupling
- **Optimizations**
	- Premature optimization is the root of all evil (or at least most of it in programming.
—Donald E. Knuth, American computer scientist [Knuth74]
	- As long as there are no explicit performance requirements to satisfy, keep your hands off optimizations.
		- The subtly bugs are slipped into the code during  optimization measures
		- compilers are nowadays very good at optimizing code.
		- Whenever you feel just a desire to optimize something, think about YAGNI
- **Principle of Least Astonishment (PLA)**
	- Also known as Principle of Least Surprise (POLS)
	- Well known in user interface design and ergonomics
	- The principle states that the user should not be surprised
by unexpected responses of the user interface.
	-  The user should not be puzzled by appearing or disappearing
controls, confusing error messages, unusual reactions on established keystroke sequences  or other unexpected behavior
- **The Boy Scout Rule**
	-  Always leave the campground cleaner than you found it.
	- One of their principles states that they should clean up a mess or pollution in the environment immediately, once they’ve found such bad things
	- Developers should apply this principle to our daily work. Whenever we find something in a piece of code that needs to be improved, or which is a bad code smell, we should fix it immediately. And it does not matter who the original author of this piece of code was.
	- Advantages
		- continuously prevent the dilapidation of our code
		- Renaming a poorly named class, variable, function, or method 
	-  Behaviors
		- Decomposing the innards of a large function into smaller pieces 
		- Deleting a comment by making the commented piece of code self-explanatory
		- Cleaning up a complex and puzzling if-else-compound.
		- Removing a small bit of duplicated code
		- Collective Code Ownership.
			- Every team member, at any time, is allowed to make a change or extension on any piece of 
### Unit Test

---

- Test specific aspects of a functionality
- Execute rapidly
- Independent of each other
- Independent of the surrounding environment
- Independent of execution order
- Automated

#### Value-based vs. state-based vs. interaction testing

---

- Value-based testing checks the value returned from a function
- State-based testing is about checking for noticeable behavior changes in the system under test, after changing its state.
- Interaction testing is testing how an object sends messages (calls methods) to other objects



#### External Dependency and Unit Test

---

> An external dependency is an object in your system that your code under test interacts with and over which you have no control. (Common examples are file systems, threads, memory, time, and so on.)

#### Inherent challenges in testing dependent objects

---

- Objects dependent on ephemeral objects produce unpredictable behavior

- User Interfaces, Databases and the like are inherently ephemeral

- How do you test that your object interacts with other objects correctly?

  

##### Stub

----

**A** **stub is a controllable replacement for an existing dependency** **(or** **collaborator) in the system. By using a stub, you can test your code without** **dealing with the dependency directly**

##### Mock

---

Complex collaborator interactions cannot be captured with a simple stub 

Defining interaction testing using  Mock Object

> A Mock Object is a fake dependency or stub  that verifies that the code being tested interacts with its collaborator properly.
>
> A mock object is a fake object in the system that decides whether the unit test has passed or failed. It does so by verifying whether the object under test called the fake object as expected. **There’s usually no more than one mock per test**.
>
> - The test tells the mock
>   - The expected calls
>   -  In the expected order
>   - What to return



###  TDD

---

-

- Is a programming practice

- Unit tests are written before the domain code
- Write a test that fails
- Write code to make the test pass
- Refactor
- Unit Tests and Refactoring are the tools of TDD



### The Benefits of TDD

---

- TDD ensures quality code from the start. Developers are encouraged to write only the code needed to make the test pass and thus fulfill the requirement. 
- Whether by design or by coincidence, most TDD practitioners write code that follows the SOLID principals
- TDD ensures a high degree of fidelity between the code and the business requirements.
- If your requirements are written as tests, and your tests all pass, you can say with a high degree of confidence that your code meets the needs of the business
- TDD encourages the creation of simpler, more focused libraries and APIs
- TDD encourages communication with the business. To create these tests, you are encouraged to interact with the business users. This way, you can make sure that the input and output combinations make sense, and you can help the users understand what they are building.
- TDD helps keep unused code out of the system. Most developers have written applications in which they designed interfaces and wrote methods based on what might happen. This leads to systems with large parts of code or functionality that are never used. This code is expensive. TDD helps keep this parasite code out of your system.
- TDD provides built - in regression testing. As changes are made to the system and your code, you always have the suite of tests you created to ensure that tomorrow ’ s changes do not damage today ’ s functionality.
- TDD puts a stop to recurring bugs, With TDD, as soon as a defect is reported, a new test is written to expose it. When this test passes, and continues to pass, you know the defect is gone for good
- When developing applications with testability in mind, the result is an architecture that is open, extensible and flexible. Dependency Injection  is a key component of both TDD and a loosely coupled architecture

### The Rules of TDD

---

- You are not allowed to write any production code until you have first written a failing unit test.
- You are not allowed to write more of a unit test than is sufficient to fail – and not compiling is failing
- You are not allowed to write more production code than is sufficient to pass the currently failing unit test

### TDD Steps

---

- **Write** a test.

- **Run** the test. It **fails to compile** because the code you're trying to test doesn't even exist yet! (This is the same thing as failing.)

- **Write a bare-bones stub** to make the **test compile**.

- Run the test. It should **fail**. (If it doesn't, then the test wasn't very good.)

- **Implement the code** to make the test pass.

- Run the test. It should **pass.** (If it doesn't, back up one step and try again.)

- Start over with a new test!

### When We Should Not Use TDD

---

- When Requirements are Volatile and Fuzzy
- When some parts of project are so simple, small, or less complex where complexity and risks are low
- TDD won’t help in getting a good architecture
  - If you are faced with fundamental decisions about frameworks, libraries, technologies, or architecture patterns, TDD will not help you
- The project is short, simple and straight-forward

### Why Developers Don’t Use TDD

---

- "Management Doesn’t Allow Us”
-  “Not Enough Time to Write Tests”
- “My Team Disagrees on Whether We Should Use TDD"
- “Return on Investment in TDD Isn’t Proven”
- “We Tried but It Didn’t Work”
- “Slow Build Process”
- “I Don’t Know All the Requirements Upfront to Write All Tests”



### How to Succeed With TDD

---

- Make each test very small and focused.
- Avoid making tests depend on each other, either explicitly or implicitly
- Make each test express its intent very clearly.
- Pay attention to failure messages. Make each failure message as helpful for diagnosis as you can.
- Involve as little of the system as you possibly can in each test

### TDD - Embedded System Constraints

---

- Development speed
  - when the target for test execution is not the same as the host for developing software, a delay is introduced into development
- Memory footprint
  - Tests and the testing framework are added to the program code residing in target memory
- Cross-compilation issues
  - Difference in processor architecture or build tool chain
- Hardware dependencies
  - External dependencies, like hardware interaction, complicate the automation of tests. Concurrent Hardware




