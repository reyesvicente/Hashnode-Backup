---
title: "Git for Amateurs"
datePublished: Tue Jun 25 2019 07:22:21 GMT+0000 (Coordinated Universal Time)
cuid: ckgot8qrg05gdo9s11e153wjc
slug: git-for-amateurs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682771006049/c0e682c3-195c-488d-a514-8b2c3f69064e.jpeg

---

*Originally posted on https://highcenburg.herokuapp.com*

Make it a habit to  run `git status` before doing anything on a repository. I just learned this today and wanted to write it down to make it stick on my mind - or get familiar with it.

1.) To create a new branch :  `git branch <name_of_branch>`. A good practice is to name the branch with the focus of your edit.

Ex:

 `git branch markdown`
 
2.) Next, run `git checkout <name_of_branch>` to work on your branch. Make it a habit to do small changes so it's easy to go back if you mess up.

Once done with your changes, you want to add all changes, commit to your changes and push it to your dev repository.

1.) Now you run  `git status` to see the changes on your repository.

2.) Then run  `git add <filename>` or you can do  `git add -A` to add all the changes instead.

3.) Next, commit your changes by running  `git commit -m "Your message"`

4.) Finally, you push your commit by running  `git push`. Git will compain because GitHub doesn't know about your new branch. So run  ` git push --set-upstream origin <branch_name>` 

If you want to delete a branch. We would go to the main branch then delete our branch from there. 

1.) To go to the main branch, run  `git checkout <main_branch>`

2.) Once in the main branch, run  `git branch -d <name_of_branch>` 

This is what I learned in an hour, with an example of the main repository from a mentor.