# Github setup

For this course, you will likely be working on multiple clusters and you will want a way to save and maintain your changes across clusters. You may also want to hold onto your work in this course for future reference. It might be useful to you!

For this reason, we recommend each student set up a personal repository where you can save your completed assignments separate from the repository where assignemnts are released.

For those of you already familiar with github, you can skip most of this. The main important thing is that your repository **MUST** be **private**. If course staff finds your code solutions to the assignments publicly available, you will receive and academic integrity violation.

For those of you not as familiar with github, or in need of a refresher, here's a step-by-step walkthrough!

# Create a github repo

Create a [github](https://github.com/) account if you haven't already, and navigate to your dashboard. Click on the green 'New' button next to 'Top Repositories' to make a new repository. Name your repository something descriptive, and change the visibility to **private**. Do not pick a template, add a README, a .gitignore, or a license.

NOTE: Pushing your assignment solutions to a public repository is a violation of academic integrity. It is very imporant that you make your repository **private**.

Once you click 'Create Repository', you will see a screen with instructions on how to either create a new repository from the command line or push an existing repostory. You will be pushing your existing repository on the cluster. However, you may want to name your new remote something different.

On the cluster, if you enter the command `git remote -v` from the command line in your repository folder, you will likely see the following:

```
origin  https://github.com/parasollab/cs498-booster-track.git (fetch)
origin  https://github.com/parasollab/cs498-booster-track.git (push)
```

Your repository currently has a remote called `origin`, and it's the `cs498-booster-track` repo where staff releases assignments. You pull from `origin`, but you can't push to it (because then every student would see what you push).

You are going to add your repository as a new remote. You can name it whatever you want, but to start, call it `solutions`. Use the following command to add and push to this new remote:

```bash
git remote add solutions https://github.com/<your_github_username>/<your_repo_name>.git
git branch -M main
git push -u solutions main
```

Now, if you refresh the page, you'll see all of your code in your repo on github!

If you already have a repo on another cluster, just run the `git remote add` command from the block above to add the remote there as well.

# Github Workflow

Whenever you have worked on a unit of code that you would like to push, first run `git status` to see what all you have changed. Your changes start 'unstaged'. To stage them, use the command `git add <file_1> <file_2> ...` for anything you want staged. If you want to stage all changes, you can also run `git add .`. If you run `git status` again, you can verify that the changes were staged (sometimes you may need to change folders to the root of your repository if you are deep in a folder so that you can stage changes further up).

Once changes are 'staged' they can be `commited` with the command `git commit -m "<insert commit message>"`. It's good practice to commit incremental improvements in code and add a short and descriptive message whenever you do this. (But that is a skill that, admittedly, takes years of practice). Go ahead and run `git status` one more time to verify changes were commited.

After you commit, you can now push those changes to your remote with the command `git push solutions main`.

If you switch clusters (ex. you were working on Delta AI but now you want to work on delta), then you'll want to bring your code back up to date with what you had on the other cluster. Run `git pull solutions main`. This should work automatically, but one reason it may fail is if there are merge conflicts. Basically, all that means is that you made some changes to a file elsewhere and git doesn't know how to merge those changes with the files you have. The fix is to go into the files that git says it can't merge and manually fix it. The files with merge conflicts will have those secctions clearly marked like this:

```
<<<<<<<<<<<<<<<<<<<<< HEAD
some changes in some file
=======================
Different changes in another file
>>>>>>>>>>>>>>>>>>>> <some commit hash>
```

Remove all the extraneous `<<<<<<<<<<`, `============`, and `>>>>>>>>>>` markers and edit the file to the combined version you want. Then save it, stage the change, commit the change, and push it back to github.