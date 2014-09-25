# Lamprey

The lamprey is a jawless fish that lives off the blood of other fish. It's also
a new language.

Lamprey's goal is to leverage the amazing, tastey work of the
[Elixir community](http://elixir-lang.org).

Here's the problem. Lots of people love Erlang, including its
[hideous syntax](https://www.youtube.com/watch?v=zY-IueSMAPc), but are
frustrated by the library inconsistency, documentation, and tools. They look
with envy to the Elixir community, which has a wonderful opportunity to rethink
the language and add new features. But Elixir has soft underbelly that can be
exposed -- it runs on the Erlang VM. That means it compiles to BEAM files. That
means anything Elixir produces must ultimately remain compatable with *Erlang*.

So here's what Lamprey does. It fixes itself to Elixir and provides all of the
wonderful library consistency, documentation, and tools to Erlang programmers
-- but as Erlang syntax.

Here's an example. Consider the Hello example provided in the first few
paragraphs of [http://elixir-lang.org/](http://elixir-lang.org):

```
defmodule Hello do
  IO.puts "Defining the function world"

  def world do
    IO.puts "Hello World"
  end

  IO.puts "Function world defined"
end

Hello.world
```

Here it is in Lamprey:

``` erlang
-module(hello).

world() ->
    io:puts("Hello World").
```

Good old, understandable Erlang. What about the other parts of the Elixir
example? You mean the part where the language is used to define a new function?
Bleh, this is confusing to Erlang programmers. Lamprey ignores that part.

Here's the next example, the MathTest module:

```
defmodule MathTest do
  use ExUnit.Case, async: true

  test "can add two numbers" do
      assert 1 + 1 == 2
  end
end
```

Elixir's meta programming voodoo is of no interest to Lamprey. It seeks out
fresh Erlang compatible patterns.

Next example:

```
defmodule MyModule do
  @moduledoc """
  Documentation for my module. With **formatting**.
  """

  @doc "Hello"
  def world do
    "World"
  end
end
```

In Lamprey:

```
-module(mymodule).

% @moduledoc
% Documentation for my module. With **formatting**.

% @doc "Hello"
hello() -> "World".
```

Here's something that's just plain annoying in Elixir:

```
{a, b, c} = {:hello, "world", 42}
```

Atoms perform critical roles in Erlang. Why add line noise to atoms just
because you don't like to capitalize your variables? Lamprey fixes it:

```erlang
{A, B, C} = {hello, "world", 42}
```

Lamprey feels better and so will you!

Elixir places a syntax tax, or synTAX, on creating functions, forcing your
poor, tired little fingers to type "def" and "defp" all the time:

```
defmodule Math do
  def sum(a, b) do
    do_sum(a, b)
  end

  defp do_sum(a, b) do
    a + b
  end
end
```

Lamprey lets you cheat:

```erlang
-module(math).

-export([sum/2]).

sum(A, B) -> do_sum(A, B).

do_sum(A, B) -> A + B.
```

Your fingers feel better already! True, we had to type the export line. But
this has value - you can see what's exported, at a glance!

You think that's an improve, try this A/B comparison:

Elixir:

```
defmodule Math do
  def zero?(0) do
    true
  end

  def zero?(x) when is_number(x) do
    false
  end
end
```

Lamprey:

```erlang
-module(math).

zero(0) -> true;
zero(X) when is_number(X) -> false.
```

What about awesome Elixir features that aren't supported by Erlang syntax?
Lamprey simply doesn't use them. Take default argument for example:

```
defmodule Concat do
  def join(a, b, sep \\ " ") do
    a <> sep <> b
  end
end
```

In Lamprey, you'd use the old fashion approach:

```erlang
-module(contact).

join(A, B) -> join(A, B, " ").

join(A, B, C) -> A ++ B ++ C.
```

Could Lamprey ever modify the Erlang syntax to support something like this?

```erlang
-module(contact).

join(A, B, C \\ " ") -> A ++ B ++ C.
```

Sure, but like the fish, Lamprey the language doesn't like to work too hard. So
probably not.
