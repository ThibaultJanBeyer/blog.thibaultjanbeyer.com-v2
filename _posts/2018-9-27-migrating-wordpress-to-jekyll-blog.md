---
layout: post
title: Migrating a wordpress blog to jekyll only 4 steps
excerpt: Wordpress is all nice and stuff but when all you need is just a clean place to write content, jekyll is the better tooling!
image: /images/wiki/broomball-players-fighting-for-the-ball.jpg
image_alt: three broomball players fighting for the ball. One is jumping, the other one is already lying on the ice, the third one skates in from the back. They wear normal sports clothes, broomball specific helmets and have brooms. In the background stands a referee. 
---

Wordpress is all nice and stuff but when all you need is just a clean place to write content, jekyll is the better tooling!
However, most developers, like myself, are stuck with a very old wordpress installation on their servers.
Worry not my friends, in this article, I’ll walk you 1 by 1 through the steps required to replace your wordpress blog with jekyll.

## What’s the problem with wordpress ?

## What is jekyll ?

## What is Github & Why should I host on Github ?

## Why should I use jekyll ?

## How to move from Wordpress to Jekyll ?

### 1. Backup wordpress & Setup Jekyll

#### Backup Wordpress

First of all, you should backup your current wordpress installation in case something goes wrong.  
There are plenty of available backup tools in the wordpress plugins section. Have a look.  
Personally, I used BackupGuard for no specific reason.  
Go to "Backups" and run a new full backup. Then download the backup files and place them somewhere safe. This will be our fallback if anything goes wrong.

#### Setup Jekyll

Setting up Jekyll is very easy. You can set up everything by hand or use a preset.  
Because I’m lazy, I used a pre-configured Jekyll made by Barry Clask which is called [jekyll-now](https://github.com/barryclark/jekyll-now). Setting that one up is quite easy:
- Fork the repo.
- Follow the [local development guide](https://github.com/barryclark/jekyll-now#local-development)
- You can now run your blog locally using `jekyll serve`
That’s it.

*PS: If you want to setup everything on your own, you can read up the process on [Jekyllrb.com](http://jekyllrb.com).*  

### 2. Import content from Wordpress

Now here comes the tricky part. All posts on the topic suggest to use Ben Balters [wordpress-to-jekyll-exporter](https://github.com/benbalter/wordpress-to-jekyll-exporter) plugin. However, that one did not work for me, as unfortunately it does not seem support the latest version of wordpress.  
Luckily, our friends at Jekyll wrote their own scripts to achieve just that. Head over to the [documentation on Jekyll Import](https://import.jekyllrb.com/docs/home/).  
As I did not have ssh access to my shared host (I know it’s crap but it is so damn cheap), I used the [Wordpress.com importer](https://import.jekyllrb.com/docs/wordpressdotcom/). The Process is also very easy:
- Install Jekyll importer with `gem install jekyll-import` (use `sudo gem install …` if it is complaining about rights)
- Run the importer:
```
ruby -rubygems -e 'require "jekyll-import";
    JekyllImport::Importers::WordpressDotCom.run({
      "source" => "wordpress.xml",
      "no_fetch_images" => false,
      "assets_folder" => "assets"
    })'
```
Usually when you first call it, you’ll get errors about missing dependencies. Just install whatever it sais that is missing with `gem install NAME` (replace name with the name of the missing dependency).  

That will import all your wordpress content. For me, it put all my images in a new folder called `assets` and all my posts as html into the `_posts` folder.  

Now you can run `jekyll serve` all your posts should be then. Neat right?  

Sure it’s probably not 100% clean but it takes a lot of work away already. You should check your site for broken image link, sometimes when you pictures have special characters the importer was not able to download them. Also for me, unfortunately the `excerpt` was missing on every post, which I had to re-add manually. While doing that, I also cleaned out the unnescessary `meta` part which was added to every post.  

### 3. Setting everything up



### 4. 404 Redirects



### Further improvements

#### Theming

Obviously theme of your blog can not be copied 1 by 1 since wordpress and jekyll are two fundamentally different tools.  
Luckily there are tons of Jekyll themes out there, ready for you to download for free.
