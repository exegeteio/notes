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

## Logging

`Trace` is used in prod and dev, `Debug` is used in dev:

```C#
Trace.Listeners.Add(new TextWriterTraceListener(File.CreateText("log.txt")));
Trace.AutoFlush = true; // Required to flush log on write.
Trace.WriteLine("It did the do in prod!");
Debug.WriteLine("It did more do in dev!");
```

## Swap Configuration

Can swap to the "Release" configuration with the `dotnet run` command:

```sh
$ dotnet run --configuration Release
```

## Unit Testing

Unit tests are generated with `dotnet new xunit --name <name>UnitTests`.  The name does not matter.

In the `.csproj` file, create an ItemGroup to reference what you're testing.  Even on MacOS `\` is
used.

```xml
<ItemGroup>
  <ProjectReference Include="..\CalculatorLib\CalculatorLib.csproj" />
</ItemGroup>
```

Run your tests with:

```sh
$ dotnet test
```

## Classes

### Shared Member

A shared member is a `static` attribute on a class which makes the value available as a class
variable.

```c#
public class BankAccount { public static decimal InterestRate; }
// ...
Console.WriteLine(BankAccount.InterestRate); // This works.
var account = new BankAccount();
Console.Write?Line(account.InterestRate); // Will not build!
```

### Constant Members 

`const` values also cannot be accessed by via an instance, but can be used as a local property
inside a class.

**Warning:** At compile time `const` variables are replaced with their literal value.  This means that
a library which uses your `const`, will need to be recompiled to pick up the new constant.  The book
I'm reading suggest that constants should be avoided because of this.

```c#
public class Person {
  public const string Species = "Homo Sapien";
  public override string ToString() {
    return $"This person is a {Species}"; // Works as expected.
  }
}
// ...
Console.WriteLine(Person.Species); // This works.
var bob = new Person();
Console.WriteLine(bob.Species); // This will not compile.
```

### Readonly Members

Similar to `const`, but does not get replaced at compile time.  Can also be set by Constructors.

```c#
public readonly string Planet = "Earth";
```

### Default Values

Not sure what use this is, but you can use the keyword `default` to define a value as the default
value for its type.

`DateTime` objects initialize at midnight of year one, month one, day one.

```c#
public class Thing {
  public DateTime When;
  public Thing() {
    When = default;
  }
}
```

### Optional Params

Optional method parameters are defined with an `=` in the defenition, and must come last.

```c#
public void SayHello(greeting, name = "World!") {};
```

Optional parameters can be specified positionally or with a `:` in the message defenition.

```c#
SayHello("Howdy", "Pardner");
SayHello("Hola", name: "Mundo");
```

### Passing Pointers (References) 

References to objects can be defined in the method defenition and when calling the method.

```c#
public void SayHello(ref greeting, name = "World") {
  greeting = "Hi!"; // Changes the value outside the method!
  // ...
};

var greeting = "Hola";
SayHello(ref greeting);
Console.WriteLine(greeting); // "Hi!"
```

Can also pass a reference to a single `out` variable as well.

```c#
public void SayHello(greeting, out string message) {
  message = $"{greeting}, World!";
  // ...
};

var greeting = "Hola";
string mess1;

SayHello(greeting, out mess1);
Console.WriteLine(mess1); // "Hola, World!"

// C#7 shortcut to define mess2 inline.
SayHello("Howdy", out string mess2);
Console.WriteLine(mess2); // "Howdy, World!"
```

### Init-only Properties

Properties can be defined such that they are only assignable when being
initialized.

```c#
public string FirstName { get; init; }
```
