# Understanding Function Calls

## Learning Goals

By the end of this lesson you should be able to

- Explain how function calls work
- Explain the terms
  - Pass by reference
  - Pass by value
  - Call stack
  - Recursive Function
- Correctly identify the time and space complexity of small recursive functions

## Video Lesson

- [Video Lesson]()
- [Slides Used]()
- [Assignment](https://github.com/Ada-C12/recursion-tracing)

## Introduction

We have been using methods/functions for several months now.  However we have not investigated how functions actually work, under the hood.  How does information get passed into a method and how is information returned.

So what does a method need:

- Method name - A symbol which represents the block of code
- Method arguments - the data items sent to the method for processing.
- Local variables - A method may have local variables to store information while it runs
- Return value - A method should have some way to return a value as a result

We will first investigate how method arguments get passed into a method, then look at how the system stack is used to store method arguments and local variables and how the return value is sent back when the method finishes.

## Pass By Value

Take a look at the code below, plug it into irb or run it via the terminal or repl.  Try to understand the code. What do you expect to see in the console output? When do you expect the object_ids to change verses remain the same?  An object id references the object being stored in memory.  If two variables share the same object id, they must both reference the same object in memory.

```ruby
def peek_add_ten(number)
  puts "In peek_add_ten: #{number.object_id}"
  number = number + 10
  puts "Number is now: #{number}"
  puts "On adding in peek_add_ten: #{number.object_id}"
  return
end

def add_ten(number)
  puts "In add_ten: #{number.object_id}"
  number = number + 10
  puts "Number is now: #{number}"
  puts "On adding in add_ten: #{number.object_id}"
  return number
end

# Main code logic
result = 10
puts "Original value: #{result}"
puts "Object id is: #{result.object_id}"
puts "\n"

peek_add_ten(result)
puts "After peek_add_ten value is: #{result}"
puts "Object id is: #{result.object_id}"
puts "\n"

result = add_ten(result)
puts "After add_ten value is: #{result}"
puts "Object id is: #{result.object_id}"
```

Now, copy over this code and try it out in the console. Did your expectations match what the console output showed?

Ruby passes _references_ to the parameters in a method.  This means that if `result` in the main code logic has an object id of 41 that object id will be passed into `number` in the `peek_add_ten` parameter.  So both variables will refer to the sample place in memory.  However in the 2nd line of `peek_add_ten` method you have `number = number + 10`, and this _changes_ the object being referred to by `number` to a new place in memory.  So after that line `result` and `number` refer to **different** objects.  When we use the equal sign `=` or any of the shortcut operators like `+=` or `-=` you will **reassign** the variable to another memory address.

On the other hand consider this set of methods below.  Where will the object ids change?  Will `result` in the main code logic section be changed by what happens in `add_three`?

```ruby
def add_three(list)
  puts "in add_three list object_id: #{list.object_id}"
  list << 3

  puts "list now: #{list}"
  puts "after pushing in 3 the object id is: #{list.object_id}"
  puts "\n"
  return list
end

# Main code logic
result = [1, 2]
puts "Original value: #{result}"
puts "Object id is: #{result.object_id}"
puts "\n"

answer = add_three(result)
puts "After add_three value is: #{result}"
puts "Object id is: #{result.object_id}"
```

So the object ids are all the same across the program.  Just like with the integers passed in the previous example, `result` and `list` both refer to the same variable in memory.  However arrays and other objects like hashes, or custom objects have methods which act upon the data stored at that location in memory.

For arrays, the `<<`, as well as with the `[]` operator follow the reference to the place in memory where the array is stored and make changes upon the data stored inside the array.  The reference only changes for a variable when you use the `=` operator or a similar shortucut operator to assign that variable to another place in memory.

So in Ruby arguments are passed into a method's parameters by taking the memory address the arguments reference and passing them into the parameters.  This means that for primitive data types like integers, or floats, there is no way for a parameter to change the value of the argument passed in because these types of objects have no mutator methods which change the data stored inside those objects.

Learn more by reading [Object passing in Ruby](https://launchschool.com/blog/object-passing-in-ruby) by LaunchSchool or our [references & values](https://github.com/Ada-Developers-Academy/textbook-curriculum/blob/master/02-intermediate-ruby/references-and-values.md) lecture.

## The Call Stack

A running program has 4 sections of memory.

1. Memory used to store the program instructions, where the executable code is stored.
1. Memory used to store global variables.
1. The heap, which is a section of memory used to allocate space allocated with the `.new` method.
1. The system stack (more on this) where method calls and local variables are stored.
1. Details of the environment

![The Call Stack](images/call-stack.png)

The system stack is used to keep track of method calls.  A stack works kind of like a stack of plates, you add things to the top and you also remove things from the top.  So If this is my stack below and I add "Mary" to the stack, "Mary" will go to the top.

![Example Stack](images/example-stack.png)

Resulting in the stack below.

![Example Stack 2](images/example-stack2.png)

Similarly if I remove an element from the stack, called a pop operation, I will then get "Mary"  If I then attempt to pop again, I will get "Paul".  Stacks operate in a Last-In-First-Out (LIFO) manner.  They remove things in the reverse order they were added.  

The system uses a stack to manage method calls.  When a method, lets call it `method_a`, is called something called a _stack frame_ is added to the system stack.  That stack frame contains all the local variables used by the method, the parameters used by the method and a memory address to return to when the function completes.  If `method_a` calls another method, `method_b` then `method_b`'s stack frame is added to the stack (with `method_a` still on the stack) and begins to execute.  Similarly if it calls another method, `method_c` then that method's stack frame gets added.  When `method_c` completes it's stack frame is popped off the stack and the program returns to where `method_b` left off, as indicated by the return address.  Then when `method_b` completes it's stack frame pops off and and `method_a` resumes.  If too many method calls are added to the system stack and memory is exhausted this results in a Stack Overflow Error, an inspiration for one of our favorite [websites](https://stackoverflow.com/).

![Example call stack run](images/call-stack-run.png)

## Recursion



## Terms & Definitions

| Term | Definition |
|--- |--- |
| Call Stack | Also known as the system stack, or activation chain, a stack data structure which stores currently running method calls |
| Stack | A data structure which stores information in a Last-In-First-Out order.
| Stack Overflow Error | An error which occurs when the call stack runs out of memory |

## Resources

- Read more on [References and mutability in Ruby](https://launchschool.com/blog/references-and-mutability-in-ruby) by LaunchSchool
- Read more on [Mutating and non-mutating methods in Ruby](https://launchschool.com/blog/mutating-and-non-mutating-methods) by LaunchSchool
- Revise lesson on [Reference and values](https://github.com/Ada-Developers-Academy/textbook-curriculum/blob/master/02-intermediate-ruby/references-and-values.md)