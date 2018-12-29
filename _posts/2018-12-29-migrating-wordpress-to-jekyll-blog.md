---
layout: post
title: Migrating a wordpress blog to jekyll only 4 steps
excerpt: Wordpress is all nice and stuff but when all you need is just a clean place to write content, jekyll is the better tooling!
image: /assets/jekyll.png
image_alt: Migrating wordpress to jekyll
---

Wordpress is all nice and stuff but when all you need is just a clean place to write content, jekyll is the better tooling!
However, most developers, like myself, are stuck with a very old wordpress installation on their servers.
Worry not my friends, in this article, I’ll walk you 1 by 1 through the steps required to replace your wordpress blog with jekyll.

## Why not to use Wordpress.

Wordpress has some huge downsides. Here is what I’ve found most frustrating:

- You’ll have to backup your wordpress every week because sometimes it will break for no good reason.
- Wordpress can be hacked and will be hacked.
- To protect against hacks you’ll have to update your Wordpress every now and then, which is always a high risk operation since it might break your installation.
- Wordpress is slow af.

So as you can see it’s mostly about maintenance and the time you have to put into non-relevant things.

## What is jekyll ?

Jekyll is a static site generator based on ruby. Meaning, you give it content and it will create some static pages html for you.  
The nice thing is that Github Pages support Jekyll natively. That leads us to following advantages:

- Hosting on Github Pages is Free.
- Jekyll serves static Pages, those can’t be hacked.
- You don’t have to update Jekyll.
- Static Pages are lightning fast.

Basically, in the long run, that will save a lot of time.

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

_PS: If you want to setup everything on your own, you can read up the process on [Jekyllrb.com](http://jekyllrb.com)._

### 2. Import content from Wordpress

Now here comes the tricky part. All posts on the topic suggest to use Ben Balters [wordpress-to-jekyll-exporter](https://github.com/benbalter/wordpress-to-jekyll-exporter) plugin. However, that one did not work for me, as unfortunately it does not seem support the latest version of wordpress.  
Luckily, our friends at Jekyll wrote their own scripts to achieve just that. Head over to the [documentation on Jekyll Import](https://import.jekyllrb.com/docs/home/).  
As I did not have ssh access to my shared host (I know it’s crap but it is so damn cheap), I used the [Wordpress.com importer](https://import.jekyllrb.com/docs/wordpressdotcom/). The Process is also very easy:

- Install Jekyll importer with `gem install jekyll-import` (use `sudo gem install …` if it is complaining about rights)
- Run the importer:

```bash
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

#### Add pagination

From here, adding pagination is straight forward. You can use the provided [jekyll-paginate](https://jekyllrb.com/docs/pagination/)

#### Add a search

I used the [simple-jekyll-search](https://www.npmjs.com/package/simple-jekyll-search) javascript plugin.

#### Fix all links and add excerpts

This was the most time consuming thing as I had to go through all the blogposts, check the links for websites and images, update those links accordingly.

Same thing for the excerpts: open each blog post and add an introducing description.

#### Add related posts

Well here is the catch. The related posts by Jekyll are not really related to the article, it’s more jus showing the latest posts you wrote. So to add some mix to it, I combined the latest posts with some random posts, here is the magic code snippet:

```html
<ul class="related_posts">
  {{ "{% for post in site.related_posts limit: 4 "}} %} 
  {{ "{% if forloop.index > 2 "}} %} 
  {{ "{% assign random = site.time | date: '%s%N' | modulo: site.posts.size "}} %}
  {{ "{% assign post = site.posts[random] "}} %}
  {{ "{% endif "}} %}
  <li class="related_post">
    <a href="{{ post.url }}">
      {{ "{% if post.image "}} %}
      <img
        src="{{ post.image }}"
        alt="{{ post.image_alt }}"
        class="posts__img"
      />
      {{ "{% endif "}} %}
      <h3 class="title">{{ "{{ post.title "}} }}</h3>
    </a>
  </li>
  {{ "{% endfor "}} %}
</ul>
```

This will add 2 latest posts + 2 random posts.

#### Email Subscriptions

That’s also something I had to do from scratch, since I used a wordpress specific plugin before. For my Jekyll blog, I decided to use [mailchimp](https://mailchimp.com/) for a very simple reason: comparing it with other products, mailchimp did offer the biggest free plan and I did not want to pay just to save email addresses.

#### Comments

Since Jekyll is only a static site generator, you can only use third party commenting systems. I went for [Disqus](https://disqus.com/) because it’s already provided in the [Jekyll Now](https://github.com/barryclark/jekyll-now) setup.  
Disqus even offers a possibility to [import your wordpress comments](https://help.disqus.com/import-export-and-syncing/importing-comments-from-wordpress).

### 4. Redirects

So on Wordpress, the url for posts was `/portfolio/post`. However with Jekyll the path is now `/post` without the portfolio part. Since Jekyll does not run on any server, you have two options for redirects.  
Either you set up redirect on your domain name provider or you use some Javascript hack.  
I choose to go with the hack and here is the straight forward snipped I used:

```javascript
if (location.href.indexOf("/portfolio/") > -1)
  location.href = location.href.replace(/\/portfolio/g, "");
```

Which checks if there is the `/portfolio/` part in the URL and just removes it.

### Further improvements

#### Theming

Obviously theme of your blog can not be copied 1 by 1 since wordpress and jekyll are two fundamentally different tools.  
Luckily there are tons of [free Jekyll themes](http://jekyllthemes.org/), as well as [paid themes](https://jekyllthemes.io/) out there, ready for you to download.  
For my part, as I like to style things myself, I quickly copied the Theming of my Wordpress Theme myself using writing some SASS.

#### Custom Email Address

I chose to get rid of my custom email address `@thibaultjanbeyer.com`, just because I have to many addresses. But you don’t have to get rid of yours when changing DNS provider. The best solution in my opinion is to use the [free custom domain email from Zoho Mail](https://www.zoho.com/mail/help/email-hosting-with-zoho.html) I use that service for several other projects and in my opinion it is the most reliable free option.

## Last words

That’s about it. Now, the blog is running on Jekyll instead of wordpress. Hopefully you don’t even notice the difference or if so, only because it’s better now :)  
I hope that I could help someone who is thinking of leaving Wordpress. Don’t get me wrong, it’s work. But in my opinion it’s worth it.

<button data-a11y-dialog-show="subscribe-dialog" type="button" class="metalink">Subscribe</button> to my blog if you found this article useful. Thank you =)
