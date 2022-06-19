---
title: First Post
author: Brandon Waite
date: 2022-06-19 00:00:00 +0800
categories: [Blogging, Tutorial]
tags: [Jekyll, Github, HUGO, AWS, Azure]
---

# Static Site with Jekyll and Github Pages

> Best way for me to learn is by just start doing it.
{: .prompt-tip }

This project started because I was looking to solve a problem at work with a documentation site. Thus down the rabbit hole this project was created. 

I was looking at Hugo and Jekyll for an easy to host static webpage. Seemed straight forward enough , I have used gitbooks before and it wasnt much different. The goal was to create a document site that was only available to a internal CIDR range. We also needed to have a code repostory for version control and community contribution. I wanted to have this all in a CI/CD pipeline so that when someone made and update to documentation it redployed the site with the updated changes. 

The site was just going to have a basic processes design to get someone new up and going, or where to look for xyz. I really didnt want to deal with authentication as it might be used by people without an account. Most of the people contributing to the site would be part of the technical team so learning markdown shouldn't be an issue. Templates would be made and instructions on how to clone the repo and update would be created. 

## CI/CD and Repo

[AzureDevOPS](https://azure.dev.com)
[Github](https://github.com/)

## AWS Amplify and HUGO 

[AWS Amplify](https://aws.amazon.com/amplify/)
[HUGO](https://gohugo.io/))

## Azure and HUGO

[Azure](https://azure.microsoft.com/en-us/services/app-service/static/)
[HUGO](https://gohugo.io/))

## Jekyll and Github Pages

[Jekyll](https://jekyllrb.com/)
[Github pages](https://pages.github.com/)

### Jekyll Install Dependencies

Homebrew - if you dont have this you should

[Homebrew](https://brew.sh/)

```
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
``

```
sudo apt update
sudo apt install ruby-full build-essential zlib1g-dev git
```

```
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

```
gem install jekyll bundler
```