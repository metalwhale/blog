---
title: "Julia Cheat Sheet"
date: 2020-06-14T02:35:57Z
description: "Julia manual at a glance"
enableToc: true
tags:
- Julia
categories:
- Programming
---

Reference: [Julia Documentation](https://docs.julialang.org/)

## Integers and Floating-Point Numbers

#### Numeric Literal Coefficients
- Allows variables to be immediately preceded by a numeric literal, implying multiplication:
``` julia
julia> x = 3
3

julia> 2x^2 - 3x + 1
10

julia> 1.5x^2 - .5x + 1
13.0
```

## Mathematical Operations and Elementary Functions

#### Vectorized "dot" operators
- For *every* binary operation like `^`, there is a corresponding "dot" operation `.^` that is *automatically* defined to perform `^` element-by-element on arrays:
``` julia
julia> [1,2,3] .^ 3
3-element Array{Int64,1}:
  1
  8
 27
```

#### Numeric Comparisons
- Comparisons can be arbitrarily chained:
``` julia
julia> 1 < 2 <= 2 < 3 == 3 > 2 >= 1 == 1 < 3 != 5
true
```

#### Operator Precedence and Associativity
| Category       | Operators                                             | Associativity   |
| ---------------| ----------------------------------------------------- | --------------- |
| Syntax         | `.` followed by `::`                                  | Left            |
| Exponentiation | `^`                                                   | Right           |
| Unary          | `+ - √`                                               | Right           |
| Bitshifts      | `<< >> >>>`                                           | Left            |
| Fractions      | `//`                                                  | Left            |
| Multiplication | `* / % & \ ÷`                                         | Left            |
| Addition       | `+ - | ⊻`                                             | Left            |
| Syntax         | `: ..`                                                | Left            |
| Syntax         | `|>`                                                  | Left            |
| Syntax         | `<|`                                                  | Right           |
| Comparisons    | `> < >= <= == === != !== <:`                          | Non-associative |
| Control flow   | `&& followed by || followed by ?`                     | Right           |
| Pair           | `=>`                                                  | Right           |
| Assignments    | `= += -= *= /= //= \= ^= ÷= %= |= &= ⊻= <<= >>= >>>=` | Right           |

## Complex and Rational Numbers

#### Complex Numbers
- The global constant `im` is bound to the complex number *i*:
``` julia
julia> 1+2im
1 + 2im
```

#### Rational Numbers
- Represent exact ratios of integers:
``` julia
julia> 6//9
2//3

julia> -4//8
-1//2

julia> 5//-15
-1//3

julia> -4//-12
1//3
```

## Strings

#### String Basics
- String literals:
``` julia
julia> str = "Hello, world.\n"
"Hello, world.\n"

julia> """Contains "quote" characters"""
"Contains \"quote\" characters"
```
- Extract a character from a string:
``` julia
julia> str[begin]
'H': ASCII/Unicode U+0048 (category Lu: Letter, uppercase)

julia> str[1]
'H': ASCII/Unicode U+0048 (category Lu: Letter, uppercase)

julia> str[6]
',': ASCII/Unicode U+002C (category Po: Punctuation, other)

julia> str[end]
'\n': ASCII/Unicode U+000A (category Cc: Other, control)
```
- Extract a substring using range indexing:
``` julia
julia> str[4:9]
"lo, wo"
```

#### Concatenation
``` julia
julia> greet = "Hello"
"Hello"

julia> whom = "world"
"world"

julia> string(greet, ", ", whom, ".\n")
"Hello, world.\n"
```
- Provides `*` for string concatenation:
``` julia
julia> greet * ", " * whom * ".\n"
"Hello, world.\n"
```

#### Interpolation
- Allows interpolation into string literals using `$`:
``` julia
julia> "$greet, $whom.\n"
"Hello, world.\n"
```
- Interpolate any expression into a string using parentheses:
``` julia
julia> "1 + 2 = $(1 + 2)"
"1 + 2 = 3"
```

#### Regular Expressions
- The most basic regular expression literal without any options turned on just uses `r"..."`:
``` julia
julia> r"^\s*(?:#|$)"
r"^\s*(?:#|$)"

julia> typeof(ans)
Regex
```
- To capture this information about a match, use the `match` function:
``` julia
julia> match(r"^\s*(?:#|$)", "not a comment")

julia> match(r"^\s*(?:#|$)", "# a comment")
RegexMatch("#")
```

#### Version Number Literals
- Expressed with non-standard string literals of the form `v"..."`:
``` julia
if v"0.2" <= VERSION < v"0.3-"
    # do something specific to 0.2 release series
end
```

#### Raw String Literals
- Expressed with non-standard string literals of the form `raw"..."`:
``` julia
julia> println(raw"\\ \\\"")
\\ \"
```

## Functions
- Basic syntax:
``` julia
julia> function f(x,y)
           x + y
       end
f (generic function with 1 method)
```
- Assignment form:
``` julia
julia> f(x,y) = x + y
f (generic function with 1 method)
```

#### The `return` Keyword
- A return type can be specified in the function declaration using the `::` operator:
``` julia
julia> function g(x, y)::Int8
           return x * y
       end;

julia> typeof(g(1, 2))
Int8
```

- Return the value `nothing`:
``` julia
function printx(x)
    println("x = $x")
    return nothing
end
```

#### Operators With Special Names
| Expression        | Calls          |
| ----------------- | -------------- |
| `[A B C ...]`     | `hcat`         |
| `[A; B; C; ...]`  | `vcat`         |
| `[A B; C D; ...]` | `hvcat`        |
| `A'`              | `adjoint`      |
| `A[i]`            | `getindex`     |
| `A[i] = x`        | `setindex!`    |
| `A.n`             | `getproperty`  |
| `A.n = x`         | `setproperty!` |

#### Anonymous Functions
- Created anonymously, without being given a name:
``` julia
julia> x -> x^2 + 2x - 1
#1 (generic function with 1 method)

julia> function (x)
           x^2 + 2x - 1
       end
#3 (generic function with 1 method)
```

#### Named Tuples
- The components of tuples can optionally be named:
``` julia
julia> x = (a=1, b=1+1)
(a = 1, b = 2)

julia> x.a
1
```

#### Argument destructuring
- If a function argument name is written as a tuple (e.g. `(x, y)`) instead of just a symbol, then an assignment `(x, y) = argument` will be inserted:
``` julia
julia> minmax(x, y) = (y < x) ? (y, x) : (x, y)

julia> range((min, max)) = max - min

julia> range(minmax(10, 2))
8
```

#### Varargs Functions
- Taking an arbitrary number of arguments:
``` julia
julia> bar(a,b,x...) = (a,b,x)
bar (generic function with 1 method)
```
- `x` is bound to a tuple of the trailing values passed to `bar`:
``` julia
julia> bar(1,2)
(1, 2, ())

julia> bar(1,2,3)
(1, 2, (3,))

julia> bar(1, 2, 3, 4)
(1, 2, (3, 4))

julia> bar(1,2,3,4,5,6)
(1, 2, (3, 4, 5, 6))
```

- "Splat" the values contained in an iterable collection into a function call as individual arguments:
``` julia
julia> x = (3, 4)
(3, 4)

julia> bar(1,2,x...)
(1, 2, (3, 4))
```
- A tuple of values is spliced into a varargs call precisely where the variable number of arguments go:
``` julia
julia> x = (2, 3, 4)
(2, 3, 4)

julia> bar(1,x...)
(1, 2, (3, 4))

julia> x = (1, 2, 3, 4)
(1, 2, 3, 4)

julia> bar(x...)
(1, 2, (3, 4))
```
- The iterable object splatted into a function call need not be a tuple:
``` julia
julia> x = [3,4]
2-element Array{Int64,1}:
 3
 4

julia> bar(1,2,x...)
(1, 2, (3, 4))

julia> x = [1,2,3,4]
4-element Array{Int64,1}:
 1
 2
 3
 4

julia> bar(x...)
(1, 2, (3, 4))
```

#### Optional Arguments
- Arguments have sensible default values and therefore might not need to be passed explicitly in every call:
``` julia
function Date(y::Int64, m::Int64=1, d::Int64=1)
    err = validargs(Date, y, m, d)
    err === nothing || throw(err)
    return Date(UTD(totaldays(y, m, d)))
end
```

#### Keyword Arguments
- Allowing arguments to be identified by name instead of only by position:
``` julia
function plot(x, y; style="solid", width=1, color="black")
    ###
end
```
- Extra keyword arguments can be collected using `...`:
``` julia
function f(x; y=0, kwargs...)
    ###
end
```

#### Do-Block Syntax for Function Arguments
``` julia
map([A, B, C]) do x
    if x < 0 && iseven(x)
        return 0
    elseif x == 0
        return 1
    else
        return x
    end
end
```

#### Function composition and piping
- Use the function composition operator (`∘`) to compose the functions:
``` julia
julia> (sqrt ∘ +)(3, 6)
3.0
```
- Function chaining:
``` julia
julia> 1:10 |> sum |> sqrt
7.416198487095663
```

#### Dot Syntax for Vectorizing Functions
- *Any* Julia function `f` can be applied elementwise to any array (or other collection) with the syntax `f.(A)`:
``` julia
julia> A = [1.0, 2.0, 3.0]
3-element Array{Float64,1}:
 1.0
 2.0
 3.0

julia> sin.(A)
3-element Array{Float64,1}:
 0.8414709848078965
 0.9092974268256817
 0.1411200080598672
```
- The macro `@.` is provided to convert *every* function call, operation, and assignment in an expression into the "dotted" version:
``` julia
julia> Y = [1.0, 2.0, 3.0, 4.0];

julia> X = similar(Y); # pre-allocate output array

julia> @. X = sin(cos(Y)) # equivalent to X .= sin.(cos.(Y))
4-element Array{Float64,1}:
  0.5143952585235492
 -0.4042391538522658
 -0.8360218615377305
 -0.6080830096407656
```

## Control Flow

#### Compound Expressions
- An example of a `begin` block:
``` julia
julia> z = begin
           x = 1
           y = 2
           x + y
       end
3
```
- The `;` chain syntax:
``` julia
julia> z = (x = 1; y = 2; x + y)
3
```

#### Conditional Evaluation
- The anatomy of the `if-elseif-else` conditional syntax:
``` julia
if x < y
    println("x is less than y")
elseif x > y
    println("x is greater than y")
else
    println("x is equal to y")
end
```

#### Repeated Evaluation: Loops
- `while` loop:
``` julia
julia> i = 1;

julia> while i <= 5
           println(i)
           global i += 1
       end
1
2
3
4
5
```
- `for` loop:
``` julia
julia> for i = 1:5
           println(i)
       end
1
2
3
4
5
```
- The `for` loop construct can iterate over any container:
``` julia
julia> for i in [1,4,0]
           println(i)
       end
1
4
0

julia> for s ∈ ["foo","bar","baz"]
           println(s)
       end
foo
bar
baz
```
- Multiple nested `for` loops can be combined into a single outer loop, forming the cartesian product of its iterables:
``` julia
julia> for i = 1:2, j = 3:4
           println((i, j))
       end
(1, 3)
(1, 4)
(2, 3)
(2, 4)
```

#### Tasks (aka Coroutines)
- A producer task, which produces values via the `put!` call:
``` julia
julia> function producer(c::Channel)
           put!(c, "start")
           for n=1:4
               put!(c, 2n)
           end
           put!(c, "stop")
       end;

julia> chnl = Channel(producer);

julia> take!(chnl)
"start"

julia> take!(chnl)
2

julia> take!(chnl)
4

julia> take!(chnl)
6

julia> take!(chnl)
8

julia> take!(chnl)
"stop"
```

## Scope of Variables
| Construct                                        | Scope type | Scope blocks it may be nested in |
| ------------------------------------------------ | ---------- | -------------------------------- |
| `module`, `baremodule`                           | global     | global                           |
| interactive prompt (REPL)                        | global     | global                           |
| (mutable) `struct`, `macro`                      | local      | global                           |
| `for`, `while`, `try-catch-finally`, `let`       | local      | global or local                  |
| functions (either syntax, anonymous & do-blocks) | local      | global or local                  |
| comprehensions, broadcast-fusing                 | local      | global or local                  |

#### Local Scope
- Inside a local scope a variable can be forced to be a new local variable using the `local` keyword:
``` julia
julia> for i = 1:1
           x = i + 1
           for j = 1:1
               local x = 0
           end
           println(x)
       end
2
```
- Inside a local scope a global variable can be assigned to by using the keyword `global`:
``` julia
julia> for i = 1:10
           global z
           z = i
       end

julia> z
10
```
- *Nested functions* can modify their parent scope's *local* variables:
``` julia
julia> x, y = 1, 2;

julia> function baz()
           x = 2 # introduces a new local
           function bar()
               x = 10       # modifies the parent's x
               return x + y # y is global
           end
           return bar() + x # 12 + 10 (x is modified in call of bar())
       end;

julia> baz()
22

julia> x, y # verify that global x and y are unchanged
(1, 2)
```
- `let` statements allocate new variable bindings each time they run:
``` julia
julia> x, y, z = -1, -1, -1;

julia> let x = 1, z
           println("x: $x, y: $y") # x is local variable, y the global
           println("z: $z") # errors as z has not been assigned yet but is local
       end
x: 1, y: -1
ERROR: UndefVarError: z not defined
```
- Use `let` to create a new binding:
``` julia
julia> Fs = Vector{Any}(undef, 2); i = 1;

julia> while i <= 2
           let i = i
               Fs[i] = ()->i
           end
           global i += 1
       end

julia> Fs[1]()
1

julia> Fs[2]()
2
```
- Since the `begin` construct does not introduce a new scope, it can be useful to use a zero-argument `let` to just introduce a new scope block without creating any new bindings:
``` julia
julia> let
           local x = 1
           let
               local x = 2
           end
           x
       end
1
```
- Reuse an existing local variable as the iteration variable:
``` julia
julia> function f()
           i = 0
           for outer i = 1:3
           end
           return i
       end;

julia> f()
3
```

## Types

#### Type Declarations
- Declares the variable to always have the specified type:
``` julia
julia> function foo()
           x::Int8 = 100
           x
       end
foo (generic function with 1 method)

julia> foo()
100

julia> typeof(ans)
Int8
```

#### Abstract Types
- Abstract types are declared using the `abstract type` keyword:
``` julia
abstract type «name» end
abstract type «name» <: «supertype» end
```

#### Primitive Types
- The general syntaxes for declaring a primitive type are:
``` julia
primitive type «name» «bits» end
primitive type «name» <: «supertype» «bits» end
```

#### Composite Types
- Introduced with the `struct` keyword followed by a block of field names, optionally annotated with types using the `::` operator:
``` julia
julia> struct Foo
           bar
           baz::Int
           qux::Float64
       end
```

#### Mutable Composite Types
- If a composite type is declared with `mutable struct` instead of `struct`, then instances of it can be modified:
``` julia
julia> mutable struct Bar
           baz
           qux::Float64
       end

julia> bar = Bar("Hello", 1.5);

julia> bar.qux = 2.0
2.0

julia> bar.baz = 1//2
1//2
```

#### Type Unions
- A type union is a special abstract type which includes as objects all instances of any of its argument types:
``` julia
julia> IntOrString = Union{Int,AbstractString}
Union{Int64, AbstractString}

julia> 1 :: IntOrString
1

julia> "Hello!" :: IntOrString
"Hello!"

julia> 1.0 :: IntOrString
ERROR: TypeError: in typeassert, expected Union{Int64, AbstractString}, got Float64
```

#### Parametric Types
- Type parameters are introduced immediately after the type name, surrounded by curly braces:
``` julia
julia> struct Point{T}
           x::T
           y::T
       end
```
- A correct way to define a method that accepts all arguments of type `Point{T}` where `T` is a subtype of `Real` is:
``` julia
function norm(p::Point{<:Real})
    sqrt(p.x^2 + p.y^2)
end
```
- Parametric abstract type declarations declare a collection of abstract types:
``` julia
julia> abstract type Pointy{T} end
```
- Constrain the range of `T`:
``` julia
julia> abstract type Pointy{T<:Real} end
```

#### UnionAll Types
- `Ptr` itself cannot be a normal data type, since without knowing the type of the referenced data the type clearly cannot be used for memory operations. The answer is that `Ptr` (or other parametric types like `Array`) is a different kind of type called a `UnionAll` type.
- The `where` keyword itself can be nested:
``` julia
julia> const T1 = Array{Array{T,1} where T, 1}
Array{Array{T,1} where T,1}

julia> const T2 = Array{Array{T,1}, 1} where T
Array{Array{T,1},1} where T
```

#### "Value types"
- Accept `Val` instances as arguments:
``` julia
julia> firstlast(::Val{true}) = "First"
firstlast (generic function with 1 method)

julia> firstlast(::Val{false}) = "Last"
firstlast (generic function with 2 methods)

julia> firstlast(Val(true))
"First"

julia> firstlast(Val(false))
"Last"
```

## Methods

#### Parametric Methods
- Method definitions can optionally have type parameters qualifying the signature:
``` julia
julia> same_type(x::T, y::T) where {T} = true
same_type (generic function with 1 method)

julia> same_type(x,y) = false
same_type (generic function with 2 methods)
```
``` julia
julia> same_type(1, 2)
true

julia> same_type(1, 2.0)
false

julia> same_type(1.0, 2.0)
true

julia> same_type("foo", 2.0)
false

julia> same_type("foo", "bar")
true

julia> same_type(Int32(1), Int64(2))
false
```
- Method type parameters are not restricted to being used as the types of arguments: they can be used anywhere a value would be in the signature of the function or body of the function:
``` julia
julia> myappend(v::Vector{T}, x::T) where {T} = [v..., x]
myappend (generic function with 1 method)

julia> myappend([1,2,3],4)
4-element Array{Int64,1}:
 1
 2
 3
 4

julia> myappend([1,2,3],2.5)
ERROR: MethodError: no method matching myappend(::Array{Int64,1}, ::Float64)
Closest candidates are:
  myappend(::Array{T,1}, !Matched::T) where T at none:1

julia> myappend([1.0,2.0,3.0],4.0)
4-element Array{Float64,1}:
 1.0
 2.0
 3.0
 4.0

julia> myappend([1.0,2.0,3.0],4)
ERROR: MethodError: no method matching myappend(::Array{Float64,1}, ::Int64)
Closest candidates are:
  myappend(::Array{T,1}, !Matched::T) where T at none:1
```

#### Function-like objects
- It is possible to make any arbitrary Julia object "callable" by adding methods to its type:
``` julia
julia> struct Polynomial{R}
           coeffs::Vector{R}
       end

julia> function (p::Polynomial)(x)
           v = p.coeffs[end]
           for i = (length(p.coeffs)-1):-1:1
               v = v*x + p.coeffs[i]
           end
           return v
       end

julia> (p::Polynomial)() = p(5)
```
``` julia
julia> p = Polynomial([1,10,100])
Polynomial{Int64}([1, 10, 100])

julia> p(3)
931

julia> p()
2551
```

## Constructors

#### Outer Constructor Methods
- Add functionality to a constructor by simply defining new methods:
``` julia
julia> Foo(x) = Foo(x,x)
Foo

julia> Foo(1)
Foo(1, 1)
```

#### Inner Constructor Methods
- An inner constructor method is like an outer constructor method, except for two differences:
  - It is declared inside the block of a type declaration, rather than outside of it like normal methods.
  - It has access to a special locally existent function called `new` that creates objects of the block's type.
``` julia
julia> struct OrderedPair
           x::Real
           y::Real
           OrderedPair(x,y) = x > y ? error("out of order") : new(x,y)
       end
```

#### Incomplete Initialization
- Allows the `new` function to be called with fewer than the number of fields that the type has:
``` julia
julia> mutable struct SelfReferential
           obj::SelfReferential
           SelfReferential() = (x = new(); x.obj = x)
       end
```

## Modules
| Import Command                  | What is brought into scope                                                      | Available for method extension              |
| ------------------------------- | ------------------------------------------------------------------------------- | ------------------------------------------- |
| `using MyModule`                | All `exported` names (`x` and `y`), `MyModule.x`, `MyModule.y` and `MyModule.p` | `MyModule.x`, `MyModule.y` and `MyModule.p` |
| `using MyModule: x, p`          | `x` and `p`                                                                     |                                             |
| `import MyModule`               | `MyModule.x`, `MyModule.y` and `MyModule.p`                                     | `MyModule.x`, `MyModule.y` and `MyModule.p` |
| `import MyModule.x, MyModule.p` | `x` and `p`                                                                     | `x` and `p`                                 |
| `import MyModule: x, p`         | `x` and `p`                                                                     | `x` and `p`                                 |
