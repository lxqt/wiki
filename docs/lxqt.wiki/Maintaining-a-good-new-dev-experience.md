LXQt is short on manpower. For that reason, our best investment is into making it as easy as possible for new developers to contribute to the project and get familiar enough with it to become a regular contributor.

For a high quality new dev experience, this is what we have to maintain:

1. A clear, crisp and understandable codebase with immediately-available documentation
2. Communication channels with people familiar with the codebase
3. Clear goals: What we want, our priorities, etc.
4. A "Beginner's guide" with pointers on where to begin

Let's review those one by one. Keep in mind, exceptions can always be made with a good reason. 


*Maintaining a clear, understandable codebase*:

* Clear and concise code. No hacks, no syntax-soup. The code should be easy to follow even without an IDE. [Functions and variables should have descriptive names](http://www.slideshare.net/pirhilton/how-to-name-things-the-hardest-problem-in-programming) and some thought should go into which files functionality ends up in.
* Code style should be consistent. Not just within single files but throughout all of LXQt. We should have a styleguide and keep it short and example-driven so that our devs can naturally integrate it.
* Prioritize maintainability over performance in non-performance-critical sections. 
* No debug code left in production, even commented-out.
* Test suites are good, and they're best added early. Since we're a DE, don't test UI, test functionality. This is especially important for libraries.

*Immediately-available documentation*:

* Separate documentation (wiki, articles, etc)
 * Best used for things that have no relation to specific parts of the code. Reading material, how-tos, guides, etc. Beginner's Guide is part of this. It should not depend on the code (eg. if the code is updated, the documentation shouldn't suddenly be out of date).
 * User-driven documentation should be very easy to edit. Github wiki is a great platform for that (no pull requests, no approval).
 * It's not enough that documentation exists, it has to be easy to find! Link it wherever it's expected or helpful.
* Code metadata
 * Comments! Comment your code whenever you are doing something complex or unintuitive. Always document workarounds appropriately. When you fix a bug is also a good time to ask yourself if you shouldn't add a comment for it. 
 * On the other hand, do not *over-comment*. Do not simply repeat what the code says in English, comments should almost always *add* to the reader's comprehension.
 * Keep a clean `git log`! It is one of the first things a developer will look at when checking out a project. When you commit, maintain the same name and, if possible, email.
   * [Write informative commit messages](http://chris.beams.io/posts/git-commit/).
    * Avoid merge commits when merging PRs, we prefer [rebase (fast forward)](https://www.atlassian.com/git/tutorials/merging-vs-rebasing).
 * Similarly, respect `git blame`. One commit per feature, and each commit should compile and work reasonably well (so that bisection for regression testing is possible). Avoid very large commits.

*Communication channels with people familiar with the codebase*:

* Maintainers in the file headers, with name + email address. 
* IRC is our main communication channel. If you're a regular contributor please be online and logged in if possible so you can be autovoiced for easy pings.
* Prominent links to our mailing list. We *really* should move it to `@lxde.org` ASAP, so that we have control over the archives.

*Clear goals: What we want, our priorities, etc*:

Each release should have well documented tasks. Priorities should be clear. Guidelines on what goes or doesn't go into LXQt should be clarified whenever possible; we want to avoid feature creep, but we don't want to stop people from contributing third party software. The [QtDesktop organization](https://github.com/QtDesktop) is there for grouping development on such software if ever needed.
This is all mostly on me ([**@jleclanche**](https://github.com/jleclanche/)) so do not hesitate to bug me for it :-)

*Writing a Beginner's Guide*:

This is self-explanatory. LXQt has a lot of components, we should have clear entry points for contribution to each of them. Communication channels should be repeated there. Ask new devs for feedback on pain points to figure out what to improve. [CONTRIBUTING.md](https://github.com/lxde/lxqt/blob/master/CONTRIBUTING.md) is a good starting point and should link to it.


Apart from the beginner's guide, these are rules which help *all* contributors including long-time ones. We should work on them starting immediately.