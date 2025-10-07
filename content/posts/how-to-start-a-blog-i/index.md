---
title: How to start a blog (2025)
description: Hosting a Hugo blog using GitHub Pages
date: 2025-10-07
categories: ["tutorial"]
tags: ["Hugo", "GitHub Pages", "Blog"]
draft: false
---

## Introduction

**First of all:** Welcome to my brand new homelab blog!

This is not only the start of an article series on **How to start a blog (2025)**, it is also the very first post on my blog. So please be kind, I never did that before!

### Why (yet another) homelab blog?

Actually there are quite a few reasons:

- A few months ago a close friend of mine told me she was going to start an animal welfare association and wanted me to help her launch a website. So what could be a better motivation to check out some CMS software than **helping puppies**? :dog:
- While there are already many pretty awesome blogs in the homelab community, I always had the feeling that they often **only show you the basic stuff** and leave you alone when it comes to the more interesting problems. For example there are countless tutorials on how to set up a reverse proxy and use let's encrypt to get valid certificates for your internal services, but there is basically nothing on how to use the very same technology to get certificates for LDAPS.
- Ever had to pause on a project for a few weeks or even months and found some pretty weird config settings you had absolutely no clue about anymore? Been there, done that. There were just too many times I couldn't for the life of me remember why I chose to set up the service the way I did. So this blog hopefully will also serve me as a **documentation of my homelab** in case of desaster recovery.

That's why I decided to start this blog and provide an article series about how to start a homelab from scratch. Starting with the install of the hypervisor and going through parts like adding reverse proxies, authentication and monitoring services while also paying attention to coding best practices like linting or security checks along our way.

### What this tutorial covers

In this tutorial we will set up a [Hugo](https://gohugo.io) project, configure the look of your new blog, add some very first content and host the website using [GitHub Pages](https://pages.github.com). So at the end of this article you will have a fully functional blog that can be read by a public audience. Pretty neat, isn't it?

### A few words on Hugo

[Hugo](https://gohugo.io) is an open source static site generator. That means, that it takes ordinary markdown files and creates a static website out of it in milliseconds. There is no database involved and it's incredibly fast. You can add your markdown files to a version control system and have the page hosted using a platform like [GitHub Pages](https://pages.github.com) or [Cloudflare Pages](https://pages.cloudflare.com/). As it is way less complex than a full blown CMS like [WordPress](https://wordpress.com), it is also easier to set up and has less security problems.

While there are also other popular static site generators like [Jekyll](https://jekyllrb.com/), [Eleventy](https://www.11ty.dev/) or [Gatsby](https://www.gatsbyjs.com/), I decided to go with Hugo as it seems to be one of the most widely used ones and is written in Golang.

## Pre-Requisites

So what will you need to follow along with this tutorial? Actually not that much!

- **the code editor of your choice:** I'm using VS Code with the WSL plugin here but use whatever editor you are comfortable with.
- **a GitHub account:** Well, that one most of you will already have anyways and in case you don't, it shouldn't be too challenging.
- **some basic knowledge of markdown and git:** Or at the very least you should know what the both of them are. Most modern code editors do the hard work of git commands for you anyways. If you're not comfortable with markdown yet, there is a pretty good documentation called [Markdown Guide](https://www.markdownguide.org/) you can check out first.
- **a registered domain:** *(optional)* While this one is not really necessary to follow along, I would really recommend you to get one. It's not as expensive as you might think (I'm paying 24 € a year for this one) and makes your blog look way more professional.
- **a cup of *whatever your favorite hot drink is*:** Yep, already quite cold outside here in Germany.

## Installing Hugo

Installing Hugo is not that hard. You can find instructions for all major operating systems in the [documentation](https://gohugo.io/installation/). I'm going to install it on a fresh Ubuntu 24.04 WSL machine using apt:

```bash
sudo apt update
sudo apt install hugo
```

You can check if your install was successful using the command `hugo version` in your terminal.

> :bulb: **Tip:** Unfortunately the package provided by apt was already quite old when I wrote this tutorial (**v0.131.0**), so I decided to go with the manual install of the newest version **v0.151.0** instead.

```bash
sudo apt update
wget https://github.com/gohugoio/hugo/releases/download/v0.151.0/hugo_extended_0.151.0_linux-amd64.deb
sudo apt install ./hugo_extended_0.151.0_linux-amd64.deb
```

## Creating your project

As already stated, we want to host the blog using GitHub Pages, so the very first step will be to create a repository for it. Head over to [GitHub](https://github.com) and create a new repository which is named `<your username>.github.io`. In my case, this is going to be `webealin.github.io`. A bit of a caveat here is, that your repository has to be public, if you're not a GitHub pro user. But I guess it is not that much of a problem as the blog is going to be publicly hosted anyways.

> :warning: **Warning:** It is pretty important, that you call the repository exactly like that. Your blog will later be published under that subdomain. If you would call the repository for example `myblog`, the blog would later be hosted under `<your username>.github.io/myblog` which is probably not what you wanted to end up with.

Next we are creating our project directory, which I am going to name 'myblog' for this tutorial. I want to store it in a 'projects' directory I have in my home folder, so I will change the directory to this folder first.

```bash
cd ~/projects
hugo new site myblog
cd myblog
```

If we open the folder in our code editor now, we will see that Hugo already created the default structure of the project for us.

## Changing the look & feel

## Adding some first content

## Hosting your blog using GitHub Pages

## Next steps

So what's next? As stated in the beginning, this is supposed to be an article series. In the next posts I want to go through some more advanced topics like adding support for multiple languages or adding a CMS like Decap CMS to your blog.

## References

References to the tools and products used within this tutorial

- **Hugo:** [https://gohugo.io](https://gohugo.io)
- **GitHub Pages:** [GitHub Pages](https://pages.github.com)
