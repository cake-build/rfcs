# Cake RFCs

Many changes, including bug fixes and documentation improvements can be
implemented and reviewed via the normal GitHub pull request workflow.

Some changes though are "substantial", and we ask that these be put through a
bit of a design process and produce a consensus among the Cake community and
the maintainance team.

The "RFC" (request for comments) process is intended to provide a consistent
and controlled path for new features to enter the language and standard
libraries, so that all stakeholders can be confident about the direction the
language is evolving in.

## What the process is
[What the process is]: #what-the-process-is

In short, to get a major feature added to Cake, one must first get the RFC
merged into the RFC repository as a markdown file. At that point the RFC is
"active" and may be implemented with the goal of eventual inclusion into Cake.

  - Fork the RFC repo [RFC repository]
  - Copy `templates/feature-template.md` to `text/0000-my-feature.md` 
    (where "my-feature" is descriptive. don't assign an RFC number yet).
  - Fill in the RFC. Put care into the details: RFCs that do not present
    convincing motivation, demonstrate understanding of the impact of the
    design, or are disingenuous about the drawbacks or alternatives tend to be
    poorly-received.
  - Submit a pull request. As a pull request the RFC will receive design
    feedback from the larger community, and the author should be prepared to
    revise it in response.
  - Build consensus and integrate feedback. RFCs that have broad support are
    much more likely to make progress than those that don't receive any
    comments. Feel free to reach out to the RFC assignee in particular to get
    help identifying stakeholders and obstacles.
  - The maintenance will discuss the RFC pull request, as much as possible in the
    comment thread of the pull request itself. Offline discussion will be
    summarized on the pull request comment thread.
  - RFCs rarely go through this process unchanged, especially as alternatives
    and drawbacks are shown. You can make edits, big and small, to the RFC to
    clarify or change the design, but make changes as new commits to the pull
    request, and leave a comment on the pull request explaining your changes.
    Specifically, do not squash or rebase commits after they are visible on the
    pull request.
  - At some point, a member of the maintenance team will propose a "motion for
    final comment period" (FCP), along with a *disposition* for the RFC 
    (merge, close, or postpone).
    - This step is taken when enough of the tradeoffs have been discussed that
    the maintaince team is in a position to make a decision. That does not require
    consensus amongst all participants in the RFC thread (which is usually
    impossible). However, the argument supporting the disposition on the RFC
    needs to have already been clearly articulated, and there should not be a
    strong consensus *against* that position outside of the maintenance team. maintenance team members use their best judgment in taking this step, and the FCP itself ensures there is ample time and notification for stakeholders to push back if it is made prematurely.
    - Before actually entering FCP, *all* members of the maintenance team must sign off; this is often the point at which many maintenance team members first review the RFC in full depth.
  - In most cases, the FCP period is quiet, and the RFC is either merged or
    closed. However, sometimes substantial new arguments or ideas are raised,
    the FCP is canceled, and the RFC goes back into development mode.

## The RFC life-cycle

Once an RFC becomes active, then authors may implement it and submit the
feature as a pull request to the Cake repo. Becoming 'active' is not a rubber
stamp, and in particular still does not mean the feature will ultimately
be merged; it does mean that the core team has agreed to it in principle
and are amenable to merging it.

Furthermore, the fact that a given RFC has been accepted and is
'active' implies nothing about what priority is assigned to its
implementation, nor whether anybody is currently working on it.

Modifications to active RFCs should be done in followup PRs.

## Reviewing RFCs

Cake is an open source project which is maintained in the maintainers free time. 
There is no SLA when it comes to reviewing RFCs.

## Implementing an RFC

The author of an RFC is not obligated to implement it. Of course, the
RFC author (like any other developer) is welcome to post an
implementation for review after the RFC has been accepted.

If you are interested in working on the implementation for an 'active'
RFC, but cannot determine if someone else is already working on it,
feel free to ask (e.g. by leaving a comment on the associated issue).

**Cake's RFC process owes its inspiration to the [Yarn RFC process], [Rust RFC process], [React RFC process] and [Ember RFC process]**

[Yarn RFC process]: https://github.com/yarnpkg/rfcs
[Rust RFC process]: https://github.com/rust-lang/rfcs
[React RFC process]: https://raw.githubusercontent.com/reactjs/rfcs
[Ember RFC process]: https://github.com/emberjs/rfcs