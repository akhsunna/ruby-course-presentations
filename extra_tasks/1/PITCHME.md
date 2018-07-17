
What will val1 and val2 equal after the code below is executed?

```ruby
val1 = true and false
val2 = true && false
```

+++

Here’s the same code, but employing parentheses to clarify the default order of operations:

```ruby
(val1 = true) and false    # results in val1 being equal to true
val2 = (true && false)     # results in val2 being equal to false
```

---

Which of the expressions listed below will result in "false"?

```ruby
true    ? "true" : "false"
false   ? "true" : "false"
nil     ? "true" : "false"
1       ? "true" : "false"
0       ? "true" : "false"
"false" ? "true" : "false"
""      ? "true" : "false"
[]      ? "true" : "false"
```

---

What is the value of the variable `upcased` in the below piece of code?

```ruby
  upcased = %w[one two three].map {|n| puts n.upcase }
```

+++

```ruby
>> upcased = %w[one two three].map {|n| puts n.upcase }
ONE
TWO
THREE
=> [nil, nil, nil]
```

---

What is the difference between `Array#map` and `Array#each`?

+++

```ruby
# Given
>> arr = [1,2,3,4]

# This will print 2,4,6,8 but will return [1,2,3,4]
>> arr.each {|x| puts x*2; x*2 }
2
4
6
8
=> [1, 2, 3, 4]

# This will also print 2,4,6,8 but returns [2,4,6,8]
>> arr.map {|x| puts x*2; x*2 }
2
4
6
8
=> [2, 4, 6, 8]
```
@[1-2]
@[4-10]
@[12-18]

---

Can you call a private method outside a Ruby class using its object?

+++

Yes, with the help of the `#send` method.

Given the class `Test`:

```ruby
  class Test
    private
    def method
      p 'I am a private method'
    end
  end
```
We can execute the private method using `#send`:

```ruby
>> Test.new.send(:method)
"I am a private method"
```

---

What is the difference between calling super and calling super()?

+++

A call to `super` invokes the parent method with the same arguments that were passed to the child method. An error will therefore occur if the arguments passed to the child method don’t match what the parent is expecting.
<br>
A call to `super()` invokes the parent method without any arguments, as presumably expected. As always, being explicit in your code is a good thing.

---


