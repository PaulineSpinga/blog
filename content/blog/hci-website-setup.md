---
title: "HCI - Website setup (Lab Homework-1)"
date: 2023-09-24T13:45:06+06:00
image: images/blog/blog-post-01.png
feature_image: images/blog/blog-details-image.jpg
author: Pauline SPINGA
subject : "HCI"
---
### Create your own website, using Hugo
As part of the M2 Augmented and Virtual Reality, we are asked to create our own website in order to present our projects. I chose to create mine on Hugo, one of the most popular open-spurce static site generators. In this post, I will explain the requested steps in order to set up your own page using Hugo on MacOS.

##### Installation 

- Open a terminal and execute the folling commands :  
``/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"``
- Then, install Hugo using brew commande : ``brew install hugo ``
- You can check the downloaded version : ``hugo version``
- Create your blog :  ``hugo new site blog_name``

##### Theme

- Choose a theme on https://themes.gohugo.io/ and follow the given instructions 
- I choose "Roxo Hugo", here are the steps to use this templates
- Make sure you're still in your website repository using: ``pwd``
- **Make sure your SSH keys are correctly set up** (cf. https://docs.github.com/fr/authentication/connecting-to-github-with-ssh/checking-for-existing-ssh-keys)
- Add the template repository into your Hugo Project repository as a submodule:   
``git submodule add git@github.com:StaticMania/roxo-hugo.git themes/roxo-hugo``
- To copy the data, content, static, resources & config.toml files from the exampleSite directory and to paste it on your Hugo Project, run ``cp -a themes/roxo-hugo/exampleSite/* .`` from the home repository of your project
- Delete the **Hugo.toml** and keep only the **config.toml**

##### Render your website
- Build your site with and run the server ``hugo serve``
- See the result at http://localhost:1313/


![blog-details-image-02](https://user-images.githubusercontent.com/16266381/71399826-2009b380-264f-11ea-9bc3-59d7fa9a9994.jpg)





