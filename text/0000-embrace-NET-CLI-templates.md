- Start Date: 2021-05-03
- RFC PR: (leave this empty)
- Cake Issue: (leave this empty)
- Affected Areas: Cake-yeoman, pantry, Cake.Frosting.Template, Cake-vs, Cake-rider

## Summary
[summary]: #summary

This RFC is actually two-fold:
* Embrace .NET CLI templates over yeoman
* Harmonize existing templates and template-formats:
Start using .NET CLI templates for all project templates in Cake.

## Motivation
[motivation]: #motivation

Before starting to implement [project templates for Cake for Rider](https://github.com/cake-build/cake-rider/issues/63) I looked into exiting templates and found that we currently have:

* Supplied in cake-vs
  * Cake addin project in Visual Studio
  * Cake module project in Visual Studio
  * Cake addin unit test project in Visual Studio (empty and with samples)
* Supplied in Cake.Frosting.Template
  * Cake.Frosting project template
* Supplied in cake-yeoman
  * `cake.build` template
  * template for bootstrappers
  * `cake.config` template
  * Cake.Frosting project template
* Supplied in cake-vscode
  * `cake.build` template
  * download of a `cake.config` (template is not supplied, but always downloaded.)
  * downloads of bootstrappers (templates are not supplied, but always downloaded.)

Then there's the pantry repo that contains handlebar templates to generate cake templates.
(I've not checked which of the above were generated using pantry, so I'm not sure if that's up-to-date or not.)

While Rider (since version 2017.3) will pick up installed .NET CLI project templates directly and
offer them as new project templates, Visual Studio supports this as
an experimental feature, starting from [Visual Studio 16.8 Preview 2](https://devblogs.microsoft.com/dotnet/net-cli-templates-in-visual-studio/). 

So, for current versions of Rider and coming versions of VS we could maintain only one version of (project) templates instead of maintaining multiple.

## Detailed design
[detailed-design]: #detailed-design

* Create a new Project Cake.Templates (Or Cake-Templates, or something else..)
* Move the current Cake.Frosting.Template into Cake.Templates
  * Deprecate Cake.Frosting.Template
* Create all currently in-use project and item templates as .NET CLI templates
  * Deprecate cake-yeoman and probably pantry

(When above stage has been released the first part of this proposal (*Embrace .NET CLI templates*) will be reached, and [custom templates for Cake for Rider](https://github.com/cake-build/cake-rider/issues/63) would not be needed.)

* Modify the commands in *Cake for VS Code* to utilize the .NET CLI templates instead of providing/downloading templates. 
* When .NET CLI project templates in VS eventually leave the experimental status, drop the custom templates from *Cake for VS* and use only the .NET CLI templates.
* When/if .NET CLI item templates become available in VS and/or Rider, drop the respective item templates and use only the .NET CLI templates.

(Both of the above will be breaking changes to the older versions of VS/Rider)

## How we teach this
[how-we-teach-this]: #how-we-teach-this

Communications on the new .NET CLI templates: Those would be new additions to our already growing set of tools. No special communication aside from "normal" communications is needed.

Communication on the deprecation of Cake.Frosting.Template: Since the template will not be deprecated but only be moved as-is into another `nupkg`, I feel nothing "special" communication aside from "normal" communications is needed.

Communication on the deprecation of cake-yeoman and pantry: Not sure how usage of those repositories currently is. Could be as simple as "We're dropping support of yeoman and start supporting .NET CLI templates"

Communication on the deprecation of templates for older VS versions: I would suggest (when the day comes that .NET CLI project templates in VS are no longer experimental) we stop maintaining the old `vsix` template, but leave them in the `vsix`. This way they will be available and will continue to be of use for users of older VS versions.

Depending on the state of the templates in Rider, no communication could be needed at all.

## Drawbacks
[drawbacks]: #drawbacks

### Drawbacks to embracing .NET CLI templates

Compared to the possibilities of yeoman templates, .NET CLI templates are rather "crude": The yeoman generator can create all item-templates in one go, as well as use the newest sources from a GitHub repository.

These features we would (probably, not 100% sure) lose.

### Drawbacks to using .NET CLI templates as the single source of templates in VS/Rider

* The feature in Visual Studio is currently experimental - so it has to activated before .NET CLI project templates will start showing up.
* Neither VS, nor Rider currently show item templates. (Though VS *should* show them, I could not make it work.)
* While VS supports having the same template in a `vsix` installer (like we do now in *Cake for VS*) and in .NET CLI (seemingly only one will be shown, if `id` is the same) having both for backwards compatibility also means maintaining both.

## Rationale and alternatives
[rationale-and-alternatives]: #rationale-and-alternatives

Firstly, .NET CLI templates are "the way" of the .NET world, so we should supply all required project and item templates as .NET CLI templates.

Secondly, maintaining one common set of templates is more "cost-effective" than maintaining multiple.

The alternative would probably be to check/update `pantry` (last commit was in 2018) then keep maintaining different sets of templates (Rider, VS and Frosting) as we currently do.

## Unresolved questions
[unresolved-questions]: #unresolved-questions

* Not sure when the feature in VS will leave experimental status
* Not sure if showing item templates in VS is not supported or simply "currently broken"
* Not sure if Rider will start supporting item templates
* could .NET CLI templates provide templates to be downloaded, as cake-yeoman currently does.