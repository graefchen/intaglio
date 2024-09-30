# intaglio - language funinition

## base ideas

- small dynamic class-based scripting language
- inspired by
  - io
  - lua
  - ruby
- easy and good to read (like ruby)
- fast
  - one pass compiler
  - register vm (bytecode)
  - gc (todo)
    - piccole [ref](https://kyju.org/blog/piccolo-a-stackless-lua-interpreter/)
    - grimoire [ref](https://verdagon.dev/grimoire/grimoire)
    - ci [ref](https://tunglevo.com/note/crafting-interpreters-with-rust-on-garbage-collection/)
    - ci [ref](https://ceronman.com/2021/07/22/my-experience-crafting-an-interpreter-with-rust/)
  - possible extensions
    - multithread ([ref](https://blog.subnetzero.io/post/building-language-vm-part-17/))
    - clustering ([ref](https://blog.subnetzero.io/post/building-language-vm-part-27/))
- small

## design

> [!NOTE]
> Under the Design it isn't understood that only the languge by itself should be designed,
> but also the whole command line interface and error messages.
> In the best case you could go from an empty file to an functining programm.
> Use elm and gleam as a heavy reference for this.

### command line

Interface over subcommands.

#### intaglio

```bash
$ intaglio
A small dynamic class-based scripting language.

Usage: intaglio.exe <COMMAND>

Commands:
  init     Initialize a new file.
  compile  Compile files into bytecode.
  run      Run a file.
  eval     Evaluate a string.
  fmt      Format file/directoy.
  repl     Start the REPL.
  help     Print help message.

Options:
  -h, --help     Print help
  -V, --version  Print version
```

#### intaglio init

```bash
$ intaglio init --help
Initialize a new file.

Usage: intaglio.exe init

Options:
  -h, --help  Print help
```

#### intaglio compile

```bash
$ intaglio compile --help
Initialize a new file.

Usage: intaglio.exe compile [FILE]...

Arguments:
  FILE  The file to be run

Options:
  -h, --help  Print help
```

#### intaglio run

```bash
$ intaglio run --help
Run a file.

Usage: intaglio.exe run <FILE>

Arguments:
  FILE  The file to be run

Options:
  -h, --help  Print help
```

#### intaglio eval

```bash
$ intaglio eval --help
Evaluate a string.

Usage: intaglio.exe eval <STRING>

Arguments:
  STRING  The string to be evaluated

Options:
  -h, --help  Print help
```

#### intaglio fmt

- [ref](https://www.ntietz.com/blog/writing-basic-code-formatter/)
- [ref](https://journal.stuffwithstuff.com/2015/09/08/the-hardest-program-ive-ever-written/)

```bash
$ intaglio fmt --help
Format file/directoy.

Usage: intaglio.exe fmt <PATH>

Arguments:
  PATH  The file/directory to format.

Options:
  -h, --help  Print help
```

#### intaglio repl

inspired by:

- elm
- io
- julia
- numbat
- python
- uiua
- r
- ruby

Help message for `intaglio repl`:

```bash
$ intaglio repl --help
Start the REPL.

Usage: intaglio.exe repl

Options:
  -h, --help  Print help
```

Running command `intaglio repl`:

(If possible the output should be colored)

```bash
$ intaglio repl
Intaglio 0.0.1  :help for help  Ctrl+C to exit
ingl:0:1> 1 + 1
2
ingl:0:2> exit
$
```

#### intaglio help

```bash
$ intaglio help
Print help message.

Usage: intaglio.exe help [COMMAND]

Commands:
  init     Initialize a new file.
  compile  Compile files into bytecode.
  run      Run a file.
  eval     Evaluate a string.
  fmt      Format file/directoy.
  repl     Start the REPL.
  help     Print help message.

Options:
  -h, --help  Print help
```

### language

- [ref](https://ruby-doc.org/docs/ruby-doc-bundle/Manual/man-1.4/syntax.html)
- [ref](https://devhints.io/lua)

#### operators

| symbol | type               | function              |
| ------ | ------------------ | --------------------- |
| ==     | relational(binary) | equal                 |
| <      | relational(binary) | less than             |
| >      | relational(binary) | greater than          |
| <=     | relational(binary) | less than or equal    |
| >=     | relational(binary) | greater than or equal |
| !=     | relational(binary) | not equal             |
| +      | arithmetic(binary) | addition              |
| \-     | arithmetic(binary) | subtraction           |
| \*     | arithmetic(binary) | multiplication        |
| /      | arithmetic(binary) | division              |
| %      | arithmetic(binary) | modulo                |
| ^      | arithmetic(binary) | power                 |
| \-     | arithmetic(unary)  | negate                |
| &&     | logic              | logical and           |
| \|\|   | logic              | logical or            |
| !      | logic              | logical not           |
| ..     | others(binary)     | string concatonation  |

#### comments

```
# This here is an integlio comment
```

#### variables

```
a = 100
b = 2.5
c = "Hello, 🌍"
d = [ "Hello", "World" ]
```

#### constants

```
nil
false
true
```

#### keywords

```
if
else
function
class
end
```

#### control flow

```
if true
  println("yes")
else
  println("no")
end

i = 0
while i < 10
  i = i + 1
end

for i = 1, i < 10, i = i + 1

end

```

#### functions

```
function hello()
  h = "Hello, 🌍"
  # function calls can be with or without brackets
  println(h)
  println h
end

function hi(name)
  println("Hello, " .. name)
end

hello()
hi("🌍")
```

#### closures

```
function hello()
  hw = "Hello, 🌍"
  println(h)
end

hi = hello()
hi()
```

#### classe

```
class bird
  # adding a variable
  name = "Raven"

  # adding a function
  function scream(noise)
    println(noise)
  end
end

bird.scream("AWK")

raven = new bird()
raven.scream("KRAH")

class Vector
  x = 1, y = 1

  function init(x,y)
    self.x = x
    self.y = y
  end

  function length()
    return sqrt(self.x * self.x + self.y * self.y)
  end
end
```

#### buildin-functions

| name    | desciption                        | argument 1 |
| ------- | --------------------------------- | ---------- |
| load    | loading an file into the vm       | string     |
| print   | printing an object                | string     |
| println | printing an object with a newline | string     |
