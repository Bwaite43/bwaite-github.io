---
title: Azure and Jekyll
author: Brandon Waite
date: 2022-06-26 00:00:00 +0800
categories: [Blogging, Tutorial]
tags: [Github, Jekyll, Azure]
---

# Static Site with Azure and Jekyll


## Azure and Jekyll

[Azure](https://azure.microsoft.com/en-us/services/app-service/static/)

[Jekyll](https://jekyllrb.com/)


Say you want to use a Static webpage for work or really just want to understand how to set it up with SSO so that you can put some authentication in front of your site. In this we will be using Azure Static web Apps with Jekyll and using Azure AD as the IDP. 

### Jekyll Install Dependencies

Head on over to the Jekyll website and check out install guide, its rather simple and quick to create your scaffolding for your static website. You can also check out my post [here](https://brandonw.me/posts/First-Post/).


```
  gem install bundler jekyll

  jekyll new my-awesome-site

  cd my-awesome-site

  bundle exec jekyll serve

Now browse to http://localhost:4000

```
Figure out what code repoistory your going to use, I would recommend Github. Microsoft is moving over to this anyway so ADO / Azure DevOps is old news. Create your repoistory and upload all the files in the directory that Jekyll create.  

### Azure Static Web Apps

Now that you have your scaffolding, lets head on over to [Azure](https://portal.azure.com/).

You will want to do a search for Static web apps. [Static web apps](https://azure.microsoft.com/en-us/services/app-service/static/)

During this process we will setup Static web apps to pull from our repo and build out and host the website. 

<div style="text-align: center">
<img src="https://brandonw.me/assets/images/valprop2.png" alt="swa"/>
</div>

Once you have the page for Static web apps open..


### Azure App registration

Now lets go a create a app registration for the IDP

### Test your Build


