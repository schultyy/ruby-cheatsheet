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

## Parse Yaml files

```Ruby
    require 'yaml'
    
    config = YAML.load_file('config.yml')
    puts config.inspect
```

## Shell Commands

### Exec
Replaces the current process by running the given command. 

```Bash
    $ irb
    >> exec 'echo "hello $HOSTNAME"'
    hello nate.local
    $
```

### System
The `system` command operates similarly but runy in a subshell instead of replacing the current process.
`system` returns a little more information than `exec` in that it returns `true` if the command ran successfully or `false` otherwise.

```Bash

    $ irb
    >> system 'echo "Hello $HOSTNAME"'
    hello nate.local
    => true
    >> system 'false'
    => false
    >> puts $?
    256
    => nil
    >>
```

`system` sets the global variable `$0` to the exit status of the process. Notice that we have the exit status of 
the `false` command (which always exits with a non-zero code). 

> Note: Unix commands typically exit with a status of `0` on success and non-zero otherwise.

### Backticks(`)

Backticks (also called "backquotes") runs the command in a subshell and returns the standard output from that command.

```Bash
    $ irb
    >> today = `date`
    => "Monday..."
    >> $?
    => #<Process::Status: pid=25827, exited(0)>
    >> $?.to_i
    => 0
```

> Note: `$?` is not simply an integer of the return status but actually a `Process::Status` object.

One consequence of using backticks is that only *standard output* (`stdout`)  of this command is available but not `stderr`.

## Classes

### attr_accessor

Let's say there is a class `Person`:

```Ruby

    class Person
    end
    
    person = Person.new
    person.name # => no method error

```

Obviously we never defined method `name`. Let's do that.

```Ruby

    class Person
        def name
            @name #Simply returning an instance variable @name
        end
    end

person = Person.new
person.name # => nil
person.name = "Boo" # => no method error

```

We can read the name but that doesn't mean we can assign a name. Former called **reader** and latter called **writer**. Let's create a writer:

```Ruby

	class Person
		def name
			@name #Returns instance variable @name
		end
		
		def name=(str)
			@name = str
		end
		
		person = Person.new
		person.name = "Boo"
		person.name # => "Boo"
	end
```

Now we can read and write instance variable `@name` using reader and writer methods. Except, this is done so frequently, why waste time writin these methods every time? We can do it easier.

```Ruby
	
	class Person
		attr_reader :name
		attr_writer :name
	end
	
```

Even this can get repetitive. When you want both reader and writer just use accessor!

```Ruby

	class Person
		attr_accessor :name
	end
	
	person = Person.new
	person.name = "Boo"
	person.name # => "Boo"

```

## irb
### Reload file in irb

```irb
>> load 'myfile.rb'
```
