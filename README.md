# ruby-cheatsheet

## Iterators

```Ruby
    a_list.each do |item|
      #Inside of block
      #we can work with item
    end
```

## object_id

The `object_id` method returns the identity of an object. If two objects have the same `object_id`, they are the same (point to the same Object in memory).

## Symbols

    > :george.object_id == :george.object_id
    => true
    > "george".object_id == "george.object_id
    => false

Symbols are identities (IDs). It is important *who* it is not *what* it is.
A symbol with the same characters references the same Object in memory. For any given two Symbols that represent the same characters, the `object_id`s match.

If in doubt wheter to use a Symbol or a String, consider what's more important: the identity of an object (i.e. Hash key), or the contents (in the example above, "george").

## Naming conventions

* Constant: Starts with capital letter
* $: global variable
* @: instance variable
* @@: class variable

Method names are allowed to start with capital letters:

```Ruby
    Constant = 10
    def Constant
        11
    end
    puts Constant   #10
    puts Constant() #11
```

## Keyword arguments (Ruby 2.0)

```Ruby
    def deliver(from: 'A', to: nil, via: 'mail')
        "Sending from #{from} to #{to} via #{via}."
    end
    
    > deliver(to: 'B')
    => "Sending from A to B via mail."
    > deliver(via: 'Pony Express', from: 'B', to: 'A')
    => "Sending from B to A via Pony Express."
```

## The universal truth

Everything except `nil` and `false` is considered `true`.

```Ruby
    if 0
        puts "0 is true"
    else
        puts "0 is false"
    end
    
    #0 is true
```

## Message passing, not functions calls

A method call is really a *message* to another object:

```Ruby
    1 + 2
    # is the same like
    1.+(2)
    # Which is the same as this:
    1.send "+", 2
```

## Get all instance methods

### Returns all methods
```Ruby
    > Greeter.instance_methods
    => ["method", "send", "object_id", ...]
```
### Returns only instance methods
```Ruby
    > Greeter.instance_methods(false)
    => ["say_hello", "say_goodbye"]
```
### Check if an object responds to a method
```Ruby
    > g.respond_to?("foo")
    => false
```

### Duck typing
```Ruby
    def say_goodbye
        if @names.nil?
            puts "..."
        elsif @names.respond_to?("join")
            #Check if names respond to types. Here the actual type of @names does not matter
            puts "Cia, #{@names.join(", ")}
        else
            puts "Ciao #{@names}"
        end
    end
```
### Entry point for scripts

```Ruby
    if __FILE__ == $0
    end
```
