---
title: "HCI - Website setup"
date: 2023-09-24T13:45:06+06:00
image: images/blog/HCI/blog-post-01.png
feature_image: images/blog/HCI/website.png
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

##### Deploy your website
- Create a GitHub repository 
- Push your local repository to Github
- Change your baseURL = “https://<username>.github.io/<reponame>” in the 
**config.toml** file
- Visit your GitHub repository. From the main menu choose Settings > Pages. In the section **Build and deployment** select **GitHub Actions**
- Visit the **Actions** section of your project and click on **set up a workflow yourself**
- Name your file **hugo.yaml** and copy past the following content : 

```yaml
# Sample workflow for building and deploying a Hugo site to GitHub Pages
name: Deploy Hugo site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches:
      - main

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

# Default to bash
defaults:
  run:
    shell: bash

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.115.4
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb          
      - name: Install Dart Sass
        run: sudo snap install dart-sass
      - name: Checkout
        uses: actions/checkout@v3
        with:
          submodules: recursive
          fetch-depth: 0
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v3
      - name: Install Node.js dependencies
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
      - name: Build with Hugo
        env:
          # For maximum backward compatibility with Hugo modules
          HUGO_ENVIRONMENT: production
          HUGO_ENV: production
        run: |
          hugo \
            --gc \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"          
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v1
        with:
          path: ./public

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```

- Commit the change to your local repository with a commit message “Add workflow”, and push to GitHub.
- From GitHub’s main menu, choose Actions. You will see the **Add workflow** in progress with an orange item. 

![orange](https://i.imgur.com/kMpEwPm.png)


- When GitHub has finished building and deploying your site, the color of the status indicator will change to green.

![green](https://i.imgur.com/ZvNIwcE.png)

- Click on the commit message and you will see that **build** and **deploy** ran successfully. 
- Click on the link under the **deploy** section to visit your website

![github_actions](https://i.imgur.com/bf0boQu.png)

### Set up Unity

- Install Unity Hub https://unity.com/download 
- Connect with your account/license ( choose personal license)
- Install Unity 2022.3.2f1 through Unity Hub unityhub://2022.3.2f1/d74737c6db50 
- Install the modules needed (build for IOS, Windows and Android)
- Install a code editor: Visual Studio Code and the Unity plug-in 







