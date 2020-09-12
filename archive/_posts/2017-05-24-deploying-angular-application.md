---
layout: post
title: "Deploying Angular application using GitHub Pages"
date: 2017-05-24
category: archive
comments: true
author: "Harshit Prasad"
tags: [github pages, github, angular, web-development]
---


***This blog was originally posted on FOSSASIA Blog.***

In recent months, I have started working with Angular v2 framework as my project is based on this tech stack. Angular v2 is one of the famous frameworks of JavaScript developed by Google. The project name is ‘Susper’ which is currently being in development stages under FOSSASIA. In FOSSASIA, to be a good developer everyone follows good practices. One of the good practice is providing a live preview of the fix done in a pull request related to a particular issue. It was not simple to deploy test page as it looks on GitHub pages. I read a lot of StackOverflow answers and surfed on Google a lot to find a solution. Then I came up with a solution, which I’ll be sharing with you in this blog.

I’m assuming your Angular v2 application must be using webpack services and the latest version of Angular has been installed. Firstly, be sure Angular CLI must be updated. If not, then update the Angular CLI to a new version.

You must update both the global package and local package of your project.

Global package:

```
npm uninstall –g @angular/cli
npm cache clean
npm install –g @angular/cli@latest
```

Make sure to install local packages, you must be inside the project folder.

To make deployments easier, follow these steps after updating global and local packages:

#### Install angular-cli-ghpages
```
npm i –g angular–cli–ghpages
```

This command is similar to the old github `pages:deploy` command of `@angular/cli` and this script works great with Travis CI.

After installing you should see the changes in the `package.json` as well:
```
“devDependencies”: {
    “angular-cli-ghpages”: “^0.5.0”
}
```
After updating the global and local package, you will notice a new folder named ‘node_modules’ has been created. Now the magic part comes to play here!

#### Add deploy script
In `package.json` file add the following deploy script:
```
“scripts”: {
    “deploy”: “ng build –prod –aot –base-href=/project_repo_name/ && cp ./dist/index.html ./dist/404.html && ./node_modules/.bin/angular-cli-ghpages –no-silent”
}
```
Our setup has been done and we're ready to deploy test page.

Now, here it comes to generate a live preview.

Steps:
```
git checkout working_branch
ng build
npm run deploy
```

We have successfully deployed the repository to GitHub pages.

To refer live preview go here:
```
https://yourusername.github.io/project_name
```

#### How did it work out?

Well, this is the easiest way to deploy any Angular v2 app on GitHub pages. The only disadvantage of deploying to GitHub pages is that we have to always perform a manual build before providing a live preview whenever some changes have been done in that particular branch.
