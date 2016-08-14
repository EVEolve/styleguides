# Ruby Style Guide

#### End Of File

Files should end with an empty line. This prevents the diff from showing the
last line of code when additional code is appended to the file. It's also nice
for text editors that are smart enough to copy the line feed when an entire
line is copied.

An exception to this rule is if adding the empty line adversely affects the
output from a code-generated template.


#### Line Length

Lines should have a soft-limit of 80 characters. This means a reasonable attempt
should be made to prevent lines from exceeding 80 characters. However, it may
be necessary to exceed the limit in some cases where breaking up the code over
multiple lines makes it more difficult to read.


#### Indentation

Lines should be indented with 2 spaces, which is a common Ruby convention.

```ruby
def my_method
  @items.each do |item|
    do_something_with(item)
    do_something_else
  end
end
```


#### Leading Spaces

Leading spaces between lines or at the end of lines should be removed. Git will
point these out with red highlights.

```ruby
def my_method
··array = [1, 2, 3]

··array.each do |i|
····number = an_equation(i)

····do_something_with(number)
··end
end
```


#### Trailing Commas

Use trailing commas at the end of multi-line lists. This prevents the diff
from showing lines that weren't actually changed if a contributor needs to
add to the list. It also makes it easier to copy/paste and rearrange lines.

```ruby
data = [
  "foo",
  "bar",
  "baz",
]

data = {
  foo: 1,
  bar: 2,
  baz: 3,
}
```


#### Parenthesis

Parenthesis should be used when parameters are specified in a method call, as
well as in method definitions.

```ruby
def my_method(x, y)
  File.exists?(File.expand_path("../lib", __FILE__))
end

let(:foo) { "bar" }
```

Parenthesis can be omitted when calling DSL methods, or if the method definition
or method call does not have any arguments.

```ruby
class Thing
  attr_accessor :foo
  attr_accessor :bar
end

on "index" do
  present Model::User.find(id: 1)
end

def method_without_args
  "hello world"
end

method_without_args.upcase
```


#### Hash Rocket

Prefer the colon syntax over the hash rocket `=>`. This takes up less space,
requires fewer characters, and more closely relates the value to the key.

```ruby
data = {foo: 1, bar: 2, baz: 3}
```


#### Keyword Args

Prefer `**kwargs` over `opts = {}` when targeting Ruby >= 2.0.

```ruby
def my_method(**kwargs)
  var_1 = kwargs.fetch(:var_1)
end

def my_method(var_1: 123)
  do_something_with(var_1)
end
```


#### Blocks: `do` vs `{}`

Prefer `{}` for block chaining. Prefer `do` for multi-line blocks that aren't
chained. If you have to chain multi-line blocks, prefer `{}` but use your best
judgement.

```ruby
list
  .map { |i| i + 1 }
  .reduce { |acc, i| acc + i }

list
  .map { |i|
    i = 10 unless i
    secret_formula(i)
  }
  .reduce { |acc, i|
    data = i > 50 ? formula_1(i) : formula_2(i)
    acc + data
  }

XML::Doc.new.tap do |doc|
  XML::Element.new("Data").tap do |data_element|
    doc << data_element
    
    XML::Element.new("Result").tap do |result|
      data_element << result
      
      result.text = "4"
    end
  end
end
```


#### Ternary

Prefer the ternary operator `?` for conditional assignment that can fit on a
single line. Do not use it to replace conditional logic.

```ruby
x = data > 50 ? formula_1(data) : formula_2(data)
```

If the ternary logic is relatively simple but the line is long due to variable
or method names, split the line on `?` and `:`.

```ruby
variable_to_capture_data = some_var_with_a_long_name_to_compare > 10 \
  ? run_formula_1(the_data)
  : run_formula_2(the_data)
```

Don't do this!

```ruby
threat_level > 50 ? fire_missiles : remain_calm
```


#### Logical Operators

`and`, `or` vs `&&`, `||`

For logical operations, prefer using the symbols (`&&`), not the words (`and`).
Unfortunately the word-versions of the operators can have unexpected results in
some cases.

```ruby
# Good
if foo || bar
  perform_task
end

x = foo || bar
```


#### Whitespace

Whitespace should be used reasonably, but not excessively. The code should flow
smoothly and be easy to read. As in graphic design, the use of whitespace should
be used as a tool to move the viewer's eye through the document and accentuate
areas of importance.

Whitespace should be used:
  * After a comma `,`
  * After a colon `:`, like when defining a hash
  * Around operators, like `+` or `=>`
  * Around the braces `{}` of a block (not a hash)
  * To separate logical groups of code or ideas

##### No spaces inside array or hash braces

Note that spaces are not necessary directly inside the brackets `[]`. This is
because the brackets are enclosing related items, so the close proximity of the
brackets to the data better conveys the concept of a group.

```ruby
data = ["foo", "bar", "baz"]
```

The same applies to a hash, where spaces are not necessary directly inside `{}`.
(It's the same philosophy of a tuple in Elixir.)

```ruby
data = {foo: 1, bar: 2, baz: 3}

def my_method(data: 25)
  # ...
end
```

##### Spaces after commas and colons

Spaces should be used after commas and colons.

```ruby
data = ["foo", "bar", "baz"]

data = {foo: 1, bar: 2, baz: 3}
```

##### Spaces around `{}` of a block
Whitespace helps to separate the block parameters from the statement,
making the code easier to read and separating unrelated elements. If the
block doesn't contain parameters, include spaces around `{}` for consistency.
Also include spaces around the block's parameter definition `|x|` if it is
present.

```ruby
data.map { |item| item + 1 }

let(:foo) { "bar" }
```

##### Spaces around operators

```ruby
x = data > 10 ? data + 5 : data * 2
```

##### Whitespace separating logical groups: Assignment, calculation, return value

```ruby
def my_method(data)
  x = data || 25
  
  computed_data =
    @list
      .map { |i| secret_formula(i) } # Spaces around `{}` of a block.
      .reduce { |acc, i| acc + i }
  
  computed_data + x
end 
```

This is a gray area where use of a separating line is up to the discretion of
the author. A two-line map statement like this is so simple that adding a
separating line probably doesn't increase the clarity of the code.

```ruby
list.map do |i|
  i = 10 unless i
  secret_formula(i)
end
```

If the two statements are more complex, then a separating line might increase
clarity and readability.

```ruby
list.map do |i|
  x = i > 50 ? formula_1(i) : formula_2(i)
  
  secret_formula(x)
end
```
