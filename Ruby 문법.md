# Ruby Syntax

https://ruby-doc.org/docs/ruby-doc-bundle/Manual/man-1.4/syntax.html#ident

## 주석

#### 한 줄짜리 주석

```ruby
# this is a comment.
```

#### 여러줄

```ruby
=begin
`=begin`과 `=end` 사이에 있는 모든 텍스트는 인터프리터가 무시함.
Every text betwen `=begin` and `=end` will be ignored by interpreter.
=end
```



## 변수와 상수

변수에는 global, instance, local 변수가 있고, 상수에는 class 상수가 있다. 모두 이름의 첫 글자로 구분한다.

#### Global variables

```ruby
$foobar
```

- `$`로 시작.
- 프로그램 어디서든 사용 가능
- 초기화 하지 않았을 때는 `nil`을 갖는다.

#### Instance variables

```ruby
@foobar
```

- `@`로 시작.
- 특정 오브젝트 안에서만 사용 가능
- 초기화 하지 않았을 때는 `nil`을 갖는다.

#### Constants

```
FOOBAR
Foobar
```

- 대문자로 시작.
- 반드시 1번 값을 할당해야 한다.
  - 상수 선언과 할당을 같이 해야 하는 것은 아니다. 
- 클래스 내부에 있는 상수를 사용하려면 `::` 연산자를 쓰면 된다.

```ruby
Foo::Bar
::Bar # `Object` 클래스에 정의된 Bar 함수 사용
```

#### Local variables

```ruby
foobar
```

- 소문자로 시작.

## 표현식

#### Array expression

```ruby
[1, 2, 3]
```

- Array 클래스의 인스턴스를 반환한다.

```ruby
%w(foo bar baz)
```

- 위 표현식은 편의를 위한 것으로,  `["foo", "bar", "baz"]`와 같은 의미를 갖는다.

#### Hash expression

```ruby
{1=>2, 2=>4, 3=>6}
```

## 할당문

```ruby
foo = bar
foo[0] = bar
foo.bar = baz
```

#### Self assignment

아래 문법을 지원한다.

```ruby
foo += 12
```

#### Multiple assignment

```ruby
foo, bar, baz = 1, 2, 3
foo, bar = 1 # foo = 1; bar = nil;
foo, *bar = 1, 2, 3 # foo = 1; bar = [2, 3]
```

## 제어문

#### If statement

`if`, `unless`,  `elsif`, `else`, `end` 키워드를 사용한다.

- `unless (condition)`은 `if !(condition)`과 같은 의미를 가진다.

```ruby
if age >= 12 then
  print "adult fee\n"
elsif age >= 7 then
  print "child fee\n"
else
  print "baby fee\n"
end
```

Inline 제어문도 사용 할 수 있다.

```ruby
gender = if foo.gender == "male" then "male" else "female" end
```

#### Switch statement

`case`, `when`, `else`, `end` 키워드를 사용한다. 비교를 위해서는 `..`, `,`, `===` 연산자를 사용할 수 있다.

```ruby
	case $age
	when 0 .. 2
	  "baby"
	when 3 .. 6
	  "little child"
	when 7 .. 12
	  "child"
	when 12 .. 18
	  # Note: 12 already matched by "child"
	  "youth"
	else
	  "adult"
	end
```

```ruby
case expr0
when expr1, expr2
  stmt1
when expr3, expr4
  stmt2
else
  stmt3
end

# 위 코드는 아래 코드와 같다.
_tmp = expr0
if expr1 === _tmp || expr2 === _tmp
  stmt1
elsif expr3 === _tmp || expr4 === _tmp
  stmt2
else
  stmt3
end
```

## 반복자

`do .. end`나 `{..}`로 만든 코드블록을 반복해서 실행한다.

```ruby
[1, 2, 3].each do |i| print i*2, "\n" end
[1, 2, 3].each{|i| print i*2, "\n"}
```

#### For loop

```ruby
for i in [1, 2, 3]
  print i*2, "\n"
end
```

## 함수

#### 정의

`def`, `end` 키워드를 사용한다.

```ruby
def fact(n)
  if n == 1 then
    1 
  else
    n * fact(n-1)
  end
  end
```

## 클래스

https://dev-yakuza.posstree.com/ko/ruby/class/

#### 선언

```ruby
class Book
  ...
end
```

필드 변수를 미리 나열하지는 않는다.

#### 생성자

```ruby
book = Book.new
```

- `{클래스명}.new` 로 새 인스턴스를 생성한다.

#### 오브젝트 초기화

```ruby
class Book
  def initialize (author)
    @author = author
  end
end

book = Book.new('dev-yakuza')
book.printAuthor
# dev-yakuza
```

- `initialize` 메서드는 `new` 메서드로 새 오브젝트를 생성할 때 자동으로 호출된다.	

#### 상속

#### 상속

```ruby
	class Foo < Super
	  def test
	     :
	  end
	     :
	end
```

#### 인스턴스 변수

```ruby
class Book
  def printTitle
    puts @title
  end
  ...
end
```

- `@`로 시작.

##### 인스턴스 변수 접근자

```ruby
class Book
  def initialize
    @title = 'Ruby'
  end

  attr_accessor :title
end

book = Book.new
puts book.title # Ruby
book.title = 'Rails'
puts book.title # Rails
```

- `attr_accessor`, `attr_writer`, `atter_reader`로 사용할 접근자를 정의할 수 있다.

#### 클래스 변수(? 이게 명칭이 맞나)

```
class Book
  @@publisher = 'dev-yakuza'

  def printPublisher
    puts @@publisher
  end
end

book = Book.new
book.publisher
# dev-yakuza
```

- `@@`로 시작.
- 같은 클래스로부터 생성된 모든 오브젝트에서 공유하는 변수.
- 값을 여러번 할당할 수 있음.

#### 클래스 메서드

```ruby
class Greeting
  @@name = 'World'

  def Greeting.hello
    puts 'Hello ' + @@name
  end
end

Greeting.hello
# Hello World
```

- 같은 클래스로부터 생성된 모든 오브젝트에서 공유하는 메서드.