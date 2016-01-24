> This is a try to add remote repository by using `git config` command.

> It turns out, that this is possible. Now I will make a few more tests, like some push and pull tests and after that, I'll try adding a new branch through `git config`<br>
> Then, I will add here steps taken to achieve that.<br>
> This is edited on GitHub so I can try pulling.

# The steps taken

## Creating repository

Create repository with `git init` command inside `git_practice` directory.

The `.git/config` file that is created at this moment looks like this:

```
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
```

There is no information about remote repository, and at this point it does not exist either.<br>
Next thing, go to GitHub and create a remote repository. After creating a remote repository, copy the url for repository. e.g. `git@github.com:Gruximillian/git_practice.git`<br>
Now, there are two repositories, local and remote, and at this point they are not connected.

## Connecting local and remote repository

To connect local and remote repository and set tracking between them we can use the following commands (this is the standard approach, and we will not use it here, this is here just to compare with used approach, covered bellow):

```
git remote add origin git@github.com:Gruximillian/git_practice.git
git push -u origin master
```

The first command sets url for remote named "origin" and the second command is connecting our local master branch and remote master branch to track each other and performs a push. **This is not what we will do!** :wink:

The information about remote repository is being kept inside `.git/config` file bellow `[remote]` section of the file. Inside that section there are two variables to set: `url` and `fetch`.<br>
The `url` part specifies the url of the remote repository (url that we copied from the GitHub) and the `fetch` part that has the format `+source:destination`.<br>
`source` is specifying branches from the remote repository and `destination` part is specifying the names of corresponding remote branches in the local repository. For remote named "origin" we can set those like this:<br>

```
git config remote.origin.url "git@github.com:Gruximillian/git_practice.git"
git config remote.origin.fetch "+refs/heads/*:refs/remotes/origin/*"
```
(`remote` part is the section and the remote name `"origin"` is its subsection in the `config` file)<br>
In the `config`, the following section is added:

```
[remote "origin"]
	url = git@github.com:Gruximillian/git_practice.git
	fetch = +refs/heads/*:refs/remotes/origin/*`
```

## Set tracking between local and remote repository

Now that remote repository is defined, if we try `git push` or `git pull` git will complain that it does not know to where or from here to push or pull. It will ask you to specify the name of the branch, like this: `git push master` or `git pull master`.

Next thing we need to do is set tracking between local and remote branches. For `master` branch we will do it inside `[branch "master"]` subsection (`branch` is section and the branch name `"master"` is its subsection), like this:

```
git config branch.master.remote "origin"
git config branch.master.merge "refs/heads/master"
```

Now, we get new section in the `config` file:

```
[branch "master"]
	remote = origin
	merge = refs/heads/master
```

In this section it is specified that the `master` branch will track `master` branch from remote repository called `origin`.

Now that we have set up remote repository for the `master` branch and tracking between remote and local master branches. To push and pull from and to master branch we can now use just `git push` and `git pull`.

## Creating new branch and setting its remote and tracking

What is a master without an apprentice?<br>
Let's create new branch that will be called "apprentice":

`git branch apprentice`

Now we have another branch that we can checkout (`git checkout apprentice`) and make some changes to the files inside that branch.<br>
Since the remote repository is set, we can push these changes to the remote, but if we try to pull changes for this branch from its remote counterpart we will get a message that there is no tracking information for this branch and that pull can not be performed.

Normally, we can do that with:

`git branch -u origin/apprentice apprentice`

but, we are not here to do things in a "normal" way.

We want to use `git config` to set up tracking for `apprentice` branch. Since the remote location is already set, we just need to do the tracking part like we did for the master branch:

```
git config branch.apprentice.remote "origin"
git config branch.apprentice.merge "refs/heads/apprentice"
```

Now, we get new section in the `config` file:

```
[branch "apprentice"]
	remote = origin
	merge = refs/heads/apprentice
```

And now, if we are on the `apprentice` branch, we can use just `git push` and `git pull`, without specifying the branch name, to push an pull on this branch.

## Using different remote for a branch

> Why do it simple when you can complicate it

Looking at the `.git/config` file, we can conclude that we can add another subsection for the `remote` section. If we can do that, then it turns out that we can use a different remote repository for some of our branches. As to why we would do that, I currently do not have some useful answer. We will do ti here because we can, just out of curiosity.

For defining new `remote`, as we noticed above, we need to add another subsection for that new remote:

```
git config remote.other_remote.url "git@github.com:Gruximillian/test.git"
git config remote.other_remote.fetch "+refs/heads/*:refs/remotes/other_remote/*"
```
This will add a new remote repository called "test", and set new remote branch called "other_remote".<br>
In the `config`, the following section is added:

```
[remote "other_remote"]
	url = git@github.com:Gruximillian/test.git
	fetch = +refs/heads/*:refs/remotes/other_remote/*`
```

Next, we will create new branch (`git branch newone`), and lastly, we need to set up tracking for this branch:

```
git config branch.newone.remote "other_remote"
git config branch.newone.merge "refs/heads/newone"
```

Now, we get new section in the `config` file:

```
[branch "newone"]
	remote = other_remote
	merge = refs/heads/newone
```

This is telling git to fetch from the `other_remote` repository and to merge remote `newone` branch into our `newone` branch.<br>
Our local `newone` branch is actually a remote branch of the `other_remote` repository, and not for remote `origin` repository. So, GitHub repository `git_practice` will not have `newone` branch, but the `test` repository
[will have it](https://github.com/Gruximillian/test/tree/newone).
Notice that if we want to use `master` branch from the `other_remote` we will get conflicting definitions for master branch:

```
[branch "master"]
	remote = other_remote
	merge = refs/heads/master
```

will conflict with:


```
[branch "master"]
	remote = origin
	merge = refs/heads/master
```

The usage of different repositories for different branches may or may not be smart, useful or needed. Is this in some way useful or needed, I can't say at this moment, because my experience with Git is pretty much superficial.<br>
But, it is obvious that is is doable, and that was all the point of this section and very much the point of this complete article, because everything described here can be achieved with Gits built in commands.
