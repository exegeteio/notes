# C#

## VS Code

When VS Code asks to create "missing" files, DO IT!  This creates the `.vscode` folder and enabled
OmniSharp and Intellisense to start working.  Prior to this being in place, nothing worked.  I
struggled with VS Code for a few hours to get OmniSharp working, because I either did not see, or
did not understand this popup.

## Static Imports

Imports methods from a module into the current scope.

```C#
using static System.Console;
WriteLine("Whatever");
```

## Console

```C#
WriteLine();
string line = ReadLine();
ConsoleKeyInfo key = ReadKey();
Write();
```

## String Interpolation

Interp. can be formatted with:  `{index,alignment:formatString}`.

For formats see: 
[https://docs.microsoft.com/en-us/dotnet/standard/base-types/formatting-types](https://docs.microsoft.com/en-us/dotnet/standard/base-types/formatting-types)

```C#
$"string with math:  {i * 2}";
$"string with 5 left-padding:  {aStr,5}";
$"string with 5 right-padding:  {aStr,-5}";
```

## Type matching with `if` 

Note that this will throw an error if `i` is a `string` instead or `object`.

```C#
object i = 1;
if (i is int j) {
  // j == i
}
```

## Switch / Case

```C#
switch (number)
{
  case 1:
    WriteLine("One");
    break; // Required.
  case 2:
    WriteLine("Two");
    break;
  case 3:
  case 4:
    WriteLine("3 || 4")
    goto case 1; // WHAAAT?!  Goto feels dirty.
  case Int64 j:
    // j is now the value, only get here if `number` is an Int64.
  default: // Always evaluated last, regardless of placement.
    // Do stuff.
    break;
}
```

Alternate syntax in C# 8:

```C#
object i = 123;
var message = i switch
{
    int j => $"{j} and other text",
    null => "j is null",
    _ => "Default" // This is an underscore!
};
WriteLine(message);
```

## Functional Syntax

Different implementation for defining functions.  Book says more of an Functiona;/F# style?

```C#
static int Fib(int term) =>
  term switch {
    1 => 0,
    2 => 1,
    _ => Fib(term - 1) + Fib(term - 2)
  };
```

Interesting style.  Seems to communicate a lot of info in a short statement.

## Iterating

```C#
while (i < 10) {i++;}
do {i++;} while (i < 10);
for (int i = 1; i < 100; i++) {}
string[] names = {"Adam", "Barry", "Charlie" };
foreach (string name in names) {}
```

## Casting

Converters for System types can be found in `System.Convert`.  Also includes `ToBase64String` and
`ToBase64String`.

C# does that weird thing where rounding is different for evan and odd numbers, unless you
explicitely tell it how to round with `Math.Round` or similar.  Called "Banker's Rounding".

```C#
int i = (int) aDouble;
int j = (int) 10.5; // j == 10
Math.Round(
  value: 10.5,
  digits: 0,
  mode: MidpointRounding.AwayFromZero
); // Output is 11.
```

Int can try to parse strings with a special method:

```C#
Write("How many eggs are there? ");
string input = ReadLine();

int count;
if (int.TryParse(input, out count)) {
    WriteLine($"There are {count} eggs.");
} else {
    WriteLine($"I could not parse your input:  {input}");
}
// count == either 0 or the value input.
```

## Exceptions

```C#
try {}
catch (FormatException) {}
catch (OtherException ex) {} // ex is the exception object.
catch (Exception ex) { // All exceptions!
  WriteLine($"{ex.GetType()} says {ex.Message}");
}
```


