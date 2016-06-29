---
layout: post
title: Using Webpack Part 1
comments: true
---

##Another Webpack Article

I understand that there are many Webpack articles all over the web, but I wanted to write an article we could reference in the future. This overview will be broken up into two articles.

The first article will cover how to quickly set-up a project using Webpack, and we will reference this from time to time in the future.

In the second article we will play with Webpack and hopefully this will provide a little better understanding of what can be done with the package - along with a better understanding of what is going on.

You can find the final commit for [part one here](https://github.com/FullPint/webpack_demo_part1/commit/061af1fa70f1a8d9974c1426b84fbf946fd35f33), and the link the  [part two article here](http://alecdavila.me/2016/06/21/using-webpack-part-2).

##What is Webpack?

You may have used [grunt.js](http://gruntjs.com/) or [gulp.js](http://gulpjs.com/), maybe even have used [browserify](http://browserify.org/). By now you're probably feeling the [Javascript Fatigue](https://medium.com/@ericclemmons/javascript-fatigue-48d4011b6fc4#.wb7x8f335), but I promise this is one library that kind-of removes a lot to the fatigue.

I've always found the amount of boilerplate necessary for `grunt.js` and `gulp.js` to really be their biggest issue. While it's always been great to write logic with `gulp.js`, it always ended up being a bigger - and - bigger time sink as my projects began to grow.  

Webpack has a much more hands-off attitude that may freak out some to begin with, but in the long run it becomes really beneficial. I've found that the time sink I used to experience is no longer there when working with Webpack. I now have a much easier time working directly on the project rather than fiddling with another build module.

Webpack is also great as it does bundling. Often times with many other front-end package managers we tend to see a bloated list of libraries in the `Head` of the `DOM`. Or we get these giant minified build files with everything crammed in and the our file size is still really quite giant. These large files end up slowing down our load times. Webpack is smart and offers [code splitting](http://webpack.github.io/docs/code-splitting.html). Webpack breaks your code into chunks and only provides what is necessary and on-demand.

###Starting Our Set-Up

Let's first run a few commands:
{% highlight bash %}
$ mkdir articleWebPack && cd articleWebPack
$ npm init -y
{% endhighlight %}

The `-y option` just answers yes to all of the `npm init` questions, and applies default string options wherever necessary. Your package.json should be more or less this:

{% gist e6c11ab7eafd6082e5eefcf2c4d230d7/bb996118ec6627261e032941a49904cb2475ed72 %}

Before we get moving anywhere we should probably install Webpack itself. `npm install --save-dev webpack`. Now that we have Webpack installed let's start building our directory structure. First we will had directories `src/` and `build/`.

{% highlight bash %}
$ mkdir build
$ mkdir src
{% endhighlight %}

Webpack will take what we do in `src/` and and will "bundle" it in `build/`. We are almost ready to begin working. Let's create a few of the files we need to start.

{% highlight bash %}
$ touch build/index.html
$ touch src/index.js
$ touch webpack.config.js
{% endhighlight %}

With that done your project should look something like this:

{% highlight bash %}
.
├── build
│   └── index.html
├── package.json
├── src
│   └── index.js
└── webpack.config.js
{% endhighlight %}

###Connecting Our Set-Up

Now that we have the very bare-bone basics setup we can start moving closer to using Webpack and having it build our project for us. We will configure the `webpack.config.js` file to bundle our build. We will also write some javascript to show that it does work.

First thing is first let's build our `build/index.html` file.
{% gist 8bd126b02f57bc2914e7a5788455a133 %}

Here we have a simple `.html` file. Our `div` is given the `id` app because usually I use this sort of setup for [React.js](https://facebook.github.io/react/) applications. We will be using this in React tutorials later on. And then we have a reference to `./bundle.js` which is the file that Webpack bundles for us in the build folder.

From here we will build build our `src/index.js` file. This will just add the text "Test: Hello World" to the `DOM` at `app`. While this is nothing special at the moment, we will show here pretty soon why webpack is so great.
{% gist 2c36dbf5ffd64756c8ed8fb1ceda32f3/cedb6a0e96f7260aa55f85967b1b4121404d22ee %}

And now all that is left is to build our `webpack.config.js`.  The one we will be building at the beginning is really simple -- so don't be fooled and underestimate how powerful Webpack can be. You can copy and pasted this
{% gist 8761bda303dff3e497fd888e3e0ae6fa/8734eec1a94afb6f6d7b11a716f7597785a198cd %}

Let's talk a little bit about the contents of `webpack.bundle.js`. First we declare a constant `path` to be the [path module from the NodeJs core](https://nodejs.org/api/path.html). It provides us with ways to work with paths of files and directories.  We then declare another constant `PATHS`. The `PATHS` describes the two paths to our `src/` and `build/` directories using the NodeJs variable `__dirname`. On the next line we export file giving Webpack out source directory and destination directory, along with the path for `bundle.js`.

Now you'll run this command: `node_modules/.bin/webpack`.
You should see something like this:
{% highlight bash %}
Time: 54ms
    Asset     Size  Chunks             Chunk Names
bundle.js  1.64 kB       0  [emitted]  src
   [0] ./src/index.js 240 bytes {0} [built]
{% endhighlight %}

When you open up `index.html` in a browser, everything should be there!

I'm not a huge fan of running that command all the time, and would lke to forget about it. So we will modify the `package.json` file to have it run a simpler command. From now when you need Whttps://facebook.github.io/react/ebpack to build all you will need to command is `npm build`.  
{% gist e6c11ab7eafd6082e5eefcf2c4d230d7/71f0762434a79d3aac96d61109e97895a8f2196f package.json %}

###Our Development Server

Next we will install `webpack-dev-server`  -- `$ npm install --save-dev webpack-dev-server`. This kicks up a nice [Express.js](http://expressjs.com/) server for when we are working. All of what makes this package so important is the [Hot Module Replacement](https://webpack.github.io/docs/hot-module-replacement.html) it provides. It provides live reload like functionality but only swaps out the changes rather than making a refresh. Let's also update our `package.json` to provide a command for the dev server.
{% gist e6c11ab7eafd6082e5eefcf2c4d230d7/d6d0fbcdafaeb36d70ca4475fa295b55dc95f4d7 package.json%}

Let's go ahead and run our new command for our dev server: `npm start`
You should see something like this:
{% highlight bash %}
> articleWebPack@1.0.0 start /Users/user/articleWebPack
> webpack-dev-server

http://localhost:8080/webpack-dev-server/
webpack result is served from /
content is served from /Users/user/articleWebPack
Hash: d209b45884eac0e1ebb8
Version: webpack 1.13.1
Time: 68ms
    Asset     Size  Chunks             Chunk Names
bundle.js  1.64 kB       0  [emitted]  src
chunk    {0} bundle.js (src) 240 bytes [rendered]
    [0] ./src/index.js 240 bytes {0} [built]
webpack: bundle is now VALID.
{% endhighlight %}

And now head over to where it's being served [http://localhost:8080/webpack-dev-server/](http://localhost:8080/webpack-dev-server/). You should see your "Test: Hello World". *Oops!!* You probably see something like:

![](http://alecdavila.me/public/images/2016-06-20-using-webpack/dev-server-screen-1.png "dev-server-screen")

Don't worry our server is working just as expected. It's just currently showing all of the files being served. In fact if you click `buil` it will take you to [http://localhost:8080/webpack-dev-server/build](http://localhost:8080/webpack-dev-server/build) which will show what we are expecting. Let's configure our `webpack-dev-server` so we won't have to navigate this way, and get rid of the pesky "App ready" bar.

This is our new `webpack.config.js `:
{% gist 8761bda303dff3e497fd888e3e0ae6fa/27561157668a83ddbb6a71b4e6cf0b6cf64de855 %}

We should probably talk a bit about what's going on here with our configuration. We first set `webpack` as a constant by using `require()`. This allows us to grab the `hot-module-replacement` plugin from `webpack` core.

Next we define our `devServer` options. We let the server know that our `contentBase` is at `PATHS.build` so we no longer have to specify the URL as [/webpack-dev-server/build](http://localhost:8080/webpack-dev-server/build), and let the base be at [webpack-dev-server/](http://localhost:8080/webpack-dev-server/).

We then set `historyApiFallback` to `true`. This Will give us a fallback for the [Web History Api](https://developer.mozilla.org/en-US/docs/Web/API/History_API) when we build our Single Page Applications later on.

Easy enough `hot` is set to `true` which adds the `Hot Module Replacement`.

Then `inline` is set to `true`. The `inline` setting is necessary for `Hot Module Replacement` --  it's what makes the refresh possible. Together only the modules that are changed will refresh.

We won't be able to see what's going on with our build since we won't be calling `build` every-time we save, since the `Hot Module Replacement` handles it. By setting `progress` to `true` we get to see the compilation in action.

###Refactoring Index.js

Now here is another piece of what makes Webpack so great. We have already showed how to pull in npm modules using Common JS via `require()`. We can leverage this when building our front-end and Webpack will have our bundler compile what is necessary.

Let's refactor our `index.js` and we will split it into two. The second one being `helloTest.js`.

{% gist 38ca5f1a3cce0eaf6b7bb56ab17d0f83/1b1d0188427814ba1a74587d11be8f60c45e70ab %}

And our new `index.js` will `require()` our `helloTest.js`.

{% gist 2c36dbf5ffd64756c8ed8fb1ceda32f3/0d294d69f2eb28e63329617d81cd9bff73e2a572 %}

Now when we run `npm start` our dev server will begin, and you can now go back to [http://localhost/8080](`http://localhost/8080`), and out "Test: Hello World" should show up. To see if `Hot Module Replacement` is working, why don't we write another function that `helloTest.js` will `require()`. Let's have it display the time of when the page is opened. To do this create file `nowMessage.js` and write the following inside of it:

{% gist 1ad87785d00baf181bc4ebbb2be72faf/75ba0428087ed114c7685c93ead4b2acfa835263 %}

If you look into the browser nothing has changed, but that's only because we haven't applied our new function. Go ahead and change your `helloTest.js` to look like this.

{% gist 38ca5f1a3cce0eaf6b7bb56ab17d0f83/778c22f147421fd3ebd2be6e07e4141f74ed2b6e %}

###Introduction to Loaders

Now there is one more thing I would like to go over before we move on to [Part 2](http://alecdavila.me/2016/06/21/using-webpack-part-2), and it is [loaders](https://webpack.github.io/docs/using-loaders.html). According to the Webpack docs, loaders are "...transformations that are applied on a resource file ... [they take] a resource file as a parameter and return the new source." This essentially allows Webpack to learn new functionality. It can be used to transform JSX to JS in our bundles, which we will later do in other tutorials. It can be used to add CSS or SASS right into our javascript making them both more modular.

Let's begin with `webpack.config.js` and how we will add our loaders. We will only be working with CSS for the time being so we will add the `css-loader` and `style-loader` for Webpack. To add we run `npm install css-loader style-loader --save-dev`.

Good now that `css-loader` and `style-loader` are installed wee need to edit our `webpack.config.js`. We will call the new loaders, and provide some regular expressions on where to look for our CSS file.

{% gist 8761bda303dff3e497fd888e3e0ae6fa/3729179efcb79fe3a891aff36a391702c9638671 %}

In our file `test:` tells the loader what to look for and where to look for it. Here we apply a regular expression to find any CSS file. Next we define what loaders to use. Here we have chained the style, and css loader separated by an expclamation point as `style!css`.

Next quickly create that simple `style.css` file, and drop in in `src/`. Copy paste what is below.

{% gist 0c3a3c9b49006122fa386b1586acc2a5/bf81505f6804b8474e5fdf1e9b0c3bf73349c56d %}

With that done we can `require()` it inside `index.js`. And it will be applied to what is being built.

{% gist 2c36dbf5ffd64756c8ed8fb1ceda32f3/a67af6ed18a1537164c77687ae04d748ffce4cd4 %}

## Conclusion

We've gone over a really simple build set-up but hopefully you can already see the clear advantage when using Webpack. In the next article you can see how to really leverage some of our new found uses to get a better hold on a more-complex projects. The `hot-module-replacement` and streamlined build process really gives us back a lot of time from our projects.
