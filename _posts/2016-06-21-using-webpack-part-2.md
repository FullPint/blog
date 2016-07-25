---
layout: post
title: Using Webpack Part 2
comments: true
---

##Getting More

Here, in Part 2 we will first  refactor our original code to use [React](https://facebook.github.io/react/). While we are there we will take a brief opportunity to add a little more function to our page, and expose some of what React has to offer.

After the React bit, we will dive back into Webpack itself and try to leverage some more complex uses of the package. This type of stuff should be useful once you're more comfortable using Webpack.

<!--more-->

If you just joined us -- this is Using Webpack Part 2, and that  means there is a [Using Webpack part 1](http://alecdavila.me/2016/06/20/using-webpack-part-1). There is also the final commit for [part 1 here](https://github.com/FullPint/webpack_demo_part-1/commit/061af1fa70f1a8d9974c1426b84fbf946fd35f33). This will get you caught up to where we are currently. Part 1 was largely how to setup a quick project and it was also an attempt to show just how easy Webpack is to use.

###Refactoring

Let's go ahead and start with installing both [React](https://www.npmjs.com/package/react) and [ReactDOM](https://www.npmjs.com/package/react-dom). You will use `npm install --save react react-dom`. This will give us the React core library, and the React DOM library that grants us an entry point to the DOM itself.

If you're unfamiliar with React please check out the [docs](https://facebook.github.io/react/). I will at some point in the near future right an article or two on React, and go into much further detail. React uses [JSX](https://facebook.github.io/react/docs/jsx-in-depth.html) to render the view. While it may look a little alien if you've never used it -- the statically typed syntax is easy to pick up.

Let's go ahead and refactor `index.js`.

{%gist ef379d26f6bae1d561a618d8fa87b8a1/4570d4468ebf05c67b70aa23bf1fd6d0b0f51e9c %}

Here we used [import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) -- for both React and React DOM -- instead of the familiar `require()` statement. This is part of ES6 syntax and under the hood it is essentially the same thing. ES6 is the standard, so we will go ahead an use it.

Then a React Component is created using ES6 class syntax. We call `render()` to build our view. Since `class` is reserved for designating classes in ES6, we use `className` for styling instead. The we 'addwhat ' our `Component` to the `DOM` using `React.Render()` -- the first param is our React Element, and the second is the node we'd like to place it.

Now this won't render anything when we run `npm-start` for our `webpack-dev-server`. We need some libraries to compile ES6 - React - JSX into pure Javascript. You will need to install [babel](https://babeljs.io/), its loader and few presets. `npm install --save-dev babel-core babel-loader babel-preset-es2015 babel-preset-react babel-preset-stage-1`.

After those have been installed we need to update the `webpack.config.js` to use  babel, and create `babel.rc` to load it's own presets. When setting the loader for react we will exclude the `npm modules` from our regex so they're not incorporated into the bundle file.

{% gist 0de42cb222577f4499d8cc4a180a503f/845f38c6b56a7a42a1766ec9a9dfd55e62d8800d %}
{% gist b9597ada271145526112f818b80e550a/276db39d7e46cd2317b1575aa80ac52fad7ab88f %}

Now when we run `npm start` everything should work fine. But our page isn't quite the same as it was before -- so we need to refactor our code a little bit more.

Let's refactor child to parent -- starting with `nowMessage.js` and ending with `index.js`.

{% gist 0d198e21c382f9a9e979207fda8e7ed5/bd6c3ac7a7c3be56053bab46ca262931dbab8325 %}

`React` is imported, but not `React DOM` because this will just be a child component. Inside our component, we call the `constructor` to initialize our props for setting `this.now` as we did before. Then we call `render()`as before -- placing `this.now` in our view.

{% gist aa767cfec2e95a044a3ec5148550c6f8/fbbfc40e569e4afc2cd23d8d6dcb6c09c7f432f3 %}

This one is a little simpler -- just importing  `NowMessage`, and placing it within our view as `<NowMessage />`. And again `export default HelloTest`.

{% gist ef379d26f6bae1d561a618d8fa87b8a1/3b0c2c52f2a1a544bc816960509c3091ce583ad8 %}

Here we just import `HelloTest` and place it into the view as `<HelloTest />`. We don't have to place `<NowMessage />` as it is a child of `HelloTest`. We did update some of the classNames so you may want to update 'style.css' as below.

{% gist 91771f806ed95384ea49632cd4cd0bbb/332995b393be9c6cad6b9e09d70fb1cd3289fb87 %}

###Adding Something Extra

I really don't like that our `nowMessage.js` only shows the time when the page was called. I would like our display to show the user their current time. Updating this module gives me a chance to expose another method in `React`.

{% gist 0d198e21c382f9a9e979207fda8e7ed5/5b905b036bcd85df76d24d13fb559e90fdf0516b %}

We use [`componentDidMount()`](https://facebook.github.io/react/docs/component-specs.html#mounting-componentdidmount) method to call [setInterval](http://www.w3schools.com/jsref/met_win_setinterval.asp) when the initial rendering occurs. It's one of many lifecycle methods available in React. We use the `setInterval()` method along with the [arrow function ](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) allowing us to keep our `lexical this` within the block. With our interval set to every second, we will update `this.now` every second.

###Back to WebPack

Webpack is full of really cool configurations. Often times in our attempt to make our applications more modular we end up crowding the top of a document with tons of `import` statements. What's even more irritating is continually having to import the same stuff over - and - over again. An example would be: `import React from 'react'` at the top of every single file. With Webpack we can expose `React from 'react'`globally, so we can say goodbye to the annoying statement.

From the [docs](https://webpack.github.io/docs/list-of-plugins.html#provideplugin) the config uses a key-value pair. The key being our identifier, and value being our module. This could be used for anything -- the docs actually use jQuery as their example.

{% highlight javascript %}
//Webpack Doc Example:
new webpack.ProvidePlugin({
    $: "jquery"
})

// in a module
$("#item") // <= just works
// $ is automatically set to the exports of module "jquery"

{% endhighlight %}

We now need to update our webpack.config.js. Remember to restart your dev-server any time you update the config.

{% gist  0de42cb222577f4499d8cc4a180a503f/3adf8c24b698bf20f2bf67ee8bd87a3f3b5d1e34 %}

So now we can remove `import React from 'react'` and `import ReactDOM from react-dom` from our code.

{% gist 0d198e21c382f9a9e979207fda8e7ed5/d0e412f1768204b9af7f9b844c76a4005adfe5f3 %}
{% gist aa767cfec2e95a044a3ec5148550c6f8/f7b86241b74607bf97de6c232f07d3aa45e445c1 %}
{% gist ef379d26f6bae1d561a618d8fa87b8a1/47f0c0d3b8ef08dc1b1fe8f84f0ac217c476d0db %}

###More Plugins
There are a lot of really cool plugins, but many of them don't fit in with the scope of our little tutorial. So from this point forward we will just use gists as examples -- and you can apply them anyway you'd like in your own projects

The [UglifyJsPlugin](https://webpack.github.io/docs/list-of-plugins.html#uglifyjsplugin) will minimize all of our chunks. And it accepts all uglifyJs [options](https://github.com/mishoo/UglifyJS2#usage). In our example you can see that we avoid mangling important key strings -- like '$' for angular, or 'exports' like we've been using all along.

{% gist 45f67b6ef7a6c259dae2d7627e2c1e20 %}

The [S3Plugin](https://github.com/MikaAK/s3-plugin-webpack) is really cool as it allows you to easily push to S3. This blog actually sits on S3. It's not part of the Webpack core -- so you will use npm to install it.

The [CommonsChunkPlugin](https://webpack.github.io/docs/list-of-plugins.html#commonschunkplugin) sound exactly like what it is. This will greatly improve performance for Multi-Page applications.

{% gist 652b3a2d78a27cda2ebae3257fc291dc/b01821e47804c41032c1f7fc92d12dfcc4f3fc3f %}

The code above shoves what we define and place vendor files into their own chunk, and then any module that is used more than 3 times into another chunk. This is great for Multi-Page applications as the more common scripts are now being cached and readily available.

##Conclusion

We've showed how Webpack can be used with frameworks like [React](https://facebook.github.io/react/), or a compiler like [BabelJs](https://babeljs.io/). We've shown how simple it is to configure -- and how it "just works." All of this configuration gives a real chance to focus on our work and use best-practices. We will continue to use this package for many of our future projects.
