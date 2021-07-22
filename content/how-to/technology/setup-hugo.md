---
title: "Setup Hugo"
date: 2021-07-22T16:36:30+07:00
draft: true
tags: ["how-to"]
categories: ["Technology"]
---

## **Setting Hugo Website & Host on Github**

### **Quick Usage Instructions**
1. Edit/add new content, this will automatically create new article in "content" directory.  
`hugo new <subdirectory_name>/<articlename.extension>`  
Ex. hugo new posts/new_post.md  
2. Push everything from project root "hugoWebsite" to Github <br>    
`git status`    
`git add .`          
`git commit -m "added new article"`         
`git push origin main`    
3. Render the content to "public" directory        
`hugo -D`  
4. Go to "public" directory and commit everything to GitHub       
`cd public`  
`git status`  
`git add .`  
`git commit -m "added new content"`  
`git push origin gh-pages` 
<br>
<br>
<br>

### **How to Setup in Detail**
**1.** Create local project directory called "hugoWebsite".      
   Follow Quick Start in below link      
   https://gohugo.io/getting-started/quick-start/#step-7-build-static-pages       
   Make sure everything work.          
**2.** Before proceeding, make sure that you create a project repository in Github "hugoWebsite" under the branch "main" instead of "master".         
**3.** Then push all the content in local project directory "hugoWebsite" onto remote Github repo "hugoWebsite".          
``git init``  
``git add .``   
``git commit -m "first commit"``    
``git branch -M main``  
``git remote add origin https://github.com/ne0permatrix/hugoWebsite.git``    
``git push -u origin main``  
**4.** Deployment of Project Pages From Your gh-pages branch.  
https://gohugo.io/hosting-and-deployment/hosting-on-github/  
<br>   
    **I. Preparations for gh-pages Branch**  
    + Add the "public" folder:
        Add "public" folder to your ".gitignore" file at the project root so that the directory is ignored on the main branch.  
        ``echo "public" >> .gitignore``       
    + You can now initialize your gh-pages branch as an empty orphan branch:  
    ``git checkout --orphan gh-pages``        
    ``git reset --hard`` (don't use this if it delete files/direcotries in branch main)              
    ``git commit --allow-empty -m "Initializing gh-pages branch"``      
    ``git push <upstream> gh-pages``          
    ex: git push origin gh-pages      
    ``git checkout main``       

    **II. Build and Deployment**  
    + Now check out the gh-pages branch into your public forlder using git's worktree feature. Essentially, the worktree allows you to have multiple branches of the same local repository to be checked out in different directories:  
        ``rm -rf public``  
        ``git worktree add -B gh-pages public origin/gh-pages``

    + Regenerate the site using the hugo command and commit the generated files on the gh-pages branch: hugo (run not in "public" direcotry but in project root   directory)  
    ``cd public``  
    ``git add .``  
    ``git commit -m "..."``  
    ``cd ..``

    + If the changes in your local gh-pages branch look alright, push them to the remote repo:
    ``git push origin gh-pages``
    + Set gh-pages as your publish branch  
        In order to use your gh-pages branch as your publishing branch, you?ll need to configure the repository within the GitHub UI. This will likely happen automatically once GitHub realizes you?ve created this branch. You can also set the branch manually from within your GitHub project:

        + Go to Settings
        + Go to GitHub Pages Section
        + From Source, select "gh-pages" branch and then save      
        If the option isn't enabled, you likely have not created the branch yet OR you have not pushed the branch from your local machine to the hosted repository on GitHub.

        After a short while, you?ll see the updated contents on your GitHub Pages site.

5. Configuration
    - Change "baseURL" in config.toml to gh-pages website link "https://ne0permatrix.github.io/hugoWebsite/"  
    - Then run  `hugo`  
    - Then add, commit, and push "public" directory to Github  
    - Also add, commit, and push project root directory "hugoWebsite" to GitHub. 

    **Problem** : the articles from "content" directory in "hugoWebsite" project root don't render to "public" directory.

    After everything is set, I only ran `hugo` in project root directory "hugoWebsite".  
    What `hugo` does is to render the site to "public" directory.
    
    **Solution** :   
    - Instead of running `hugo`, I ran `hugo -D` in project root directory "hugoWebsite"  
    - Then I push everything from "public" directory to GitHub.  
    `git status`  
    `git add .`  
    `git commit -m "ran hugo -D"`  
    `git push origin gh-pages`  
        
6. Adding Custom Domain to Github Pages
- Go to your domain (panhasuon.com) manager (1and1/IONOS)
- Config DNS by adding Github Ips like in the picture
- Go to your Github project root repo, in Setting add Domain Name (www.panhasuon.com) 
- Then add CNAME file / It will be added automatically
More Detail on how to do it, watch this video at 09:52min.  
https://www.youtube.com/watch?v=SKXkC4SqtRk&t=618s

7. Add New Custom Domain to config.toml in project root "hugoWebsite" <br>
- Add *http//:www.panhasuon.com* to config.toml
- Render Everything to *public* directory
`hugo -D`  
- Go to *public* directory and push everything to Github  
`cd public`  
`git status`  
`git add .`  
`git commit -m "added new custom domain to config.toml"`  
`git push origin gh-pages`  

From there everything should be working. 

### Adding Comment Section "Disqus"       
1. Create Disqus account       
2. Find disqusshortname on Disqus website from admin/general/        
3. Add `disqusshortname = "panhasuon"` to config.toml         

### Adding *Hugo Menus* "Categories" and "Tags"       
[For detail, follow this link!](https://gohugo.io/content-management/menus/)         
Add below to config.toml      

    [menu]    

      [[menu.main]]
        identifier = "categories"
        name = "Categories"
        pre = "<i class='fa fa-heart'></i>"
        url = "/categories/"
        weight = -110

      [[menu.main]]
        name = "Tags"
        post = "<span class='alert'>New!</span>"
        pre = "<i class='fa fa-road'></i>"
        url = "/tags/"
        weight = -100

<br>

### Error
***Might encounter some merging problem from Github with CNAME file
Solution is to do `git pull` and follow suggested intructions.***    

**Error When Pushing to gh-pages**
![Error When Pushing to gh-pages](/static/error-push-gh-pages.jpg)         
**Solution**        
Need to run `git pull`  


**Error - Website doesn't render CSS properly**
That's because you don't change baseURL in config.toml to either "https://ne0permatrix.github.io/hugoWebsite/" or your custom domain "https://www.panhasuon.com".
