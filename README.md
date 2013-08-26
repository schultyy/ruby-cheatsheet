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
