---
title: "Deploy Vue app to Github Pages"
date: 2020-02-19T22:30:00+01:00
draft: false
description: "Tutorial showing how to deploy a Vue CLI app to Github Pages"
tags: [
    "github-pages",
    "vue-js",
    "hosting",
]
categories: [
    "vue-js",
]
---
A step by step tutorial showing how to deploy a Vue CLI app to Github Pages.
<!--more-->

## Introduction

Vue is personally my favorite JavaScript framework. It is intuitive to use and simple in syntax. A good place to showcase your Vue projects is [Github Pages](https://pages.github.com/). If you haven't heard of Github Pages you should definitely check it out. With the help of Github Pages you can deploy frontend side code straight from Github to a website which you can view online. In this tutorial I'll show you how I deploy [Vue CLI](https://cli.vuejs.org/) projects to Github Pages.

Now there is two types of Github Pages sites, user site or project site. Each Github user can only have one user site and its repository must have a specific name: `<github-username>.github.io`. The user site is typically used as a portfolio page for developers. Then there are project sites which correspond with your other repos, and can be named anything you want. Project sites are mainly used to show documentation or demos for your projects. There are some differences between deploying the user site or project sites, which are explained below. Note, this is not a comprehensive guide on Github Pages or Vue CLI so be sure to look into those topics separately.



## Prerequisites

- [Github](https://github.com/)
- [npm](https://www.npmjs.com/get-npm)
- [Vue-CLI](https://cli.vuejs.org/guide/installation.html)
- [npm copyfiles](https://www.npmjs.com/package/copyfiles#install)
- [npm rimraf](https://www.npmjs.com/package/rimraf#cli)



## Step 1: Create Vue CLI project

Navigate to the directory where you want to create your project. Create a new project using the following Vue CLI command. This setups most of what you need to start developing a web application. Note, this creates a subdirectory with called `<project-name>` where the project is placed.

```bash
vue create <project-name>
```



## Step 1.5: Add vue.config.js (only for project site)

Create a file in the project directory called: `vue.config.js` and add the following lines as the content of the file. This ensures the build will have the correct paths when sourcing JavaScript and CSS code when deployed to Github Pages.

```javascript
module.exports = {
  publicPath: process.env.NODE_ENV === 'production'
    ? '/<project-name>/'
    : '/'
}
```



## Step 2: Automating the Build

Navigate to your project's `package.json` file. Add the following scripts to your `scripts` section. These are called npm scripts which help automate certain task in your project. You should already have the `serve`, `build`, and `lint` scripts by default, and now add the three additional scripts. The two subsections below explain how to automate the build for project sites and for a user site, respectively.

### Npm scripts (for project site)

```javascript
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint",
    "copy-dist": "copyfiles 'dist/**/*' './docs' -u 1",
    "clean": "rimraf dist docs",
    "deploy": "npm run clean && npm run build && npm run copy-dist"
  },
```

- `copy-dist` : Copies all the contents from the `dist` directory (this is your build directory) and copies it to the `docs` directory. It will automatically create the `docs` directory as well.
- `clean` : Deletes the contents of the `dist` and `docs` directory.
- `deploy` : Run the `clean`, `build`, and ` copy-dist` tasks in series

### Npm scripts (for user site)

```javascript
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint",
    "copy-dist": "copyfiles 'dist/**/*' './' -u 1",
    "clean": "rimraf dist css img js index.html favicon.ico",
    "deploy": "npm run clean && npm run build && npm run copy-dist",
  },
```

- `copy-dist` : Copies all the contents from the `dist` directory (this is your build directory) and copies it to the root directory of the project.
- `clean` : Deletes the contents of the `dist` and other build artifacts (such as `css`, `js`, etc) in the root directory.
- `deploy` : Run the `clean`, `build`, and ` copy-dist` tasks in series

### Running the build

By running the `npm run deploy` script, it builds your a production version of you project which is ready to be deployed. In case of a project site, the build is placed in the `docs` directory. It is important that the build is in the `docs` directory, since that is where Github Pages looks for the code to deploy. In case of the user site, the build is placed in the root directory of the project. Unfortunately, with the user site case the root directory might get clutter with all the build artifacts, but it has to be located there. Now run the following command to create your build.

```
npm run deploy
```



## Step 3: Create Github Repo

Now we create the Github repository for the Vue project. If you are creating a user site, it is important that you repository is named in a specific way. The repo must be named as such: `<github-username>.github.io`

If you are making a project site, then create a Github repo and name it `<project-name>` , the arbitrary project name you have been using througout following this tutorial. It is important the names match exactly with the name in the `vue.config.js` file, otherwise the website will not find the JavaScript and CSS resources. 

Once you create the Github repo, connect it with your local project and make sure to commit and push all the changes to Github.



## Step 4: Configure Github Pages

Go to the settings of your Github repository, and scroll down until you see the Github Pages section. To configure for a user site, you can only choose `master branch`, so go ahead and choose it. For project sites, choose `master branch /docs folder`, since that is where we placed the build contents. Also it is a good idea to check in the `Enforce HTTPS` as well, to add an extra layer of security to your site.

![Github Pages Settings](../githubpages.png)


## Conclusion

If you done everyting correctly, then you should be able to access your website on the link stated on the top section of the Github Pages settings. In case you made a user site, the link is `https://<github-username>.github.io` and if you made a project site, then it is `https://<github-username>.github.io/<project-name>/`. In my case, I have a configured a custom URL, so the Vue project I made along with this tutorial can be found here: [https://gaborkevinbarta.com/vue-demo/](https://gaborkevinbarta.com/vue-demo/)

If you have any questions feel free to contact me at `gkevinba@gmail.com` and I hope you find my tutorial useful.

