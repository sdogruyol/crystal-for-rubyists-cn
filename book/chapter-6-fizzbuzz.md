# 第 6 章：FizzBuzz 问题

当然,当你接受面试的时候, Crystal 要你做的第一件事就是 FizzBuzz 问题！！！让我们开始吧

如果你还不熟悉 FizzBuzz 的话,它其实是一个简单的编程问题:

> 编写一个程序，打印数字 1 到 100.如果中途遇到 3 的倍数那么打
> 印 “Fizz” 替代这个数，同理如果遇到 5 的倍数打印 “Buzz” 替代这个数，如果既
> 是 3 也是 5 的倍数那么打印 “FizzBuzz” 替代这个数。

这个程序为面试官检验你的 Crystal 基础如：循环、测试、标准输出打印，等其他基础知识提供了很好的依据。

首先让我们创建一个项目.

    $ crystal init app fizzbuzz

然后写我们的第一个失败 (failing) 的测试，打开 `/spec/fizzbuz_spec.cr`

```ruby
require "./spec_helper"

describe Fizzbuzz do
  it "shouldn't divide 1 by 3" do
    div_by_three(1).should eq(false)
  end
end
```

运行它:

    $ crystal spec
    Error in ./spec/fizzbuzz_spec.cr:7: undefined method 'div_by_three'

    div_by_three(1).should eq(false)


这是因为：我们还没有定义任何方法，让我们定义一个:

```ruby
require "./fizzbuzz/*"

def div_by_three(n)
  false
end
```

与 Ruby 类似，最后一个语句的值会被返回。

TDD 的意思是做最简单的事，现在我们定义了一个方法，让我们编译并且运行：

    $  crystal spec
    .

    Finished in 0.82 milliseconds
    1 examples, 0 failures, 0 errors, 0 pending


真棒，我们通过了，再写另一个测试看看会发生什么

```ruby
require "./spec_helper"

describe Fizzbuzz do
  it "shouldn't divide 1 by 3" do
    div_by_three(1).should eq(false)
  end

  it "should divide 3 by 3" do
    div_by_three(3).should eq(true)
  end
end
```

运行一下：

    $ crystal spec

    .F

    Failures:

      1) Fizzbuzz should divide 3 by 3
         Failure/Error: div_by_three(3).should eq(true)

           expected: true
                got: false

         # ./spec/fizzbuzz_spec.cr:9

    Finished in 0.83 milliseconds
    2 examples, 1 failures, 0 errors, 0 pending

    Failed examples:

    crystal spec ./spec/fizzbuzz_spec.cr:8 # Fizzbuzz should divide 3 by 3

有一个错误，解决它:

```ruby
require "./fizzbuzz/*"

def div_by_three(n)
  if n % 3 == 0
    true
  else
    false
  end
end
```

运行

    $ crystal spec

    ..

    Finished in 0.61 milliseconds
    2 examples, 0 failures, 0 errors, 0 pending

溜溜溜，这个展示了 “else” 如何工作，正如你想要的，试试把它重构成一行呢

看看我的:

完成？你会怎么写，看看我的：

```ruby
def div_by_three(n)
  n % 3 == 0
end
```

记住，最后一个语句的值会被返回。好啦，让我们 TDD 两个方法 `divide_by_five` 和 `divide_by_three`，他们的原理是一样的,
但是下面的代码要试着写出完整程序了，一旦你看到了他们，说明你已经准备好了.


    $ crystal spec -v

    Fizzbuzz
      shouldn't divide 1 by 3
      should divide 3 by 3
      shouldn't divide 8 by 5
      should divide 5 by 5
      shouldn't divide 13 by 15
      should divide 15 by 15

    Finished in 0.61 milliseconds
    6 examples, 0 failures, 0 errors, 0 pending

好啦，现在让我们讨论下主程序，有了编写 FizzBuzz 的工具，让我们把它跑起来，第一件事就是打印数字 1 到 100，很简单

```ruby
100.times do |num|
  puts num
end
```

第一步，打印 100 次，如果你通过这个： `crystal build src/fizzbuzz.cr && ./fizzbuzz` 运行，你会看见**数字**打印了100次。需要提醒的是我们的测试还没有运行，实际上不仅是没有运行，他们根本就不能执行，现在我们把两段代码耦合到一起:

现在我们可以把两者结合起来:

```ruby
100.times do |num|
  answer = ""

  if div_by_fifteen num
    answer = "FizzBuzz"
  elsif div_by_three num
    answer = "Fizz"
  elsif div_by_five num
    answer = "Buzz"
  else
    answer = num
  end

  puts answer
end
```

由于 `if` 语句要返回值，所以我们因该这样：

```ruby
(1..100).each do |num|
  answer = if div_by_fifteen num
    "FizzBuzz"
  elsif div_by_three num
    "Fizz"
  elsif div_by_five num
    "Buzz"
  else
    num
  end

  puts answer
end
```

注意我们把 `100.times` 改为 `(1..100).each` 令数字从 1 到 100 而不是 0 到 99。
试着运行一下。
很好我们已经拿下 FizzBuzz 了。
