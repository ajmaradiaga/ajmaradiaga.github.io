---
layout: post
title: TIL: Rename a remote branch from Github CLI
---

I've been playing with the Github CLI lately and thought it will be interesting to see if I can rename the `master` (default) branch to `main` in a couple of legacy repos. This is easily achieved by using the [Github API](https://docs.github.com/en/rest/reference/repos) via the [GitHub CLI](https://cli.github.com/)

{% highlight console %}
# Navigate to the repository folder
$ cd my-local-folder

# Call the rename a branch method - https://docs.github.com/en/rest/reference/repos#rename-a-branch.
# Note that if the renamed branch is the default branch, it will also set the new name as default
$ gh api -X POST -H 'Accept: application/vnd.github.v3.raw+json' repos/:owner/:repo/branches/master/rename -F new_name=main

# Once updated, you can update your local repo
$ git branch -m master main
$ git fetch origin
$ git branch -u origin/main main
$ git remote set-head origin -a
{% endhighlight %}

PS: This is my first TIL (Today I learned) post. I've been inspired by the work and dedication of [Simon Willison](https://simonwillison.net/2021/May/2/one-year-of-tils/) and [Josh Branchaud](https://github.com/jbranchaud/til), who regularly share TILs. The plan is to now share on a daily basis what I learn.