+++
author = "Geoji Paul"
categories = ["Technology"]
date = 2020-03-15T23:57:57Z
description = ""
draft = false
image = "https://images.unsplash.com/photo-1558494949-ef010cbdcc31?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=2000&fit=max&ixid=eyJhcHBfaWQiOjExNzczfQ"
slug = "setting-up-ghost-using-aws-lightsail"
summary = "A how to article on setting up Ghost using AWS Lightsail"
tags = ["Technology"]
title = "Setting up Ghost using AWS Lightsail"

+++


In this post, we will go through the steps of setting up an AWS account, securing it, creating a domain name using Route53, and setting up Ghost using AWS Lightsail. The blog you are reading was setup with these steps.

### Setting up AWS account

Setting up an AWS account is relatively easy. Navigate to [AWS console](https://console.aws.amazon.com/) and follow the prompts to setup a root account. Keep in mind that you will need a cell phone number and a credit card to setup an AWS account. Once you complete entering your details, Amazon will ask you for what type of support plan you would like to choose. There are various support plans that are available for selection. For purposes of creating this blog, I selected the Basic Plan which is free.

Once the root account is created, ensure you setup MFA for the root account. Amazon has made it really easy to setup MFA. You can do that by navigating to IAM dashboard and selecting "Activate MFA on your root account". Once you do that, for further protection, it is advised that you never work with the root account. Hence, create a different user that you will use routinely to administer your AWS console. Depending on the level of access you want to provide to the new user, you can create groups and assign pre-built IAM policies to it. Once a group is created, assign the user to the group. You can setup MFA for individual users too which is highly recommended. Once you complete all Amazon recommended actions, your IAM dashboard should look like this.

{{< figure src="/post/setting-up-ghost-using-aws-lightsail/AWSIAM-1.png" >}}

### Creating a new domain using Route53

To create a new domain, navigate to "Register Domain" in Route53. Follow the steps to create the domain. For additional security enable transfer lock once you register the domain. Route53 provides private registration by default. Make use of it unless you want your registration details to be intentionally set as public for business reasons. Amazon will create a Hosted Zone by default when you create a domain. Make note of the name servers that you need to use for ensuring traffic is routed appropriately to your website (more on it under Lightsail section). If you already own a domain, you can use Route53's Transfer Domain feature to port your domain over.

### Setting up Lightsail

You can actually start creating your AWS account by going directly to [Lightsail](https://lightsail.aws.amazon.com). I will recommend however, that you go the longer route (as described above) to ensure that your root account is secured and you work with a lower privileged user as opposed to the root user for configuring Lightsail.

Once you are in the Lightsail console, click the "Create Instance" button to start the process. Lightsail will give you many options for deciding how to create the blog. In my case, I opted for the 'App + OS' option and selected Ghost for creating this blog. If you want to be more hands on, you can just create an OS at this stage and install your blogging platform on it separately.

{{< figure src="/post/setting-up-ghost-using-aws-lightsail/Lightsail.png" >}}

There are pros and cons of using App + OS vs. OS Only (discussed later). The 'App + OS' uses [Bitnami](https://bitnami.com/) for packaging the necessary applications needed for the blogging platform. In my case, since I selected Ghost as my blogging platform, the Bitnami image came with Ghost, mySQL and Apache pre-installed and configured. All that was needed to be done was to configure the [Ghost domain name](https://docs.bitnami.com/aws/apps/ghost/administration/configure-domain/) and [generate SSL certificate](https://docs.bitnami.com/aws/apps/ghost/administration/generate-configure-certificate-letsencrypt/).

### Creating Static IP and DNS Zone

Once you have your blog installed, you will have to create a public static IP for your website. This can be completed from the Networking section of Lightsail. Creating static IP is extremely simple. Lightsail will guide you through creation of the IP and attaching it to your EC2 instance.

Next is to create a DNS Zone. This can also be done using the Networking tab in Lightsail. Follow the prompts to create the DNS Zone. Where you need to pay attention is when you are asked to create DNS records for your zone. If you do not need any special routing, you can just have two A records. One for the root (indicated by @) and another for www. With those two A records created, you should be all set for the DNS Zone.

**Important**: In my case, since I had registered the domain using Route53 and since Route53 automatically creates a DNS Zone for the registered domain, the name servers used by Route53 and Lightsail will be different. Because of this, your website will not resolve properly. To fix the issue, you have to go back to Route53 and update the name servers to use the Ligthsail name servers.

Once the name servers are updated and routing works properly, you should be able to pull up your website from the internet. Bitnami has instruction on h[ow to access the admin console](https://docs.bitnami.com/aws/apps/ghost/get-started/first-steps/) with the initial installation and [how to turn off the Bitnami banner](https://docs.bitnami.com/aws/how-to/bitnami-remove-banner/) that comes default with the installation.

### Pros and Cons of App + OS option

Using Bitnami definitely cut down on the amount of time I would have otherwise spent installing and configuring these components separately. Also, another pro of using Bitnami is that the package comes with a good baseline security configuration such as using a randomly generated password for your admin console, locking down ports on your OS etc.

As far as cons, the two main issues I found was with usage of custom installation paths and config files, and general issues with https.

Bitnami uses custom installation path for Ghost and Apahce. For example, if you are trying to find a certain config file or executable using the standard installation path, you will not find it. Because of it, some of the utilities provided by the blogging platform may not work out of the box. You might have to adjust your PATH variables or find a bin from an obscure path to be able to use it. Not a major issue, but something to be aware of, if you are going down this path.

Using https was probably where I spent a little more time on. This is because configuring Ghost domain name using Bitnami's bnconfig (command below) does not allow for setting an https URL.

```
sudo /opt/bitnami/apps/ghost/bnconfig --machine_hostname example.com
```

Although bnconfig will work with your SSL enabled website, you will notice however that the website will have mixed http and https content which some browsers may complain about. Also, mixed content could result in your site getting lower ranks compared to full https websites. To fix the issue, you will have to configure Ghost URL by editing the _/opt/bitnami/apps/ghost/htdocs_/_config.production.json_ directly. Also, you will have to edit your Apache configuration to [force https redirection](https://docs.bitnami.com/aws/apps/ghost/administration/force-https-apache/). In addition, to prevent infinite redirect loop, you will have to set the below configuration in your _/opt/bitnami/apache2/conf/bitnami/bitnami.conf_  

```
<VirtualHost *:80>
    RequestHeader set X-Forwarded-Proto "http"
    ...
</VirtualHost>

<VirtualHost *:443>
    RequestHeader set X-Forwarded-Proto "https"
    ...
</VirtualHost>
```

I am still in the early stages of playing with Bitnami. If there are more issues I find during the course of using this Blog, I will update it here. Good luck setting up your personal website!

