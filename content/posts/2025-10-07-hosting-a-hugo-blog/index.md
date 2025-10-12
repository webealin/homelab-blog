---
title: Hosting a Hugo blog using GitHub Pages
description: Creating a Hugo blog, installing a theme, adding some content and hosting the page using GitHub Pages.
date: 2025-10-07
categories: ["tutorial"]
tags: ["Hugo", "Blowfish", "GitHub Pages", "Blog"]
series: ["How to Start a Blog"]
series_order: 1
draft: false
---

## Introduction

**First of all:** Welcome to my brand new homelab blog!

This is not only the start of an article series on **How to start a blog (2025)**, it is also the very first post on my blog. So please be kind, I never did that before!

### Why (yet another) homelab blog?

Actually there are quite a few reasons:

- A few months ago, a close friend of mine told me she was going to start an animal welfare association and wanted me to help her launch a website. So what could be a better motivation to check out some CMS software than **helping puppies**? :dog:
- While there are already many pretty awesome blogs in the homelab community, I always had the feeling that they often **only show you the basic stuff** and leave you alone when it comes to the more interesting problems. For example there are countless tutorials on how to set up a reverse proxy and use let's encrypt to get valid certificates for your internal services, but there is basically nothing on how to use the very same technology to get certificates for LDAPS.
- Ever had to pause on a project for a few weeks or even months and found some pretty weird config settings you had absolutely no clue about anymore? Been there, done that. There were just too many times I couldn't for the life of me remember why I chose to set up the service the way I did. So this blog hopefully will also serve me as a **documentation of my homelab** in case of disaster recovery.

That's why I decided to start this blog and provide an article series about how to start a homelab from scratch. Starting with the install of the hypervisor and going through parts like adding reverse proxies, authentication and monitoring services while also paying attention to coding best practices like linting or security checks along our way.

### What this tutorial covers

In this tutorial we will set up a [Hugo](https://gohugo.io) project, configure the look of your new blog, add some very first content and host the website using [GitHub Pages](https://pages.github.com). So at the end of this article you will have a fully functional blog that can be read by a public audience. Pretty neat, isn't it?

### A few words on Hugo

[Hugo](https://gohugo.io) is an open source static site generator. That means, that it takes ordinary markdown files and creates a static website out of it in milliseconds. There is no database involved and it's incredibly fast. You can add your markdown files to a version control system and have the page hosted using a platform like [GitHub Pages](https://pages.github.com) or [Cloudflare Pages](https://pages.cloudflare.com/). As it is way less complex than a full blown CMS like [WordPress](https://wordpress.com), it is also easier to set up and has less security problems.

While there are other popular static site generators like [Jekyll](https://jekyllrb.com/), [Eleventy](https://www.11ty.dev/) or [Gatsby](https://www.gatsbyjs.com/), I decided to go with Hugo as it seems to be one of the most widely used ones and is written in Golang.

## Pre-Requisites

So what will you need to follow along with this tutorial? Actually not that much!

- **the code editor of your choice:** I'm using VS Code with the WSL plugin here but use whatever editor you are comfortable with.
- **a GitHub account:** Well, that one most of you will already have anyways and in case you don't, it shouldn't be too challenging.
- **some basic knowledge of markdown and git:** Or at the very least you should know what the both of them are. Most modern code editors do the hard work of git commands for you anyways. If you're not comfortable with markdown yet, there is a pretty good documentation called [Markdown Guide](https://www.markdownguide.org/) you can check out first.
- **a registered domain:** *(optional)* While this one is not really necessary to follow along, I would really recommend you to get one. It's not as expensive as you might think (I'm only paying 24 € a year for this one) and makes your blog look way more professional.
- **a cup of *whatever your favorite hot drink is*:** Yep, it's already quite cold outside here in Germany.

## Installing Hugo

Installing Hugo is not that hard. You can find instructions for all major operating systems in the [documentation](https://gohugo.io/installation/). I'm going to install it on a fresh Ubuntu 24.04 WSL machine using apt:

```bash
sudo apt update
sudo apt install hugo
```

You can check if your install was successful using the command `hugo version` in your terminal.

{{< alert "lightbulb" >}}
**Tip!** Unfortunately the package provided by apt was already quite old when I wrote this tutorial (**v0.123.7.0**), so I decided to go with the manual install of the newest version **v0.151.0** instead.
{{< /alert >}}

```bash
sudo apt update
wget https://github.com/gohugoio/hugo/releases/download/v0.151.0/hugo_extended_0.151.0_linux-amd64.deb
sudo apt install ./hugo_extended_0.151.0_linux-amd64.deb
```

Note that you might need to restart your terminal session after that.

## Creating your project

As already stated, we want to host the blog using GitHub Pages, so the very first step will be to create a repository for it. Head over to [GitHub](https://github.com) and create a new repository which is named `<your username>.github.io`. In my case, this is going to be `webealin.github.io`. A bit of a caveat here is that your repository has to be public, if you're not a GitHub pro user. But I guess it's not that much of a problem as the blog is going to be publicly hosted anyways.

{{< alert >}}
**Warning!** It is pretty important, that you call the repository exactly like that. Your blog will later be published under that subdomain. If you would call the repository for example `myblog`, the blog would later be hosted under `<your username>.github.io/myblog` which is probably not what you wanted to end up with.
{{< /alert >}}

Next we are creating our project directory, which I am going to name 'myblog' for this tutorial. I want to store it in a 'projects' directory I have in my home folder, so I will change the directory to this folder first.

```bash
cd ~/projects
hugo new site myblog
cd myblog
```

If we open the folder in our code editor now, we will see that Hugo already created the default structure of the project for us. So the next step is to add the initial setup to our git repository.

```bash
git init
git branch -m main
git add .
git commit -m ":tada: initial commit"
git remote add origin git@github.com:<your username>/<your username>.github.io.git
git push -u origin main
```

You can start the hugo server using `hugo server` command now, which will tell you that the site is available under [http://localhost:1313/](http://localhost:1313/). Unfortunately, this only gives us a *Page Not Found* error. The reason is, that we did not add any layout yet.

But you may have noticed, that starting the server created a new *public* folder and a *.hugo_build.lock* in our project directory. Let's add them and another *resources* folder that doesn't exist yet to the *.gitignore* file so that we don't accidentally commit them.

```gitignore
# Hugo specific
/public/
/resources/
.hugo_build.lock
```

## Changing the look & feel

 While you could theoretically write your own layout, it's easiest to use one of the various [themes](https://themes.gohugo.io/) hugo provides. The ones I like most are [Blowfish](https://blowfish.page/), [Congo](https://themes.gohugo.io/themes/congo/) and [LoveIt](https://hugoloveit.com/). Turns out, Blowfish is basically a fork of Congo and as it has a pretty detailed [documentation](https://blowfish.page/docs/) I will install this one for the blog.

{{< alert "lightbulb" >}}
**Note!** While you can absolutely take any theme you want, note that the configuration from this point on depends on the theme you use!
{{< /alert >}}

```bash
git submodule add -b main https://github.com/nunocoracao/blowfish.git themes/blowfish
cp -r themes/blowfish/config ./config
rm hugo.toml
```

This basically adds the Blowfish repository as a subdirectory to our repository so that we can update our code and the Blowfish install independently without any issues. Afterwards the default configuration files for Blowfish are copied to the correct destination and the default config of hugo is removed as we don't need it anymore.

Let's now modify the configuration, so that we can see the blog for the very first time. Open the */config/_default/hugo.toml* file, uncomment the `theme = "blowfish"` attribute and set your domain as the `baseURL`. Your file should look like this by now:

```toml
# -- Site Configuration --
# Refer to the theme docs for more details about each of these parameters.
# https://blowfish.page/docs/getting-started/

theme = "blowfish"
baseURL = "https://<your username>.github.io/"
defaultContentLanguage = "en"
...
```

Next open up the *languages.en.toml* in the same directory, change the `title` as well as the `description` to your liking and uncomment the `[params.author]` section.

```toml
disabled = false
languageCode = "en"
languageName = "English"
weight = 1
title = "My awesome Blog!"

[params]
  displayName = "EN"
  isoCode = "en"
  rtl = false
  dateFormat = "2 January 2006"
  description = "This is my awesome website"

[params.author]
  name = "Alina Weber"
  email = "info@webernet.dev"
  image = "img/blowfish_logo.png"
  imageQuality = 96
  headline = "I'm only human"
  bio = "A little bit about you"
  links = [
    { email = "mailto:hello@your_domain.com" },
    { github = "https://github.com/username" }
  ]
```

If you start your server now using the `hugo server` command, you will have the very first version of your blog up and running! :tada:

![Blog Landing page](/blog.png)

Make sure to head over to the [configuration documentation](https://blowfish.page/docs/getting-started/) of Blowfish when you're done with this tutorial. You can check out all the options you can tweak on that page to make the design fit your needs.

## Adding some first content

Time to add some actual content to the page. Hugo already offers a command to start a new post:

```bash
hugo new content content/posts/my-first-post.md
```

This adds a new markdown file at *content/posts/my-first-post.md*. If you check the contents of this file compared to the one at *archetypes/default.md* you will notice that it basically substituted the values in this archetype file! This is called the **Frontmatter** and is the metadata of our file. Let's add some content:

```markdown
+++
date = '2025-10-07T21:22:55+02:00'
draft = true
title = 'My First Post'
+++

# Hello World

Welcome to my blog!

- [X] Install Hugo
- [X] Create Project
- [X] Change the look
- [X] Add some content
- [ ] Host on GitHub Pages
```

We should also tell Hugo to add the most recent posts to the landing page. You can do that in the *params.toml* configuration file under the `[homepage]` section. Set the attribute `showRecent` to true and start the server again.

You will see that our post is still missing on the front page! The reason is, that our post is still marked as *draft* in the frontmatter. Fortunately Hugo allows us to show these contents too using the flag `--buildDrafts`. Run this command and you will see our first post on the landing page.

![First post](/post.png)

### Add a thumbnail

You can also add a thumbnail to your post if you change the directory structure. Add a new directory *my-first-post* in the *content/posts* directory, move the *my-first-post.md* to the new directory and rename it to *index.md*. You can now add a *featured.png* to the very same folder and it will use the picture ad a thumbnail for you.

```project
...
├── content/
│   └── posts/
│       └── my-first-post
│           ├── my-first-post.md
│           └── featured.png
...
```

### Add some menu entries

We can also add some more basic pages that are not blog posts e.g. an *about* page.

If you are from Germany - as I am - you will probably also want to add pages for the legal notice and the privacy policy. No legal advice here, just saying "you will probably want to". We just place those pages to the *content* directory.

```project
...
├── content/
│   ├── posts/
│   │   └── my-first-post/
│   │       ├── my-first-post.md
│   │       └── featured.png
│   ├── about.md
│   ├── legal-notice.md
│   └── privacy-policy.md
...
```

Next head over to the *config/_default/menus.en.toml* file and add the menu entries:

```toml
[[main]]
  name = "Posts"
  pageRef = "posts"
  weight = 10

[[main]]
  name = "About Me"
  pageRef = "about"
  weight = 20

[[footer]]
  name = "Privacy Policy"
  pageRef = "privacy-policy"
  weight = 10

[[footer]]
  name = "Legal Notice"
  pageRef = "legal-notice"
   weight = 20
```

Great! if you're happy with the content of your files, make sure to commit all of your changes and you are ready to host your page.

## Hosting your blog using GitHub Pages

Head over to the *Settings* page of your repository and open up the *Pages* section. Switch the *Source* from *Deploy from a branch* to *GitHub Actions*.

Then create a new GitHub actions file at *.github/workflows/hugo.yaml* with the content from the official [Hugo documentation](https://gohugo.io/host-and-deploy/). Make sure to also add the lines specified on this page to your *config/_default/hugo.toml* file.

```toml
[caches]
  [caches.images]
    dir = ':cacheDir/images'
```

Commit and push your changes. Then open up the *Actions* tab in your GitHub repository and see your page being deployed! Less than a minute later, you should be able to open [https://\<your username\>.github.io](https://your_username.github.io) to see your blog in action.

:tada: **Congratulations** :tada: - You are up and running now!

## Next steps

So what's next? As stated in the beginning, this is supposed to be an article series. In the next posts I want to go through some more advanced topics like adding support for multiple languages or adding a CMS like Decap CMS to your blog.

## References

References to the tools and products used within this tutorial

- **Hugo:** [https://gohugo.io](https://gohugo.io)
- **Hugo Documentation:** [https://gohugo.io/getting-started/](https://gohugo.io/getting-started/)
- **Blowfish Documentation:** [https://blowfish.page/docs/welcome/](https://blowfish.page/docs/welcome/)
- **GitHub Pages:** [GitHub Pages](https://pages.github.com)
