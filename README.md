This is a try to add remote repository by using `git config` command.

It turns out, that this is possible. Now I will make a few more tests, like some push and pull tests and after that, I'll try adding a new branch through `git config`<br>
Then, I will add here steps taken to achieve that.<br>
This is edited on GitHub so I can try pulling.

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
`source` is specifying branches from the remote repository and `destination` part is specifying the names of corresponding branches in the local repository. For remote named "origin" we can set those like this:<br>

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
