---
layout: post
title:  "Monkey patching & refinements"
date:   2020-03-30
excerpt: "One way of doing it (never used in production though)"
tags: [clean code, software, code, monkey patch, refinements]
comments: true
---
A couple of months ago, I had to use the existing class with couple of tweaks and even though I ended up using different solution, I found this one still interesting.
One of my first thoughts was start with monkey patching. Monkey patch means redefining or adding functionality to the existing classes.
But that doesn't come free, the price you pay is that you can risk some unintended bugs or breakages because all users of the monkey-patched class see the same changes.

Thankfully, since Ruby 2.0 this can be done in much safer way using refinements.
Refinements are activated until the end of the current class or module definition, or until the end of the current file if used at the top-level.

Lets say we have a class that changes input string to uppercase or lowercase:

```
class CapsSwitch
  attr_reader :name

  def initialize(name:)
    @name = name
  end

  def up
    name.upcase
  end

  def down
    name.downcase
  end
end
```
We are being a little bit mean and we want to redefine method `down` which instead of returning lowercase it returns the lenght of the string, but also we want to redefine `initialize` method to accept the new additional parameter (surname).
```
module RefinementModule
  refine CapsSwitch do

    def initialize(surname: nil, **kwargs)
      @surname = surname
      super(**kwargs)
    end

    def down
      @surname.size
    end
  end
end
```
If we make a new class which will use our `RefinementModule`, we won't get the size because refinements are only active in the current scope and `CapsSwitch` can't be initialized with the new parameter.
```
class SizeClass
  using RefinementModule

  def initialize(name:, surname:)
    @name = name
    @surname = surname
  end

  def size
    CapsSwitch.new(surname: @surname, name: @name).down
  end
end
```
Instead, we can create an additional class inherited from `CapsSwitch` class. This will allow us to have additional parameter initialized.

```
class CapsSwitchWithSize < CapsSwitch
  using RefinementModule
end
```
```
class SizeClass
  def initialize(name:, surname:)
    @name = name
    @surname = surname
  end

  def size
    CapsSwitchWithSize.new(name: @name, surname: @surname).down
  end	 
end
```
In general, I think avoiding monkey patching is the best technique, but sometimes you just need quick solution and we are all only humans after all. :)
