---
layout: post
title:  " Classes in Ruby: A Hero's Story"
date:   2017-06-25 23:40:26 +0000
---

This week, I had to put together a demo lesson. I intended to put together a lesson plan, but it came out more like a narrative. So, if you are looking for a little story-based lesson on object-oriented programming with classes in Ruby, this one is for you!

**Lesson goals**:

- introduce class construction
- introduce instance variables/methods
- use attribute readers/writers
- instantiate a class object

**This lesson assumes some basic Ruby knowledge, such as**:

- data types (String, Symbol, Array, Fixnum, Hash)
- variables
- conditionals (if/else)
- operators (+, -, +=, -=, *)
- methods/method creation
- objects

---

### Class Construction

---

A small town is being ravaged by an evil dragon. A brave hero is needed to save the day, but there is a problem...he/she doesn't exist yet! It is our job to create the perfect hero and restore peace to the land.

To build our hero, we are going to create a Ruby class. The [Ruby docs](https://ruby-doc.org/core-2.2.0/Class.html) describe classes like so:

> Classes in Ruby are first-class objects...When a new class is created, an object of type Class is initialized and assigned to a global constant.

Everything in Ruby is an object ([almost](http://rubylearning.com/blog/2010/09/27/almost-everything-is-an-object-and-everything-is-almost-an-object/)). In order to create a new "hero" object of type Class, we use this syntax:

```ruby
class Hero
  # variables and methods go here
end
```

Creating a new hero object is easy, we just call the default `.new` method outside of the class definition, like so:

```ruby
# class definition
class Hero
  # Hero behavior goes here
end

# create a new hero!
bob = Hero.new
```
What we are building here is the blueprint of a `Hero`. Inside of this class definition, we want to create all of the attributes and abilities (methods) that we would expect a hero to have. When we call `Hero.new`, we are passing all of those attributes and abilities over to our new `Hero` object!

```ruby
#What is bob's class?
bob.class
#=> Hero 
```

---

### Instance Variables

---

Now that we have our `Hero` blueprint, we need to fill it with some attributes! Let's pick a few things any good dragon-slaying hero needs: `name`, `strength`, and `health`.

In order to assign these attributes to **instance variables** that each new hero object can access, we are going to utilize the magic `initialize` method that [gets called every time a new object is created](http://ruby-doc.org/docs/ruby-doc-bundle/UsersGuide/rg/objinitialization.html).

```ruby
class Hero
  def initialize(name)
	@name = name 
	@health = 100
	@strength = 5
  end
end
```
Wait a sec, what is this "@" business? The [Ruby docs](http://ruby-doc.org/docs/ruby-doc-bundle/UsersGuide/rg/instancevars.html) have this to say:

> An instance variable has a name beginning with @, and its scope is confined to whatever object self refers to.

In our case, `self` refers to our new instance of `Hero`. Instance variables are accessible across all methods within the class. Now, we must pass a name into our  new hero creation.

```ruby
bob = Hero.new("Bob")
#=> #<Hero:0x007ff5480161d0 @name="Bob", @health=100, @strength=5>
```

Cool! We can see the reference to our new hero, Bob, with all of his attributes. So now, we should be able to access Bob's health like this:

```ruby
bob.health
NoMethodError: undefined method `health'
```
---

### Attribute Readers

---

Wait, what happened? Everything was going so smoothly! Well, let's think about this. Our instance variables are accessible to our `bob` object, but calling `bob.health` is outside of that scope. In order to access instance variables, we need to wrap our variables in **attribute reader** methods.

```ruby
class Hero
  def initialize(name)
    @name = name 
    @health = 100
    @strength = 5
  end
	
  def name
    @name
  end
	
  def health
    @health
  end
	
  def strength
    @strength
  end
end
```

Ok, can we read Bob's health now?

```ruby
bob.health
#=> 100 
```
Nice! But that was a lot of work just to read the data. Fortunately, Ruby provides us with a "magical" method called `attr_reader` (from the [Module](https://ruby-doc.org/core-2.1.1/Module.html#method-i-attr_accessor) Object) that takes care of this for us! All we have to do is pass the names of our instance variables as symbols (and without the @).

```ruby
class Hero
  attr_reader :name, :health, :strength

  def initialize(name)
    @name = name 
    @health = 100
    @strength = 5
  end
end
```

So much cleaner! Now let's think of another scenario involving these instance variables. Right now, our default `@strength` value gets assigned to 5 on creation. What if we want to create a hero with a little more muscle?

---

### Attribute Writers

---

Sure, we could add strength as an argument on `initialize`, but for the sake of the story, let's say that's out of the question. Our alternative is to create an **attribute writer** method. It will look like this:

```ruby
def strength=(strength)
  @strength = strength
end
```

This is the basic pattern for all attribute writer methods. Look familiar? it should! Attribute writers tend to look a lot like initialization methods, just for one attribute at a time.

There's one thing that is important to note here: in Ruby, an attribute writer is expected to have this syntax `def method_name=(arg)`. The "=" is important, because it gives us a little [syntactic sugar](https://en.wikipedia.org/wiki/Syntactic_sugar). When you call an attribute writer, it can look like this:

```ruby
bob.strength = 8
```
and Ruby will interpret it as

```ruby
bob.strength=(8)
```
Both lines are valid, but the first is so much sweeter. Speaking of sweet, didn't we have a magic method that took care of all this sort of stuff with attribute readers? Wouldn't it make sense to have the same thing for writers?

In short, the answers are yes and yes! There is a method called `attr_writer` for attribute writers too!

```ruby
class Hero
  attr_reader :name, :health, :strength
  attr_writer :strength
  
  def initialize(name)
    @name = name 
    @health = 100
    @strength = 5
  end
end
```

---

### Attribute Accessors

---

We can consolidate even further for any variables that we want both an attribute reader and writer for. The method to do both is `attr_accessor`. We can use that for our `@strength` instance variable.

```ruby
class Hero
  attr_reader :name, :health
  attr_accessor :strength
  
  def initialize(name)
    @name = name 
    @health = 100
    @strength = 5
  end
end
```

Just a note, we still need to create instance variable definitions within the `initialize` function if we want that for our class. Ruby isn't going to do *everything* for us.

---

### Instance Methods

---

Now that we have all of the attributes we need in a good hero, it's time to fight the dragon! Let's build a method to do it:

```ruby
def fight_dragon
  # set dragon health
  dragon_health = 100
  
  # both hero and dragon take damage
  3.times do 
    dragon_health -= @strength * rand(3)
    @health -= rand(10)
  end
  
  # whoever has the most health wins!
  # hero gets the tiebreaker
  if @health >= dragon_health
    puts "The dragon is slain!"
  else
    puts "You can't kill #{@name}! I need a break, though."
  end
end
```
Notice that the instance variables are automatically accessible to the `fight_dragon` method. Once we add it to the class, our brave hero can meet his destiny!

```ruby
class Hero
  attr_reader :name, :health
  attr_accessor :strength
  
  def initialize(name)
    @name = name 
    @health = 100
    @strength = 5
  end
  
  def fight_dragon
    # set dragon health
    dragon_health = 100
  
    # both hero and dragon take damage
    3.times do 
      dragon_health -= @strength * rand(3)
      @health -= rand(10)
    end
  
    # whoever has the most health wins!
    # hero gets the tiebreaker
    if @health >= dragon_health
      puts "The dragon is slain!"
    else
      puts "You can't kill #{@name}! I need a break, though."
    end
  end
  
end

bob = Hero.new("Bob")
#=> #<Hero:0x007ff5471e1df8 @name="Bob", @health=100, @strength=5> 
bob.strength = 8
#=> 8 
bob.fight_dragon
# The dragon is slain!
bob.health
#=> 88 
```
We did it! Our hero saved the town! He may even have enough health to hop on a horse and try to save a nearby village.

```ruby
bob.fight_dragon
# You can't kill Bob! I need a break, though.
```

Or, maybe not.
