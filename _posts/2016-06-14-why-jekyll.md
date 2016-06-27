---
layout: post
title: Why Jekyll?
comments: true
---



> A brief overview on why I chose Jekyll for this blog. I go over a quick install and compare the process to a few other static and dynamic blog generators.

## My Blog Requirements
* Lite Weight
* Display Code
* Easy Quick Template Engine
* Install Anywhere
* Great Documentation
* Easily Configurable

### [WordPress](https://wordpress.com/)

  >I work with WordPress on a daily basis for work so my initial thought was to kick up the famous WordPress five minute install. With that said I felt like WordPress would be a bit overkill for my requirements.

#### - Weight

  While WordPress is deeply rooted in blogging it has evolved into one of the most widely used CMS's. According to [W3, Wordpress makes 26.4% of all websites, and 59.5% of all CMS's](https://w3techs.com/technologies/overview/content_management/all). While there isn't anything wrong with that, and it is the platform I work with daily -- it does feel bloated. I'd like complete flexibility with the front-end and often times getting involved with templates isn't an option but a requirement when using WordPress. WordPress [weighs in at 8.0 MB](https://wordpress.org/download/) zipped, but soon climbs with the installation of a template and all the plugins necessary to get up and running.

#### - Templating Engine

  I really wanted to use something like [Markdown](http://whatismarkdown.com/), [Handlebars](http://handlebarsjs.com/), or [Jade](http://jade-lang.com/). While these all have different uses (maybe I'll write another article about them) -- it really came down to being able leverage any text-editor, or learn something new. Now while Wordpress does offer markdown ability within posts, I would have to work through their interface, and not through my ext editor.

#### - Displaying Code

  When looking at how to display code in WordPress I did find [documentation](https://codex.wordpress.org/Writing_Code_in_Your_Posts) for it. But it really lacked what I found necessary out of the box especially in terms of style. While I could use [Gists](https://help.github.com/articles/about-gists/) -- this isn't a native feature for WordPress would require more work than I would like.

#### - Install Anywhere

  This is something that works out really well with WordPress. [Nearly everywhere](https://wordpress.org/hosting/) offers WordPress specific installations. It's more than easy to get an installation up and going.

#### - Great Documentation

  Did I mention that WordPress is installed for 26.4% of all Websites? There is a lot of documentation out there for it. The [WordPress docs](https://codex.wordpress.org/) are really well maintained and are quite thorough. With that said -- I've run into a lot of really bad advice for WordPress mainly because a lot of people use it.

#### - Configurability

  While there are a lot of [configurable options](https://codex.wordpress.org/Editing_wp-config.php) that come stock with WordPress it always seems like they get a bit out of hand really, and once you add in plugins there seems to be no end insight. This may be a problem for people like me, and not so much for others. A lot of times simpler is better.

### [Ghost](https://ghost.org/)

  >Ghost was founded by [John O'Nolan -- former deputy lead for the WordPress](https://en.wikipedia.org/wiki/Ghost_(blogging_platform)#History) after being frustrated with the complexity of WordPress as a blogging platform. Ghost grabbed my interest because of it's blog-focused philosophy and being built on [NodeJs](https://nodejs.org/en/).

#### - Weight

  Ghost lists it's [unzipped download at 4 MB](https://ghost.org/developers/) half the size of WordPress. While Ghost at first glance looks like WordPress clone, Ghost has entire [page disproving](https://ghost.org/vs/wordpress/) that. The page says that Ghost is 1,900% faster than WordPress. I'm not sure if this is from the codebase or NodeJs.

#### - Templating Engine

  Ghost uses [Handlebars](http://handlebarsjs.com/) for templating. I've used Handlebars before and quite like it.
  It's really easy to pass the scope around with Handlebars. Handlebars does a really great job of separating the view from the logic so testing is easy. With that said -- I'm just trying to blog here, and would like to leave all that stuff for the tutorials I'll write.

#### - Displaying Code

  Ghost does offer [Markdown](http://support.ghost.org/markdown-guide/) natively. So displaying code is a really easy using the `code element`. With that said it didn't seem to support proper [syntax highlighting](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet#code) out of the box. Just like WordPress displaying code looked like more work than I wanted it to be.

#### - Install Anywhere

  Installing would be a breeze -- again a huge fan of NodeJs. The docs for it are [here](http://support.ghost.org/installation/) if you're going to install it on Linux. A [lot of hosts](http://www.hostingadvice.com/blog/where-to-find-free-node-js-hosting/) today offer NodeJs and [npm](https://www.npmjs.com/) from the get-go. If you're familiar with using you'll appreciate the ease of using `npm install` and `package.json`.

#### - Great Documentation

  While the [docs] (http://support.ghost.org/developers) for ghost look great -- on stack overflow their are only [244 questions](http://stackoverflow.com/questions/tagged/ghost-blog) tagged for this platform. Compared to Jekyll's [2,7433 questions](http://stackoverflow.com/questions/tagged/jekyll)

#### - Configurability

  Ghost was built with simplicity in mind which is really great when you're looking for a no fuss blog. [Configuration](http://support.ghost.org/config/) is `JSON` based which is great. I really like the configuration based on key pairs and would ultimately make configuring things like SSL a cinch. With that said this isn't unique to Ghost, and really is common with many node platforms.

### [Harp](https://harpjs.com/)

  >With Harp we have another Node based blog, but it focuses more on serving up static files rather than dynamic ones. It's big push is being able to preprocess nearly anything by default.

#### - Weight

  While there isn't anything officially printed about Harp's size. When I downloaded the zip from [Harp's github](https://github.com/sintaxi/harp) it weighed in at 72 KB. Substantially less large than both WordPress and Ghost. With that said this is before packages have been installed, but hardly ever are they large at all.  

#### - Templating Engine

  With Harp's preprocessing nature it is templates in three different ways out of the box. You have a choice of [Markdown](https://harpjs.com/docs/development/markdown), [ejs](https://harpjs.com/docs/development/ejs), and [Jade](https://harpjs.com/docs/development/jade). From digging through some of the commits it looks like the project is heading towards adding whatever Markup language you'd like via packaging.

#### - Displaying Code

  As previously mentioned Harp allows for three different templating engines. Rather than get into too many details over how each work I'll just list their documentation here:
    * [Markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet#code)
    * ~~ejs~~ *
    * [Jade - from stackoverflow](http://stackoverflow.com/questions/20517743/using-the-code-or-pre-tags-in-jade-templates)

  Since ejs doesn't do it by default it isn't difficult to incorporate [Gists](https://help.github.com/articles/about-gists/).


#### - Install Anywhere

  Harp's [install](https://harpjs.com/docs/environment/install) is really easy. I'll even display it here, it does require 'NodeJs' and 'npm'. Here are the [docs](https://nodejs.org/en/download/package-manager/) if you don't have them already.

  * This is for OS X and Linux:

  `sudo npm install -g harp`

  * Then initialize with:

  `harp init yourProject`

  * Then serve your project:

  `harp serve yourProject`

#### - Great Documentation

  While Harp's [documentation](https://harpjs.com/docs/) isn't super verbose, it is extremely light and hackable. You can add just about anything to it since it's propped up mainly by `npm`. There is even a tidy section about using Harp as a [middle ware asset pipe-line](https://harpjs.com/docs/environment/lib) for [Express](http://expressjs.com/) which may be the most popular framework for NodeJs.

#### - Configurability

  As mentioned above Harp's loose and tidy framework makes configuring it easy without the possibility of too many clashes. It's great for someone who wants to dig in, but it may require a little too much leg work for someone who just wants to get a blog up and going.  

### [Jekyll](https://jekyllrb.com/)

  > Jekyll is another static site builder, but this time it is very blog driven. Their tagline is "Transform your plain text into static websites and blogs."

#### - Weight

  Due to Jekyll being a plain text based static site generator, it is incredibly lite-weight and has great speed. And like I've said previously I'm not looking for anything big do to that fact that -- it's just a blog.

#### - Displaying Code

  Displaying code is really easy with Jekyll, here are the [docs](https://jekyllrb.com/docs/posts/) and an example:


  {% highlight ruby %}
    def show
      @widget = Widget(params[:id])
      respond_to do |format|
        format.html # show.html.erb
        format.json { render json: @widget }
      end
    end
  {% endhighlight %}


#### - Templating Engine

  Jekyll uses the [Liquid Templating Engine](https://shopify.github.io/liquid/), which was created and open sourced by [Shopify](https://www.shopify.com/). While I haven't ever used Liquid before -- the [docs](https://github.com/Shopify/liquid), the about [page](https://github.com/Shopify/liquid), and Jekyll's [templating docs](https://jekyllrb.com/docs/templates/) all point to it being fairly easy to use. If you've ever used something like [Mustache](https://mustache.github.io/) then it won't take long to feel at home.

#### - Install Anywhere

  Again Jekyll has great documentation and has a section just for [deplyment](https://jekyllrb.com/docs/deployment-methods/). I will probably either push it to an [EC2](https://github.com/laurilehmijoki/s3_website) instance, or to [Heroku](http://andycroll.com/ruby/serving-a-jekyll-blog-using-heroku/). Both will be cheap options as I do not expect much traffic, and the site is static. [GitHub pages](https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/) also offers a really simple way to deploy your Jekyll site with free hosting. It's a common solution for many projects to provide more extensive information than a readme could provide.

#### - Great Documentation

  As I've already referenced the [docs](https://jekyllrb.com/docs/home/) multiple times you probably know how thorough the documentation is. There are [2,746 questions](http://stackoverflow.com/questions/tagged/jekyll) on stackoverflow covering Jekyll.   

#### - Configurability

  [Jekyll configuration](https://jekyllrb.com/docs/configuration/) relies on a .yaml file for configuration. Most of the configuration options are back-end preferences. This is something I really liked, because I really want a simple blog. I wanted something that wouldn't get between me and doing the things I like to do.

## Conclusion

  There are many options available for blogs -- and many different preferences can be met. Ultimately I chose Jekyll because it met all of my requirements. Who know though, maybe I'll end up over-engineering this thing using [React](https://facebook.github.io/react/) + [Redux](http://redux.js.org/) + NodeJs -- and making my own CMS. But for now Jekyll is perfect, and getting used to it will make it easier to GitHub Pages for any future projects of mine.  
