---
layout: post
title: "How to Get Started Contributing to Open Source"
date: 2017-03-17 03:06:44 +0530
categories: foss,github,contribute,beginner
---

Early in college I put [open source][] software projects on a pedestal. I wanted to contribute, mainly for bragging
rights and the thrill that comes with it. I didn't feel I was qualified enough because almost all of the issues I
encountered in products that I used regularly were well above my expertise and experience. There was also no way known
to me at that time for finding easy issues apart from Mozilla's ["Good First Bugs"][]. I learnt about amazing projects
like [Your First PR][] and [Up For Grabs][] much later. My method of finding simple issues was much more involved. I
used to scourge [trending projects on GitHub][github trending] for issues and bookmark them for review. You don't have
to do that though, you can use the amazing tools I've mentioned above thanks to [Scott Hanselman][], [Kent C. Dodd][]
and [Charlotte Spencer][Your First PR]. This is how I started and where I am now. I hope this helps other people who
feel overwhelmed or think that the community is not welcoming or just somebody who wants to be reminded about why they
started contributing.

## Getting a feel for Open Source

Before you can dive in deep and start submitting non-trivial patches you need to get a feel for the process. I started
by opening issues, [lots of them][my issues], in projects that I used myself as a way to get other people to scratch my
itch. You can do the same. Make sure to follow the ensuing discussion and try to participate in it. Once somebody starts
writing a patch for your issue make sure to also follow the patch and learn how the other person did things. This will
help you in gaining both some general technical knowledge about the project and also introduce you to a first hand
demonstration of the code review and patch submission process in the project.

## Small and trivial changes

Sometime after opening issues and following them I had gained enough information about the process to start submitting
trivial changes. I realised that since the projects were open-source I could scratch my itches myself and felt the
empowerment that being literate (in this case, computer literate) brings. It was my [Eureka moment][].

I started by contributing a new challenge and solution to [an interactive coding challenge][interactive coding
challenges] (thanks a lot [Donne Martin][]). They were really small and minimal changes that were low-hanging fruit.
These kinds of changes are good to get a feel because they are low risk and are not blocking the project from moving
forward. A good way to find such bugs is to find issues labelled `backlog` or `help needed`. These will help you to get
familiar with the tools of the trade and build up confidence. Do not shy away from asking questions whenever you feel
inquisitive or feel stuck on a problem. You can even ask other people in their pull requests as to why and how they did
something.

I then proceeded to open a lot of PRs, a lot of which were [rejected][] while some were [accepted][]. There were some
PRs which some would call wasteful or stupid and [this PR][neofetch pr] to [Neofetch][] is a good example of that. I
literally added ASCII art to a PR. There's an invaluable thing I learnt from the rejection of my PRs:

> The most common reason for a PR to get rejected is that you did not open an issue and ask for feedback whether the
> feature aligned with the project's goals or ideas.

I've never had a pull request rejected if there was some discussion before starting work and sending one. What usually
happens in such a case is that the maintainer usually drops hints or reading material to help you fix the problem
yourself. A very good example is [this][first olw pr] and [this][first vscode pr].

## Picking a project

I don't think I paid much attention to what the project was in the early days. I just wanted to get a shiny badge that
said I had contributed code to someone somewhere. But that was a mistake. That approach is not sustainable because to
make good contributions and improve with time you need context. Context means a familiarity with the project, both it's
code and the development process it follows. So here is some criteria to chose a project:

- The size of the codebase: The less code the easier it is to understand how all of it's pieces fit together.
- The language of the project: If the project uses a language you are not familiar with it'll be *comparatively*
  difficult to get a good understand of the project.
- Do you use the project: If you are a user of the project you know that any changes you make will affect you. There's
  also a higher chance of you being able to find out issues yourself instead of relying on bug reports.

## Read the documentation

After selecting a project, the first step is to go read *ANY* and *ALL* documentation they have. This is beneficial for
two purposes:

- You can learn about how the project works, how to contribute, the coding style and how code is organised etc.
- You can also find gaps in the documentation to fill. This is a very easy and much appreciated way to contribute to a
  project. Documentation is one of the most neglected parts of a project and fixing it pays it's dividends by helping
  future contributors.

When going through the documentation keep a focus on finding out the tools they use, the way the project is organised,
the process that they follow and the core ideas and goals of the project.

For gathering information about the social context:

- If the project has any documentation for new contributors read it. Most projects on GitHub/GitLab include a CONTRIBUTING.md
  file that deals with it. Larger projects generally have a dedicated wiki like [Mozilla][mozilla developer wiki].
- Subscribe to the project's issues list (see [watching projects on GitHub][]) and [commit feed][mozilla-central commit feed].
- If they have a Gitter or IRC channel, join it and follow the conversation and go read up on something interesting
  they mention.
- If they have a mailing list subscribe to it. Larger projects generally tend to use mailing lists and IRC for
  communication.

For gathering information about the technical context:

- Read the source code if the project is small enough. You will definitely learn something or even find some places with
  scope for improvement.
- Read the test cases. Tests are also an ignored part of most projects and a great place to start contributing and
  learning a very important skill.
- Follow some other people's ongoing contributions to gain an idea of what it's like to contribute patches and what the
  things to take care of while doing so.
- Follow the main contributors' blogs and/or Twitter. One of the best examples of this is [Mike Conley's][] weekly
  live-coding session called [The Joy of Coding][]. It's a great way to learn how people generally work and you can
  pick up a lot of debugging tips and tricks along the way.

## Set up a local development environment

This is one of the most difficult tasks if you have never built a large project yourself or built software from source
before. Most projects generally have a README file that will give you enough information to run the project locally.
Some larger projects have a dedicated category in their wiki that deals with this and is often called "Developer
documentation". Mozilla has a very comprehensive set of "getting started" documentation that deals with each step of the
process:

- Getting the source code. Most projects use some sort of version control tool like Git or Mercurial. Go install those
  tools and "clone" the project.
- Learn about the various build configurations if there are any. Most project have different build settings that let you
  get a debug build so that you can attatch a debugger and experiment or an optimized released build which is what you
  normally run.
- Build the project. It can be as simple as running `make` or can involve using the project's own build system.
- Run the tests and any linters to get an idea of how the entire process works.

## Finding your first issue

Your goal should be to find something that is small and easy. Documentation and tests are great examples. Stylistic
fixes also fit the criteria but are discouraged as patches on themselves since they tend to pollute the project's
history. You can also look through the project's issue tracker for bugs labelled "need help" or "beginners" or something
similar.

## Working on your first issue

Learn to gain information about the code from the code itself.

- `git blame/hg blame`. There's a lot of information in the version control system to help you learn. You can look at
  the blame information and the commit messages to find out why something was written the way it is written. It also
  helps you to identify the people who have worked on that file before and you can reach out to them for more
  information.
- Pull request history. There is a lot of discussion and information available on the issue that was tracking a bug and
  also on the actual pull request. They can come in handy for gaining information.

Now you can move on to writing the actual patch.

## Writing a patch

There's a process I've developed for myself where I first try to hack together "proof of concept" to demonstrate that
the task at hand can be done and how it looks like and behaves. It also helps to think about the edge cases.

Now you can start fresh and write your most elegant code. The code should be clear, avoid magic numbers and should be
self-documenting. Your code should be such that the next person to come along should be able to intuitively understand
what the code is doing.

### Conform to the styleguide

If the project does have a styleguide, conform to it. There are generally some commit-hooks that run linters before
allowing you to commit code to ensure that all code meets guidelines. If there's no formal style guide or linter, keep
your style consistent with what is already written in the file. The entire project should ideally look like it was
written by a single person. Attention should be paid to variable naming conventions, spacing and layout and code
organisation.

### Write tests

If you are submitting a patch relating to some code that is not covered by tests, write some tests for it (in a seperate
commit generally). This benefits both you and the project because then the person reviewing your code doesn't have to
pull your changes, build the project with the changes and manually test everything. They can simply look at the tests
to determine if the code is correct and will have intended effects. This will make their work easier and you will gain
some points too.

## The commit message

In open source you cannot assume that you will be working on the same project or be available for contact a year later.
This means that the next person who has to work on that code should have all the required information without ever
needing to contact you. This information is mostly passed down through self-documenting concise code, documentation and
commit messages.

A good commit should satisfy the following:

- Should be wrapped within 72 characters.
- Should have a single line message written in imperative language giving a summary of the change. Mozilla uses a
  convention of `Bug 1224225: Test for punycode/unicode in CSP source matching code r=ckerscb`. It includes the bug
  number so that you can go and see exactly what was fixed and the related discussion. The summary is concise and very
  specific and written in imperative. The line ends with a convention of `rxreviewer` where `x` is one of `?`, `=` or
  `+` which mean requested review, accepted review and changes suggested respectively. It also provides information
  about the person who reviewed the patch so that you know who the expert on that part of code is.
- The body of the commit message in a few lines should explain things in more detail and give some background about the
  issue and maybe link to related issues or discussion.
- The body can also be several paragraphs and should explain the "why" and "what" of your patch instead of "how". It
  means that the patch doesn't need to focus on the implementation detail. It should focus on why the approach was
  chosen among other alternatives and why a certain decision was made. If the fix is doing something complicated you may
  give an overview of what it is doing.

A very good read regarding commit messages can be had from the creator of Git itself [Linus Torvalds][]
[here][torvalds good commit message].

## Opening a pull request

This is your chance to make a good first impression. You should go in expecting the standards to be very high. Expect
your choices to be challenged, nits to be picked and suggestions to come. Be ready to defend your choices with informed
decisions and be willing to incorporate feedback. Almost always a pull request has to go through several iterations
before it is accepted. Remember that a critical feedback is not meant to attack you, it's so you can improve your code
and benefit yourself and the project. Even if your PR is rejected this is not a failure. It's rather an opportunity to
improve and internalize the feedback and try again!

## Issue->Patch->Accepted...PROFIT?

If you follow this advice and the advice of the people running the project you will certainly get a PR merged within no
time. From personal experience I can tell you that it is illogically thrilling knowing that your code was merged and
other people will be using it and you've improved their lives.

The process above works like a feedback loop and the more you do it the more familiar you get with the project and rise
in the ranks and it becomes much more easier for you to write your next patch. Over time you will begin to "own" some
parts of the code in the sense that you know how it all works and maybe you even wrote most of it. When that happens
remember to give back to people who are struggling to contribute just like you were once.

There'll be another accompanying blog post for project owners and maintainers on how to attract and *retain* good
quality contributors to your project. You can find the post [here][post for maintainers].

[open source]: https://opensource.org/
["Good First Bugs"]: https://bugzilla.mozilla.org/buglist.cgi?quicksearch=sw:"[good%20first%20bug]"&limit=0
[Your First PR]: https://yourfirstpr.github.io/
[Up For Grabs]: https://up-for-grabs.net/
[First Timers Only]: https://www.firsttimersonly.com/
[github trending]: https://github.com/trending
[Scott Hanselman]: https://www.hanselman.com/blog/BringKindnessBackToOpenSource.aspx
[Kent C. Dodd]: https://medium.com/@kentcdodds/first-timers-only-78281ea47455#.k8onekqla
[my issues]: https://github.com/issues?q=is%3Aissue+author%3Ahashhar+sort%3Acreated-asc+is%3Apublic
[Eureka moment]: https://en.wikipedia.org/wiki/Eureka_effect
[interactive coding challenges]: https://github.com/donnemartin/interactive-coding-challenges
[Donne Martin]: http://donnemartin.com/
[first pr]: https://github.com/donnemartin/interactive-coding-challenges/pull/39
[rejected]: https://github.com/pulls?utf8=%E2%9C%93&q=is%3Apr+author%3Ahashhar+sort%3Acreated-asc+is%3Aunmerged
[accepted]: https://github.com/pulls?utf8=%E2%9C%93&q=is%3Apr+author%3Ahashhar+sort%3Acreated-asc+is%3Amerged
[first olw pr]: https://github.com/OpenLiveWriter/OpenLiveWriter.Github.io/pull/32
[neofetch pr]: https://github.com/dylanaraps/neofetch/pull/246
[Neofetch]: https://github.com/dylanaraps/neofetch
[first vscode pr]: https://github.com/Microsoft/vscode/pull/7029
[mozilla developer wiki]: https://developer.mozilla.org/en-US/docs/Mozilla/Developer_guide
[watching projects on GitHub]: https://help.github.com/articles/watching-repositories/
[mozilla-central commit feed]: https://hg.mozilla.org/
[Mike Conley's]: https://twitter.com/mike_conley
[The Joy of Coding]: https://air.mozilla.org/channels/livehacking/
[Linus Torvalds]: https://en.wikipedia.org/wiki/Linus_Torvalds
[torvalds good commit message]: https://github.com/torvalds/subsurface-for-dirk/blob/0f58510ce0244513521296b75281fcc32f72a931/README#L92-L119
[post for maintainers]: /making-your-open-source-project-contributor-friendly
