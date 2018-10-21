- Start Date: 2018-09-07
- RFC PR: [#2334](https://github.com/cake-build/cake/pull/2334)
- Cake Issue: [#2333](https://github.com/cake-build/cake/issues/2333)
- Affected Areas: Cake, Cake.Core

## Summary
[summary]: #summary

Rewrite/Refactor the Cake Client (`Cake.exe`/`Cake.dll`). 
Command line parsing should be adapted to better support
functionality that is expected of a modern cross platform application such as 
multiple arguments with the same name.

## Motivation
[motivation]: #motivation

While the current Cake client have served us well, it's become an over engineered
mess in many places. We are extremely reliant on using deprecated Autofac 
behaviors such as updating existing containers to support Cake modules. This often
makes it difficult to reason about the code when fixing bugs or adding new features.

It would make it easier to use Cake if we introduced proper commands and
subcommands like most people are used to from applications such as `git` and `dotnet`.
Of course we would need to be backwards compatible with how Cake works today
(without commands and sub commands) but this functionality could be phased out
before a 1.0 release of Cake. The `build` command should however be the default 
command.

```
# Example of command
C:> ./cake.exe bootstrap
```

There has also been a lot of problems with how Cake parses command line arguments,
and this might be a good time to fix this and make it follow established conventions.

```
C:> ./cake.exe --foo=bar --foo:bar --foo bar --bar
```

Adding commands and sub commands to Cake would make it easier to implement new
functionality like manipulating Cake's configuration.

```
C:> ./cake.exe config set nuget_source https://mycustomnugetsource
C:> ./cake.exe config get nuget_source
https://mycustomnugetsource
```

## Detailed design
[detailed-design]: #detailed-design

Current arguments should be backwards compatible. In addition to that we will add new
CLI commands: `build`, `bootstrap`.

### Changes to Cake

* Introduce [Spectre.Cli](https://github.com/spectresystems/spectre.cli) library for command handling.  
  You can read more about that here: https://www.patriksvensson.se/2018/04/an-introduction-to-spectre-cli

### Changes to Cake.Core

* Change `ICakeArguments`
  - `string GetArgument(string name)` becomes `IEnumerable<string> GetArguments(string name)`
  
### Changes to Cake.Common

* New alias: `IEnumerable<T> Arguments<T>(string name)`
* Modified alias: `T Argument<T>(string name)` should return the first instance or `null`.

## How we teach this
[how-we-teach-this]: #how-we-teach-this

There shouldn't be a need to teach how this works since it should be backwards 
compatible, but we will need to update the [documentation](https://github.com/cake-build/website/blob/develop/input/docs/cli/usage.md).

## Drawbacks
[drawbacks]: #drawbacks

1. Like every other time we refactor or rewrite something, there is always the chance
of us introducing bugs.
2. We would take on a third-party dependency for
argument parsing and command parsing (`Spectre.Cli`). If this is a big problem we
could probably mitigating it by incorporating the source code into the Cake
repository, or add it as a submodule or subtree to the Cake project.
3. While we want to keep argument parsing 100% compatible, there is one place
   where we can't and that's where we implictly treat short options like `-bar` as
   a long option (`--bar`). Cake currently see these two options as the same ones, but
   when using a "real" argument parser, they will (and should) be treated as `-b -a -r`. 
   All our guidelines explicitly points out that arguments are passed in it's long form 
   but we can't guarantee that someone discovered that it doesn't matter and uses the short
   form for some reason. These scripts will break but the fix is simple.

## Rationale and alternatives
[rationale-and-alternatives]: #rationale-and-alternatives

We could keep things as they are.
