# HOMEWORK 2

---

###### 10_temperature_object

Will be merged from Serhii's fork

---

###### 11_dictionary

Constructor implementations

```ruby
  def initialize
    @entries = {}
  end
  
  def initialize(dictionary = {})
    @entries = dictionary
  end
  
  def initialize
    @entries ||= {}
  end
```
@[10]("||=" in initialize is not needed)

+++

###### 11_dictionary

Also class can be without constructor, but with the following method:

```ruby
  def entries
    @entries ||= {}
  end
```

+++

###### 11_dictionary

`#add` implementations

```ruby
  def add(word)
    if word.is_a?(String)
      @entries[word]=nil
    else
      word.each do |key,value|
        @entries[key]= value
      end
    end
  end

  def add(hash)
    if hash.is_a?(Hash)
      @entries.merge!(hash)
    else
      @entries[hash] ||= nil
    end
  end

  def add(options)
    options = { options => nil } if options.class == String
    self.entries = self.entries.merge(options)
  end

  def add(entry)
    entry = { entry => nil } unless entry.is_a? Hash
    @entries.merge!(entry)
  end
```
@[1-9]
@[11-17]
@[19-22]
@[24-27]

+++

###### 11_dictionary

`#keywords` implementations

```ruby
  def keywords
    self.entries.keys.sort
  end
  
  def keywords
    entries.sort.to_h.keys
  end
  
  def keywords
    @entries.keys.sort
  end
```
@[1-3]
@[5-7]
@[9-11]

+++

###### 11_dictionary

`#include?` implementations

```ruby
  def include?(x)
    if @entries.keys.include?(x)
      true
    else
      false
    end
  end

  def include?(key)
    @entries.include?(key)
  end
  
  def include?(string)
    self.entries.has_key?(string)
  end
  
  def include?(k)
    entries.has_key?(k)
  end
```
@[1-7]
@[9-11]
@[13-15]
@[17-19]

+++

###### 11_dictionary

`#find` implementations

```ruby
  def find(word)
    word_result = {}
    @entries.each do |key, value|
      if (/#{word}/ =~ key) != nill
        word_result[key] = @entries[key]
      end
    end
    word_result
  end

  def find(word)
    self.entries.select { |k,v| k.match(/#{word}/) } || {}
  end
  
  def find(prefix)
    entries.select { |key, _| key =~ /^#{prefix}/ }
  end

```
@[1-9]
@[4]
@[11-13]
@[15-17]

+++

###### 11_dictionary

`#printable` implementations

```ruby
  def printable
    string = ''
    @entries.sort.to_h.each_pair { |k, v| string << "[#{k}] \"#{v}\"\n" }
    string.chomp
  end
  
  def printable
    strings = []
    self.entries.sort.each do |k,v|
      strings << "[#{k}] \"#{v}\""
    end
    strings.join("\n")
  end
  
  def printable
    a = @entries.sort.map do |k, v|
      "[#{k}] \"#{v}\""
    end
    a.join("\n")
  end

  def printable
    entries.sort.collect { |key, value| "[#{key}] \"#{value}\"" }.join("\n")
  end
```
@[1-5]
@[7-13]
@[15-20]
@[22-24]

+++

###### 11_dictionary

Alternative implementation

```ruby
  class Dictionary < Hash
    def add(param)
      self.merge!(param) if param.is_a?(Hash)
      self[param] = nil if param.is_a?(String)
    end
    def entries
      self
    end
    def keywords
      self.keys.reverse
    end
    def find(key)
      self.select{|k| k.include?(key)}
    end
    def printable
      self.map {|k, v| %Q([#{k}] "#{v}"\n)}.reverse.join.strip
    end
  end
```

---

###### 12_rpn_calculator

Without analysing complex algorithm just review some details:

```ruby
# Ex.1
string.split.map! do
        |token| if token.match(/[0-9]/) != nil
                  token.to_i
              else
                token.to_sym
              end
    end   

# Ex.2
if token == '+'
  plus
elsif token == '-'
  minus
elsif token == '/'
  divide
elsif token == '*'
  times
end

# Ex.3
tokens.each_index do |i|
  tokens[i] = tokens[i].to_sym if %w[+ - * /].include?(tokens[i])
  tokens[i] = Integer(tokens[i]) unless %i[+ - * /].include?(tokens[i])
end

# Ex.4
def last_el
  element = calculator.pop
  if element.nil?
    raise "calculator is empty" if element.nil?
  end 
  return element
end

# Ex.5
number_2 = @stack.pop
number_1 = @stack.pop
@stack.push(number_1 * number_2)

# Ex.6
@tokens.map! do |x|
  x = if x == '+'
        :+
      elsif x == '-'
        :-
      elsif x == '/'
        :/
      elsif x == '*'
        :*
      else
        x.to_i
      end
end
# How it should be:
t.split.map do |s|
  case s
  when '+', '-', '/', '*'
    s.to_sym
  else
    s.to_f
  end
end

# Ex.7
pol.split(' ').map{|n| n.to_i.to_s == n ? n.to_i : n.to_sym}

# Ex.8
@stack[@stack.size - 1]

# Ex.9
tokens(pol).chunk{|n| n.is_a?(Integer)}.each{|e,a| e == true ? a.each{|a| push(a) } : a.each {|o| (opps[o].call) }}
```
@[2-8]
@[11-19]
@[22-25]
@[28-34]
@[37-39]
@[42-54]
@[56-63]
@[66]
@[69]

---

###### 13_xml_document

Solution 1. (most popular)

```ruby
class XmlDocument
  attr_accessor :indent, :indent_level
  
  def initialize(indent = false)
    @indent = indent
    @indent_level = 0
  end

  def method_missing(method_name, *args, &block) 
    attributes = args[0] || {}
    tag_string = ""
    tag_string << add_indent
    tag_string << "<#{method_name}"
    attributes.each do |key, value|
      tag_string << " #{key}='#{value}'"
    end

    if block
      tag_string << ">"
      tag_string << "\n" if indent
      increment_indent
      tag_string << yield
      decrement_indent
      tag_string << add_indent
      tag_string << "</#{method_name}>"
      tag_string << "\n" if indent
    else
      tag_string << "/>"
      tag_string << "\n" if indent
    end
    tag_string
  end

  private

  def add_indent
    indent ? ("  " * indent_level) : ""
  end
  
  def increment_indent
    @indent_level += 1
  end

  def decrement_indent
    @indent_level -= 1
  end
end
```

+++

###### 13_xml_document

Solution 2. (most readable)

```ruby
class XmlDocument
  def initialize(padding = false)
    @padding = padding
    @padding_level = 0
  end

  def method_missing(name, args = {}, &block)
    @padding_level += 2
    answer = if block
               create_tag(name, args, &block)
             else
               create_empty_tag(name, args) + (@padding ? "\n" : '')
             end
    @padding_level -= 2
    answer
  end

  private

  def create_tag(name, args = {})
    prefix = create_open_tag(name, args)
    inner = yield.to_s
    suffix = create_close_tag(name)

    if @padding
      padding = ' ' * @padding_level
      prefix += "\n"
      suffix += "\n"
      inner = padding + inner + padding[0...-2]
    end

    prefix + inner + suffix
  end

  def create_empty_tag(name, args = {})
    create_open_tag(name, args).insert(-2, '/')
  end

  def create_open_tag(name, args = {})
    attributes = args.map { |key, value| " #{key}='#{value}'" }.join
    "<#{name}#{attributes}>"
  end

  def create_close_tag(name)
    "</#{name}>"
  end
end
```

---

###### 14_array_extensions

Your `#sum` implementations

But you should know that class Array already has this method :) <br>
https://goo.gl/jixQ9u

```ruby
def sum
  return 0 if self.size.zero?
  sum = 0
  self.each { |e| sum += e }
  sum
end

def sum
  if self.empty?
    0
  else
    inject(0, :+)
  end
end

def sum
  inject(0, &:+)
end

def sum
  reduce(0, &:+)
end
```
@[1-6](Oh..)
@[8-14](`self.` and condition are unecessary)
@[16-18](use `inject(0, :+)` instead of `inject(0, &:+)` it's a little bit faster)
@[20-22](Perfect)

+++

###### 14_array_extensions

`#square` and `#square!` implementations

```ruby
def square
  self.length == 0 ? self : self.map{ |num| num**2 }
end

def square!
  self.length == 0 ? self : self.map!{ |num| num**2 }
end

def square
  map { |arg| arg**2 }
end

def square!
  map! { |arg| arg**2 }
end

class Array
  SQUARE = proc { |value| value * value }

  def square
    map(&SQUARE)
  end

  def square!
    map!(&SQUARE)
  end
end
```
@[1-7](BAD)
@[9-15](Good)
@[17-27](Interesting (Good))
---

###### 15_in_words


