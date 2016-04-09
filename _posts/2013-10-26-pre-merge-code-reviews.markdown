---
title: Pre-merge Code Reviews
slug: pre-merge-code-reviews
date: 2013-10-26
layout: post
published: true
tags: [blog]
abstract: "Rules, benefits, patterns, and anti-patterns for reviewing code before merging it."
---

I'm a big proponent of pre-merge code reviews. From my experience consulting for teams in problematic projects, I can say that
(along with daily standup meetings) pre-merge code reviews are one of the most effective and yet fairly easy changes a team can introduce to radically improve
the condition of the project. And even if you consider your project to be healthy, there's always room for improvement.


## The Rules

1. **Never Push to Master**<br>
Always push to a separate branch per logical unit (story, feature, bug, optimisation, refactor, improvement). Branches are easy to make and easy to merge when you use git (and you apply some of tips further down in this post).

2. **Never Merge Your Own Branch**<br>
This helps to ensures that code is in fact reviewed. If you are caught merging into master, you will order pizza for the whole team.

3. **Review Work in Progress First**<br>
When you are finished with a task, you notify the other team members that your work is ready for final review. Then you review existing branches. Before picking up a new task, you look at all open pull requests (including unfinished ones) and review the changes since the last time you checked.

4. **Merge responsibly**<br>
Merging a pull request is the responsibility of the whole team. A pull request can not be merged when someone in the team does not understand the code or the reasoning, or does not agree with the solution.

(Note that these rules are starting points. Figure out what works in your team, adapt continuously.)


## The Benefits

Having multiple sets of eyes review a pull request before it gets merged to master or an integration branch, is a great
way to catch defects early. At this time, they are usually still cheap to fix.

There are however much more important benefits. Instead of individual developers, the team is responsible for the internal and external quality of the code.
This is a great remedy against the blame culture that is still present in many organisations. Managers or team members can no longer point
fingers to an individual for not delivering a feature within the expected quality range. You tend to become happier and more productive, knowing that the
team has your back. You can afford to make a mistake; someone will find it quickly.

Another effect is something called 'swarming' in Kanban. Because you are encouraged to help out on other branches before starting your own,
you start to help others finishing work in progress. Stories are finished faster, and there's a better flow throughout the system.
Especially when stories are difficult, or when stories block other stories, it's liberating to have people come and help you to get it done.

And of course, there's all the benefits from the clear sense of code co-ownership. It's invaluable to have a team where everybody
knows what the code does, why it's designed that way, how everything fits together. It also reduces the Bus Factor: no single team
member is a bottleneck. Best practices are shared, and the code is more consistent. Opportunities for reuse are spotted before
lots of duplication happens.

In short, pre-merge code reviews grow the team's maturity.

## Making It Easier

Reading code is hard, much harder than writing it. Here are some ideas that I have found to make things easier.

- **Atomic Stories**<br>
Break down stories in smaller, more atomic stories. "Manage products" is way to large. "Add products", "Add an image to
a product", "Modify the description of a product", "Remove a product", "Set a new price": these are atomic, and it will make branches much easier to review and merge.

- **Acceptance Criteria**<br>
List the acceptance criteria in the pull request. (In GitHub, you can use [checkboxes in Markdown](https://github.com/blog/1375-task-lists-in-gfm-issues-pulls-comments),
making it easy to tick them).

- **Atomic Commits**<br>
Commit atomically. For example, don't do code formatting and code changes in one commit, it makes the diffs very hard to
read. Each commit should preferably leave the code base in a consistent state, but apart from that, there's no limit to how small the commits can be. I often commit single lines.

- **Pull Requests as Conversations**<br>
A pull request is a conversation. Open it when you push your first commit. Comment, ask for feedback, commit to other
people's branches if that helps them out. The pull request is the story of how the feature came into being.

- **Integrate Frequently**<br>
Pull master into your branch regularly, to avoid difficult merges.

- **Pair on Merges**<br>
Pair program on difficult merge conflicts, preferably with the person who wrote the code you are merging with. (Pair
programming in general is highly recommended, but off topic here.)

- **Review Tests Only**<br>
If you use TDD, and you are short on time, review only the tests. If these are satisfactory, the implementation will
probably be ok too.

- **Solve the Problem Before Coding**<br>
Catching problems early when the code is written is great, but catching them before the code is written, is better. Go
to the whiteboard with a couple of team members and draw out a solution everybody agrees with. Reviewing the code afterwards will be easier because you already have a model on the whiteboard.

I'm assuming in this post that you use GitHub, but there are other solutions, such as ReviewBoard, BarKeep, Phabricator,
and Gerrit. I did a little bit of research last year, and felt that GitHub was the best tool, but I have no hands-on
experience with the others. YMMV. The important factor for me is that you can review stories or branches as a whole,
and comment inline. (Update: @tvlooy suggested GitLab.)


## Pitfalls and anti-patterns



- **Merge Buddies**<br>
As soon as you impose rules, you'll find people who try to game the system. Sometimes two team members consistently
merge each other's pull requests, without actually looking at them. "I'll merge
yours if you merge mine." They appear to conform to the rules, but of course it's just a variation on their old habit of
 pushing to master. Catch this early. Gatekeeper can be a good remedy.

- **GateKeeper**<br>
One person is responsible for doing all the reviews and the merges. It's not necessarily an anti-pattern, as it can be useful to prevent
people from cheating. But if you keep having a long term Gatekeeper, you have serious problems. People will not feel responsible
 for their code, let alone others. They will not learn from other people's code, and they will adapt their code to the Gatekeeper's
 comments, without bothering to understand why. Avoid when possible.

- **Total Lockdown**<br>
One step further than having a Gatekeeper, is taking away everybody's permissions to touch the main repository, and forcing
them to use their own forks and send pull requests from there. It an be a good measure if the situation is really problematic, but it's
 a terrible downer for morale. Deserved or not, people will feel distrusted. (Update: To clarify, it's not a problem if they never
  had pull permissions from the beginning.)

- **The Apologists**<br>
"We don't have time for code reviews." Or: "Code reviews do not apply in this project." As with all discussions about
software quality, there will always be people who feel it's perfectly alright to save time writing junk first,
and then waste huge amounts of it bugfixing and putting out fires. They probably don't write tests either, or refactor code.
This is a deeper issue that won't be fixed by enforcing code reviews alone.

- **Cowboys and Hermits**<br>
Some people don't want their code looked at. They write their share, and guard it from others, by making sure they are the only ones
working on that area. That's unacceptable, and you'll need to figure out why they act this way.

- **Issue Trackers**<br>
Many teams discuss issues inside their issue trackers. The distance from the actual code is too great. GitHub's inline commenting,
 on individual lines, allows developers to talk about actual code instead of abstract descriptions of code.

- **Focus on Junk Details**<br>
If your code reviews focus on having the correct amount of spaces in the indentation, or typos in comments, you are wasting the effort.
Focus on the important issues: Are the tests sufficiently conclusive? Is the naming expressive? Is there coupling where none should be?
Will we still understand the code 12 months from now? Is it refactorable? ...

As usual, I will keep adding to this post as I find more patterns. Got tips?

Update: Apparently Phil Haack [blogged about code reviews](http://haacked.com/archive/2013/10/28/code-review-like-you-mean-it.aspx)
almost at the exact same time. He has some great tips like keeping a checklist, focusing on the code and
not the author, and stepping through the code. Worth reading!


## Read next

- [Code Reviews and Blame Culture](/2016/04/code-reviews-and-blame-culture/)
- [How Much Testing is Too Much?](/2014/12/how-much-testing-is-too-much/)
- [Small Controlled Experiments](/2014/03/small-controlled-experiments/)
