# Hamlet

> �To be, or not to be, that is the question�

Hamlet is a small .NET library that provides a simple option type, as well as some extension methods
for use with Linq.

An option type is useful to represent the presence or absence of a value. In C#, `null` is often used
for that purpose, but it has some issues:
- it's a frequent source of bugs, because developers often forget to check for null
- `null` might be a valid value in some scenarios, so it doesn't really mean the same thing as an
absence of value; "I have a value that is null" is not the same as "I have no value".

Most functional languages have an option type, often called `Option` (as in F#) or `Maybe` (as in
Haskell). C# could also benefit from such a type, so here it is!

## Usage

### Creating an option

```csharp
using Hamlet;

// An optional int with the value 42
Option<int> a = Option.Some(42);
// or just
Option<int> a = 42;

// An optional int with no value
Option<int> b = Option.None();
// or
var b = Option.None<int>();
// or just
Option<int> b = default;
```

### Checking if a value is present and getting the value

```csharp
Option<int> option = ...

// Explicit check
if (option.IsSome)
{
    return option.Value;
}
else
{
    return -1;
}

// Using GetValueOrDefault
return option.GetValueOrDefault(-1);

// Using TryGetValue
return option.TryGetValue(out var value) ? value : -1;
```

### Usage with Linq

Linq can be used to work with the value that might be contained in an option, by using projections or filters.

In the following example, `result` will be `Some(option.Value + 1)` if `option` has a value, and `None` otherwise:

```csharp
using Hamlet;
using System.Linq;

Option<int> option = ...
Option<int> result =
    from value in option
    select value + 1;

```

In the following example, `result` will be `Some(option.Value)` if `option` has a value greater than 0, and
`None` otherwise:

```csharp
using Hamlet;
using System.Linq;

Option<int> option = ...
Option<int> result =
    from value in option
    where value > 0
    select value;

```

Values from multiple options can also be combined. In the following example, `result` will be
`Some(optA.Value + optB.Value)` if both `optA` and `optB` hava a value, and `optB`'s value is greater than 0:

```csharp
using Hamlet;
using System.Linq;

Option<int> optA = ...
Option<int> optB = ...
Option<int> result =
    from a in optA
    from b in optB
    where b > 0
    select a + b;
```
