---
layout: post
title:  "Duck Typing"
categories: Ruby
---
# Duck Typing?

This is a difficult subject to wrap your head around without playing with it kinesthetically.

- Weak Typed
- Strong Typed
- Static Typed
- Dynamic Typed

4 different kinds. Plus duck typed so 5?
# Variable / Function Definitions

## Static Type
For older languages you would have to define a variable and then define what object
type it was. As in, you'd have to define if the variable was a string,
integer, boolean, or array. This is what makes it "statically" typed. The variable object type
is then known at compile time.

In C:
```C
// Define variables and their types
int main () {
  // variable definition
  int a;
  int c;

  // types and initialization
  a = 10; // integer type defined
  c = 'Hello'; // string type defined
}
```

## Dynamic Type
For newer languages the types of the objects are inferred at runtime. You can define
earlier on in the code that variable `a` is equal to 5 and therefore is inferred to be an integer.

In Ruby:
```ruby
a = 5
# no further action required
```

> well hold on. why does it _need_ to know what object type it is? The performance gains on a static
typed language implies dynamic typed languages are doing something in the background
deeper in the machine code that needs this differentiation. Can't you just
store integer 5 to namespace `a`? This is probably a good chance to learn an older
language like Assembly or C that would tell me more about how/why these things are present.
I think i was thinking about the messages sent to the objects later in the program.
like 3 + '5' works in some but not all languages. I don't think that's what type is
referring to.

This distinction also applies to variable changes later in the program.

In C once you set a variable to a type, it has to remain that type. If you try to
reassign a variable to a different type later it will throw an exception. Python and
Ruby don't care at all.
