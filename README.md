I usually host my images at my google picasa gallery. And linking worked everywhere so far. You just need to right click on the image and select *'copy image location'* option or, from options to to the right, select link to the image and check *'image only, no link'* checkbox. Let's try it here.

Say hello to Odin!<br>
![Odin](https://lh3.googleusercontent.com/-MOZyebVNZz0/Te6NSNah7CI/AAAAAAAAPeo/hiO98k71jAA/s288-Ic42/DVCI0162.jpg)

But, for this course I upload images in the repository along with assignment files. It is more convenient to keep files together. It is easy, but you need to do some configuring before you can start doing it that way.

For those who would like to do it this way right now, I will try to explain how you can do it. We will learn much more on using git and github soon, I beleive, so you don't need to bother right now if it is complicated to you. Let's start.

When you clicked a link to accept your assignment and went to the github page for your assignment you get something similar to this screenshot here:

![Github new repository](https://lh3.googleusercontent.com/-MFy4C0qbPC4/VoJsHV_MKTI/AAAAAAAARLw/gYU7Iq_xHhk/s800-Ic42/git_new_repository.png)

Your repositiry now exists on the github site, but not on your computer. Now, we are following section that says ***…or create a new repository on the command line***, but not litteraly.

First, in your terminal (command line program), navigate to the folder where you want your assignment files to be.

Now use command:
>`git init`

This will tell git that it needs to track changes in this directory.

Next, add your files to the folder: text files, images, folders, anything what you need for the assignment.
If you now enter in your terminal this command:
>`git status`

git will show you that there are untracked files in this directory. You will now add them to git so that these files can be tracked. Use this command in your terminal:
>`git add *`

The star means that you will add all untracked files to git. You can do this separately for every file by substituting star with the file name, if you want. If you now try `git status` it will show that these files are now tracked.

When your files are finished, or even if they are not but you want to save current state, it is time to save a "snapshot" of your files to git. You can do this with command:
>`git commit -m "Description for commit"`

Option `-m` means "message", this is the description message for this commit that you will enter within quotes after `-m`. So, here you can describe what did you do in your project in this commit.

Now it is time to connect your local git repository and the remote git repository on github. In terminal, write this:
>`git remote add origin https://github.com/Gruximillian/test.git`

This will connect your local git repository (on your computer) and remote git repository (on github). **Of course, your link to the repository will be different, so take care of that.**

Finally, you want to send your local changes to the github. It is said that you want to **push** a commit. In your terminal, enter this command:
>`git push -u origin master`

Now, you will be asked to enter your github password so that you can be allowed to send files to github.
Since I have a setup for using **ssh** protocol, I can't test this to the end, but it should be working.

Now, your files are on github.
If later you change your files, you can send to github these new changes with just three steps:

>`git add *`<br>
`git commit -m "Message for new commit"`<br>
>`git push origin master`

And now, these new changes are saved on github.

I hope I was clear enough so you can benefit from this reading. If you have questions feel free to ask, and if I can, I will gladly help.
