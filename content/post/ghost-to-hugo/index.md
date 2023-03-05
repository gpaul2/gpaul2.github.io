+++
author = "Geoji Paul"
categories = ["Technology"]
date = 2022-12-22T04:58:36.000Z
description = ""
draft = false
image = "https://images.unsplash.com/photo-1521295121783-8a321d551ad2?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1170&q=80"
slug = "ghost-to-hugo"
summary = "Instructions on migrating from Ghost to Hugo and deploying in GitHub pages."
tags = ["Technology"]
title = "Migrating from Ghost to Hugo"

+++

The post covers the topic of migrating an existing website from Ghost CMS to a static website generated using Hugo, and deployed using GitHub pages.

### Background

This blog was initially built using Ghost CMS and deployed in AWS Lightsail. However, recently I made the decision to migrate away from Ghost and AWS. This was mainly due to the below reasons:
* Patch management of Ghost and base OS became a chore  
* AWS Lightsail is not free. The website was costing around $5/month (apart from the domain registration fees)
* Constant worry about waking up to a ginormous AWS bill due to a hacking event

As such, the requirements for the alternative were that; it should be serverless with zero infrastructure management, no CMS upkeep, and free to host. After doing some research, the option that fit all requirements was to use a static website builder like Hugo and host it using GitHub pages. If you are in a similar situation, continue to read on.

### Step 1: Convert Ghost Website to Hugo

Migrating the core website from Ghost to Hugo was accomplished using [ghostToHugo](https://github.com/jbarone/ghostToHugo/). How-to articles for extracting Ghost website into a json format, and usage of the ghosToHugo tools are pretty self explanatory. To avoid having to install the program, the pre-compiled binary of ghostToHugo from the [releases page](https://github.com/jbarone/ghostToHugo/releases/tag/v0.5.3) was used. After migrating the website, the [Clean White Hugo Theme](https://themes.gohugo.io/themes/hugo-theme-cleanwhite/) was used to make it look nice. The theme was chosen because of the layout and because of the built-in support for Disqus and Google Analytics. 

Note that images will have to be separately downloaded from the server running your Ghost CMS and placed in your local repository. Since I had used Bitnami for the initial Ghost deployment, the image path was not where typically Ghost stores images (path in command snippet below). Once you locate the image path, you can use the scp command to copy down the image content. Note that you will need to have either the login credentials or private key to your EC2 instance, IP address of your EC2 instance, and year and month of the posts so as to pull down the images.

```
scp -r -i EC2InstanceSSHKey.cer bitnami@EC2IPAddress:/opt/bitnami/apps/ghost/htdocs/content/images/PostYear/PostMonth/ ./HugoRootDirectory/static/img/
```

Once images are downloaded, ensure that those are accurately linked to the post. After experimenting a few configurations, what worked was to create a folder for every post and store images associated to the post in the folder as shown below:

{{< figure src="/post/ghost-to-hugo/folderstructure.png" >}}

For the above image to render accurately, it was referred to using the slug as shown below: 

```
{< figure src="/post/ghost-to-hugo/folderstructure.png" >}
```

### Step 2: Deploying into GitHub Pages

Once the website is built locally, the next step is to create a public repo in Github and push the contents up to the public repo. For Github pages to work, you will have to use the naming convention described by Github. In order for the page to work properly, create a new repository with ``` <USERNAME>.github.io ```

Once your code is pushed into the newly created Github repo, you can deploy the website using Github Actions. Instructions on how to do this is available in the Hugo [Hosting on Github Guide](https://gohugo.io/hosting-and-deployment/hosting-on-github/)

Happy hosting for free!
