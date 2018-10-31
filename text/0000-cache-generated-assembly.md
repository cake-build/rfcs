- Start Date: 2018-10-31
- RFC PR: (leave this empty)
- Cake Issue: (leave this empty)
- Affected Areas: Cake, Cake.Core, Cake.Scripting

## Summary
[summary]: #summary

Include in Cake the ability to cache the generated assembly, so that this
can be used when the underlying script hasn't changed.

## Motivation
[motivation]: #motivation

While for most people, the speed in which Cake executes is perfectly fine,
however, there have been those that suggest that the compilation time is a
burden that they don't want to spend.  Especially in situations where the build
script itself isn't changing, but lots of builds are happening in succession.

Caching the generated assembly so that it can be used over and over again, without
the need to compile each time, will save some workloads loads of time.

## Detailed design
[detailed-design]: #detailed-design

### Changes to configuration

A new entry will be added into the cake.config file to control the location of
the cached assemblies.  This should default to a `cache` folder within the Cake
Tools folder.  The end user can then control this location in the normal way.

### Changes to Cake Arguments

Two new arguments will be added to the Cake Client.

* Cache - If set true the compiled script will be cached
* Recompile - If set true it will force a recompile of the script even if a valid cached output exists.

These will be added to the CakeOptions class, and set based on the values which
are parsed in the ArgumentParser.

### Changes to Cake.Scripting

The majority of the work for this RFC will be done in the RoslynScriptSession class,
although a change will be needed to the RoslynScriptEngine to pass in the CakeConfiguration
instance, to inspect the configured values.

Within RoslynScriptSession, a check will be made to see whether caching of the script
is enabled.  If it is, and a cached assembly is already present, and we are not being
asked to force the re-compilation of the script, then we should go ahead and run that
assembly, rather than compiling the script.  Within this section, we should also check
the timestamps of both the cached assembly and the script.  If the script is newer than
the cached version should not be used, and instead, re-generated.

If we are being asked to cache the script, and the assembly doesn't exist, we should
generated the assembly and then execute it.  Otherwise, run the script as we currently
do.

## How we teach this
[how-we-teach-this]: #how-we-teach-this

[Documentation](https://github.com/cake-build/website/blob/develop/input/docs/cli/usage.md)
will need to be updated to reflect the new arguments that this adds to Cake.

The Help system which is displayed on the Command Line will also need to be
updated to include the new arguments.

## Drawbacks
[drawbacks]: #drawbacks

This will add some complexity to Cake, as I don't think that this is something
that will be enabled by default, but rather something that people will have to
opt into.  That in itself will cause some initial confusion, and likely some
misunderstanding.

## Rationale and alternatives
[rationale-and-alternatives]: #rationale-and-alternatives

We could keep things as they are.

## Unresolved questions
[unresolved-questions]: #unresolved-questions

Don't think there are any.
