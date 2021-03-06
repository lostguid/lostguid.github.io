---
layout: post
title: GIT merge vs rebase
published: true
image: /img/git-merge-rebase/Git.png
tags: [git,programming]
subtitle: just squash it!
---
GIT is the most resourceful GI(F)T given by [Linus Torvalds](https://en.wikipedia.org/wiki/Linus_Torvalds) to dev community. The most common commands that I use on a daily basis are clone, fetch, push, commit, **stash**. In this post I want to give my thoughts on when to use **merge** vs **rebase**, the key differences between them, pros and cons of both.

### What is merge and rebase

To integrate changes from another branch we use either `git merge` or `git rebase`. While both these commands do the job, it is good to know when to use either of them for a better commit history!

Be informed that all the below examples are assumed to **not** have any merge-conflicts(no two separate branches have edits to the same line in a file)

#### Situation

Alright! We are in a situation now! Let's say there is a master branch and you checked out a feature branch(`b1`) from the master branch(`master`). Since you're a fast coder, you've already made 2 new commits(`C1`&`C2`) to your `b1` branch! Good going!
Now your colleague who works on the same team is working on another branch `b2`, wrote code faster than you, made 2 commits(`C3`&`C4`) and is ready push changes to master before you!

Let's visualize the above situation below!
![1](/img/git-merge-rebase/git1.PNG){: .center-block :}

Okay! So your colleague is done with his feature after those 2 commits and pushed the code to master branch. The master branch is now ahead of your branch(`b1`) by 2 commits! **Forked history!!!**
![2](/img/git-merge-rebase/git2.PNG){: .center-block :}

Now you found that you have to make changes in a file that's updated by your colleague when he merged his branch `b2` to `master`
#### What do you do???
Yes, you would do either `merge` or `rebase`. But which one and why?

### `git merge master`
![3](/img/git-merge-rebase/git3.PNG){: .center-block :}

Merging is nice because it’s a non-destructive operation. The existing branches are not changed in any way. This avoids all of the potential pitfalls of rebasing (discussed below).

On the other hand, this also means that the feature branch will have an extraneous merge commit every time you need to incorporate upstream changes. If master is very active, this can pollute your feature branch’s history quite a bit. While it’s possible to mitigate this issue with advanced `git log` options, it can make it hard for other developers to understand the history of the project.

### `git rebase master`
![4](/img/git-merge-rebase/git4.PNG){: .center-block :}

`rebase` moves the entire feature branch to begin on the tip of the master branch, effectively incorporating all of the new commits in master. But, instead of using a merge commit, rebasing re-writes the project history by creating **brand new** commits for each commit in the original branch.

The major benefit of rebasing is that you get a much cleaner project history. First, it eliminates the unnecessary merge commits required by git merge. Second, as you can see in the above diagram, rebasing also results in a perfectly linear project history—you can follow the tip of feature all the way to the beginning of the project without any forks. This makes it easier to navigate your project with commands like `git log`, `git bisect`, and `gitk`.

### When to use what?

Personally I feel `merge` should be used when there is individual ownership of features! If you are the owner of the feature you are working on(branch `b1` from the above example)and your colleague is the owner of branch `b2`, **do you care** about all the **interim commits**(`C3`&`C4`) performed by your colleague to branch `b2`?
Personally, I don't care about them, I'd rather focus on my commits on my feature branch!

`rebase` would be a better option when you are pulling something into your branch and no one else is looking at your branch.

### Reviewing a Feature With a Pull Request

If you use pull requests as part of your code review process, you need to avoid using git rebase after creating the pull request. As soon as you make the pull request, other developers will be looking at your commits, which means that it’s a public branch. Re-writing its history will make it impossible for Git and your teammates to track any follow-up commits added to the feature.

Any changes from other developers need to be incorporated with git merge instead of git rebase.

For this reason, it’s usually a good idea to clean up your code with an interactive rebase before submitting your pull request.

After a feature has been approved by your team, you have the option of rebasing the feature onto the tip of the master branch before using git merge to integrate the feature into the main code base.

### Conclusion

Now, to the question of whether merging or rebasing is better: hopefully you’ll see that it’s not that simple. Git is a powerful tool, and allows you to do many things to and with your history, but every team and every project is different. It’s up to you to decide which one is best for your particular situation.

In general the way to get the best of both worlds is to rebase local changes you’ve made but haven’t shared yet before you push them in order to clean up your story, but never rebase anything you’ve pushed somewhere.
