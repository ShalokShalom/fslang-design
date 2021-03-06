
# F# RFC FS-1003 - nameof Operator

The design suggestion [nameof operator](https://github.com/fsharp/fslang-suggestions/issues/252) has been marked "approved in principle".
This RFC covers the detailed proposal for this suggestion.

[Discussion thread](https://github.com/fsharp/FSharpLangDesign/issues/48)

* [x] Approved in principle
* [ ] Details: [under discussion](https://github.com/fsharp/FSharpLangDesign/issues/48)
* [ ] Implementation: [In progress](https://github.com/Microsoft/visualfsharp/pull/2290)

### Introduction

It's often useful to obtain the simple (unqualified) string name of a variable, type, or member.

Today users end up writing something like this:

```fsharp
let add x y =
    if x < 0 then raise (ArgumentOutOfRangeException("x"))
    x + y
```

It would be useful to have this to extract the name of expressions, like:

```fsharp
let add x y =
    if x < 0 then raise (ArgumentOutOfRangeException(nameof(x)))
    x + y
```

### Naming 

The name of the operator is `nameof`

### Performance considerations

This substitution should be done at compile time, no performance impact expected

### Testing considerations

The standard range of testing  considerations for F# library functions apply.

Additional considerations:

- handle cases where `nameof` already resolves to something user-defined, to avoid being a breaking change
- when used like a function, `let f = nameof ;; f x`
- when used with pipe operator, `x |> nameof`
- works in quotations
- works with provided symbol names, i.e. generated by type providers
- works with method parameters
- works with local variables
- works with local (nested) functions
- works with local curried functions
- works with local tupled functions
- can get name from inside a local function (needs to be let rec)
- can get member names
- can get static member names
- can get static property names
- can get names that quoted in ````
- can get names of operators like `+`, `|>`, `typeof`, `nameof`, ...
- can be used in pattern matching
- can be used with generic functions/types
- works in attributes
- works with namespaces, `nameof System.Diagnostics`
- works with types, `nameof System.String`
- works with modules, `nameof List`
- works with type arguments, `let f<'t> (x : 't) = nameof 't`
