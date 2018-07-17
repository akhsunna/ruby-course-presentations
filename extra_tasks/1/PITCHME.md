
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
if 'bob'
  puts 'bob is truthy'
else
  puts 'bob is falsey'
end
```

+++

Answer:

```ruby

bob is truthy

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

What does the following expression evaluate to?

```ruby
3 / 2
```

+++

```ruby
>> 3 / 2
1
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

What does the following code print.

```ruby
cool = "Beans"
def dinner_plans()
  puts cool
end

dinner_plans()
```

+++

This raises an error because the cool variable is defined outside the dinner_plans() method.

---

```ruby
class Dog
  def speak
    'woof woof'
  end
end
```

What does the following code return? 

```ruby
Dog.speak
```

+++

```ruby

NoMethodError: undefined method `speak' for Dog:Class

```

---

What does the following code print? Explain what happens when the `#meaning_of_life` method is run multiple times for a given object.

```ruby
class Something
  def meaning_of_life
    @result ||= result
    "The meaning of life is the number #{@result}"
  end

  def result
    Random.rand(1000)
  end
end

something = Something.new
something.meaning_of_life
```

+++

```ruby
The meaning of life is the number 4
```

When the `#meaning_of_life` method is run, the return value of the `#result` method is assigned to the `@result` instance variable, but only when `@result` is `nil`. When the `#meaning_of_life` method is initially run, `@result` is `nil`, so the code in the `#result` method is executed. When the `#meaning_of_life` is run again, `@result` is not `nil`, so the `#result` method is not executed.
<br>
The `||=` operator (pronounced "or-equal operator") is useful for caching values in instance variables and preventing code from needlessly running when values have already been calculated.

---

What does the following code print? Explain.

```ruby
BEST_NOLAN_MOVIE = 'Inception'
BEST_NOLAN_MOVIE = 'Interstellar'
p BEST_NOLAN_MOVIE
```

+++

```ruby
"Interstellar"
```

BEST_MOVIE is a constant and can be reassigned to another value just like other variables. 
Constants are not constant if they are being reassigned, so reassigning constants is not advisable. 
Ruby will give you a warning when you reassign a constant, but does not raise an exception and allows the reassignment to take place.





