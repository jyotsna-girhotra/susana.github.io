---
layout: post
title: "Gifts for Future You and Friends"
date: 2018-04-25
categories: [coding, productivity, documentation, git, best-practices, maintainability]
---

As a general piece of advice, I believe getting your important things, whatever they may be, in some semblance of order will improve your quality of life. Maybe not immediately but eventually.

In the world of software development, one of the more obvious ways to do so is to document! Recording important points is invaluable because it saves everyone time and needless scrambling. Remember: documentation is for you, your fellow developers (new peeps and the OGs), and your future selves.

Below I'll detail a tips and habits I've learned during my career that have made my work-life smoother.

Caveat: not all of these tips may apply!

## Git

Take some time to think about how often you commit and what you include in your commits. Do you commit after some amount of time has passed? Do you only commit changes per directory or file? Do you commit new files along with changes to existing files? Do you commit when you've just remember you haven't committed in a while?

Then consider how easy or difficult it was to revert a particular code change or feature, or find a particular commit based on commit messages alone (i.e. in only looking at `git log`). Also imagine yourself as a newcomer to the codebase, would you be able to grok the sort of work that is taking place?


#### 📝 Create commits as if you're going to have to revert some subset of changes.

You may find you're working on a feature that requires multiple code changes across different parts of the codebase. I find that the most helpful thing to do is *not* lump all those changes into a single commit.
I prefer to put new files in their own commits. And I'll typically create additional commits if these files span various parts of the codebase. I.e., create one commit per set of new files for each part of your code base.


Generally I like to think of my commits in groups, where each group represents some part of the larger codebase I'm working on.

E.g: Separating concerns
```
Commit group 1: API controller and model changes

commit 1: model B changes, relevant changes to associated controller B
commit 1: model A changes, relevant changes to associated controller A
commit 0: new files


Commit group 2: Automation

commit 2: script B changes
commit 1: script A changes
commit 0: new files
```

The benefit I get from this is if some change I made in script B causes issues and needs to be reverted, I simply `git revert <SHA of commit 2>`. In a less ideal commit scenario, I may have written some changes to script B in _commit 1_ (of commit group 2).

E.g. Making changes to the same files across commits

```
Commit group 2: Automation

commit 2: script B changes
commit 1: script A changes, script B changes
commit 0: new files
```

In this case, reverting script B changes wouldn't be as straight forward as the previous example. I would not only need to revert _commit 2_, but I'd need to decide how I want to undo only the script B changes in _commit 1_.

#### 📝 Compose detailed commit messages.

**Why?** At some point, you've probably thought to yourself: "I'm not going to forget what this commit 'api stuff' was about". Then you did. And you realized you had 4 other commits at various points in time with the same message. Not exactly helpful. Providing a summary of what changes your commit contains will make `git log` perusal easier, making it easier to find commits you care about, and easier for new/unfamiliar peeps to know what's going on in your codebase.

**How?** First line should be a summary. Bullet points should follow that denote any particularly important points, context, caveats, tricks, notes, todos, hacks, constants updates, algorithm changes, etc.

#### 📝 `git rebase -i` is your friend

Go wild committing as you wish. You can always perform an interactive rebase before you actually push to origin to rearrange commits, reword commit messages, squash commits, and more!

## Code

#### 📝 Leave comments, and docstrings. Document methods, functions, classes, modules, and components.

**Why?** Friends, make documentation an inherent part of your programming, specifically documentation for your classes, methods, etc. You should always include these even if something seems incredibly obvious to you. And it will most likely feel that way if you've spent a lot of time in that codebase. But you must remember, this won't be true for everyone. Think about new hires (not just juniors, but those of any level), think about coworkers that have transitioned to a new team, think about your teammates that have spent most of their time in other parts of the codebase!

**How?** Follow your team's or department's doc style guide. Compose a description of your subject (a class, method, function, module, etc).

Include context if applicable. Perhaps you've implemented a `readFile()` method such that all values are read in as strings (don't worry, your team agreed this was okay for now) because a built-in function for discerning types contains a bug that cannot distinguish datetimes, and you know a very soon-to-be-released framework update is coming that will fix this. Including such a detail in your docs will relieve any head scratching from those unfamiliar with your code, and will also prevent others from trying to fix your "mistake". If you remember nothing else, try to **pretend you've never seen the codebase before**, and consider what you wish your documentation said.

Dealing with tons of data transformations? You may find it useful to mention schema changes, specifically by showing the input data's schema, and the output data's schema.

#### 📝 Try to avoid clever one-liners.

**Why?** I'm sure there's an appropriate circumstance for these (super duper optimizations?! cleverness competitions?!) but I've rarely seen them. One-liners can decrease code readability, make debugging more difficult, and code create a situation where refactoring becomes a pain.
**How?** Try avoiding obscure or rarely used syntax unless it's truly necessary. Instead, prefer to assign intermediate steps/values to variables. It'll make your code more readable, accessible to those unfamiliar with the code, and make debugging more insightful as you can discern on which line the code is breaking (imagine your debugger stopping at your one-liner).

## Additional Documentation

#### 📝 Compose documentation that encompasses a larger scope of your project.

**Why?** Outside of code comments and docstrings, your situation may call for documentation with a wider scope. Perhaps you want to include setup instructions, troubleshooting tips, FAQ, or historical context (aka "why did we create this thing?"). Again, think about yourself, your peers, and your future selves. As time goes on, all the things you know about your project in the moments you were working on it will fade away. I'm not even talking about a year into the future. I mean a month, or even a week. Capture the important details before they become difficult to recall.

**How?** Consider including a README, an instruction manual, a wiki, or any other type of document that can detail setup instructions, troubleshooting tips, historical context and more.

This may be difficult to achieve when changes are large and happen often. You'll have to use your best judgement in those cases. There may be some fine points that remain the same no matter what, and those could be worth recording.
