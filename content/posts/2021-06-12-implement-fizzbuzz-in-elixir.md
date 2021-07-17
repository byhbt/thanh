---
categories:
  - Elixir
tags:
  - elixir
  - fizzbuzz
title: Implement Fizzbuzz in Elixir
date: 2021-06-12
---


Let solve the classic programming challenge called "Fizzbuzz". I get the problem statement from [wiki.c2.com](https://wiki.c2.com/?FizzBuzzTest).

## Problem

> "Write a program that prints the numbers from 1 to 100. But for multiples of three print “Fizz” instead of the number and the multiples of five print “Buzz”. For numbers which are multiples of both three and five print “FizzBuzz”."

Then let break it down:

- Given X from 1 -> 100
- If X multiple of 3 return `Fizz`
- If X multiple of 5 return `Buzz`
- If X multiple of 3 and 5 return `FizzBuzz`
- Otherwise return the number

Above all, make sure we understand the problem correctly. If yes, cool! So now let the hacking begin!

## Implementation

Let use the Mix tool to initialize a new project:

```bash
mix new fizzbuzz
```

And here is the generated directory structure.

```bash
.
├── README.md
├── _build
│   ├── dev
│   └── test
├── lib
│   └── fizzbuzz.ex
├── mix.exs
└── test
    ├── fizzbuzz_test.exs
    └── test_helper.exs
```

Follow TDD approach, we start by writing some tests in the `fizzbuzz_test.exs` first.

```elixir

defmodule FizzbuzzTest do
  use ExUnit.Case

  import ExUnit.CaptureIO

  describe "go/1" do
    test "return fizz given the divisible number by 3" do
      assert Fizzbuzz.go(3) == "fizz"
    end

    test "returns buzz given the divisible number by 5" do
      assert Fizzbuzz.go(5) == "buzz"
    end

    test "return fizzbuzz given the divisible number by 3 and 5" do
      assert Fizzbuzz.go(15) == "fizzbuzz"
    end

    test "return the number given the indivisible number by 3 and 5" do
      assert Fizzbuzz.go(11) == 11
    end
  end

  describe "go/2" do
    test "prints the result of each number given a list" do
      assert capture_io(fn -> Fizzbuzz.go(14, 15) end) == "14\nfizzbuzz\n"
    end
  end
end


```

Then implement the main program. While implementing, we can run the test by using `mix test` command to verify our implementation.

```elixir
defmodule Fizzbuzz do
  def go(min, max) do
    Enum.each(min..max, fn n -> IO.puts go(n) end)
  end

  def go(num) do
    case { rem(num, 3), rem(num, 5) } do
      {0, 0} -> "fizzbuzz"
      {0, _} -> "fizz"
      {_, 0} -> "buzz"
      _ -> num
    end
  end
end
```

## How to run it

We can run the programming by this command

```bash
iex -S mix
```

In the Elixir IEX

```bash
Fizzbuzz.go(4)
Fizzbuzz.go(0, 100)

r(Fizzbuzz) # for reloading the application if any code change.
```

## Learning

If someone says this program is for a newbie and simple, - yes. However, for a newcomer to the Elixir world, I think it is useful to help you learn the syntax as well as the language features.

What we can learn from the above program:

- TDD
- Some basic syntax in Elixir
- Testing in Elixir
- Practice some shortcut key with your IDE/Editor

In the end, please try to read, understand and rewrite it in another way if you could.

Thank you for your reading!