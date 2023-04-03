---
layout: post
title: Blogging With Jekyll and Github
meta_title: How to Write a Blog With Jekyll and Github
last_modified_at: 2023-03-22 8:30:00 +0000
description: A guide to getting started making your own blog for free using Jekyll and Github pages
---

I am a developer, therefore I should have a blog. Right? I've been planning on setting one up for a while, but I didn't want to use Wordpress and was having trouble trying to figure out another solution without having to write one from scratch. Don't get me wrong, Wordpress does work well for blogs, but as a developer it just felt dirty to use that for my personal site where I should be showing off my mad skillz. Wordpress site != Mad Skillz. I figure it's kind of like a good carpenter buying all his wood furniture from Ikea. It's easy and works, but you can't be very proud to have it in your house.

I was also looking for something that I can use a very simple and clean layout for without too much trouble. Yes, Wordpress has lots of themes, but a lot of them are bloated, or are a pain in the ass to modify the templates unless you understand the Wordpress template system, I don't care about learning that. I like a lot of Tumblr sites that I've seen, but I hate not owning my data. I follow Zach Holman on [Twitter](http://www.twitter.com/holman) and read [his blog](http://www.zachholman.com), he's a smart developer at [GitHub](http://www.github.com) who has some interesting opinions and ideas, and he's actually pretty entertaining. I decided that I like the simplicity of his blog, and since he's a cool developer he must have created his blog in a cool way!? He did.

## Github Pages and Jekyll

There are a lot of people using Github for Git repository hosting, but did you know that your Github account also includes basic webhosting? Not only is it free, but it's automatically source controlled, and if you can push to a git repository, you can deploy your Github Pages. All you have to do is create a new repository called: *username.github.com*. If you are already familiar with HTML and Git, some of this will be pretty basic, so you can skip down a bit to find the good stuff.

First Create a directory for the new site:
```
mkdir ksornberger.github.com
cd ksornberger.github.com
```

Now create a basic index file for your site
```html
<!DOCTYPE html>
	<html>
	<head>
		<title>Kevin Sornberger/title>
	</head>

	<body>
		<h1>Kevin Sornberger's Blog!</h1>
	</body>
</html>
```
Initialize your repository and do the initial commit
```
touch README
git init
git add index.html
git touch README
git add README
git commit -m "Initial commit!"
```

Then lets push to GitHub!
```
git remote add origin git@github.com:ksornberger/ksornberger.github.com.git
git push origin master
```

Now go make a coffee or play a round of Black Ops Free For All because your page will be live at http://username.github.io in about 10 minutes.


## The Fun Stuff (Jekyll)
Adding a bunch of static files for your site is <del>boring</del> fine, but it isn't the most effective way to run your site. This is where the [Jekyll](http://github.com/mojombo/jekyll) comes into play. Jekyll is a simple, blog aware, static site generation tool. It takes a template directory, which represents the raw from of your website, runs it through Textile or Markdown and [Liquid](http://liquidmarkup.org/) converters, and outputs a complete status website for you to serve. The best part of this, is that GitHub uses Jekyll as the engine behind [GitHub Pages](http://pages.github.com/) and performs the generation automatically after a push!

You can find more detailed install and configuration instructions at the [Jekyll site](http://jekyllrb.com/), or take a look a [my repo](http://www.github.com/ksornberger/ksornberger.github.com/) for the code for this blog. 

It's a good idea to [install Jekyll](https://github.com/mojombo/jekyll/wiki/install) on your local machine so you can preview things before you push. The easiest way to do this is via Ruby Gems:
```
gem install jekyll
```	

You can find more information on Basic [Jekyll Usage here](https://github.com/mojombo/jekyll/wiki/usage), but a typical site is typically structured as follows:
```
.
|-- _config.yml
|-- _layouts
|   |-- default.html
|   `-- post.html
|-- _posts
|   |-- 2007-10-29-why-every-programmer-should-play-nethack.textile
|   `-- 2009-04-26-barcamp-boston-4-roundup.textile
|-- _site
`-- index.html
```

To create our basic blog, create the file _layouts/default.html.
```html
<!DOCTYPE html>
<html>
	<head>
		<title>{{ page.title }}</title>
	</head>

	<body>
		<h1>{{ page.title }}</h1>
		{{ content }}
	</body>
</html>
```	
And update index.html to:
```
---
layout: default
title: Kevin Sornberger's Blog
---

Welcome to my site!
```	
	
The stuff at the top of that file lets Jekyll know that we want this file processed with the template named *default*, and we are setting the variable *page.title* to use in that template. The page will be output in the template where we have the *{{ content }} tag.

Now that we have our templates and content set up, we can run the following command from the site root to build the static pages. The generated output will be put into a _site subdirectory.
```
jekyll --pygments
```	
Now all you need is a simple git commit and push to update your live site. Every time you push to GitHub it will run jekyll --pygments before publishing. Because of this, it is probably a good idea to add your _site folder to your .gitignore file.


## Creating Your Jekyll Powered Blog
Another nice thing about Jekyll is the basic support for a blog layout. It relies on a naming convention where all of your blog posts go in a the *_posts* subdirectory and each post follows the naming format: *year-month-day-title-of-the-post.format* where the *format* can be either html, markdown or textile depending on what you prefer.

For your first post, create the file: *posts/2011-03-01-first-post.markdown*
```
---
layout: default
title: First Post
---

This is my first post. Hoo-Raw.
```

Now we can update our index page to list the titles of our blog posts and provide a link to the full post (I've included it as a screen shot because I was having issues with escaping the Liquid code as Jekyll was trying to parse it):

<img src="/images/index.html-screenshot.png" alt="Jekyll Blog Index Html" title="The code snippet for the index of your Jekyll Powered blog"/>


Although this is a very simple layout, you can extend these concepts and actually create a pretty substantial site. 

## Custom Domains
Another nice thing about working with GitHub Pages is that you can add your own custom domain for your site instead of using username.github.com. All you need to do is create a file called CNAME in your site's root and put the desired domain there that you will be using. For example, CNAME for this site only contains:
	ksornberger.com
	
After adding that file, you will need to modify your DNS settings by adding A records for the following IP addresses:
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

More information around configuring a custom domain can be found [Here](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)

## Additional resources
- [Jekyll Home](https://github.com/mojombo/jekyll) - Covers more features and configuration options for Jekyll. Make sure to look at the [usage page](http://wiki.github.com/mojombo/jekyll/usage) for a good overview of everything as well as a list of [Jekyll powered sites](https://github.com/mojombo/jekyll/wiki/sites), many of which provide their source code to get an idea of how to do things on your own site.

- [GitHub Pages](http://pages.github.com/) - An explanation on how to setup your personal GitHub page as well as how to setup pages for your projects.

- [Liquid Templating Language](http://www.liquidmarkup.org/) - The Ruby templating engine used by Jekyll.
