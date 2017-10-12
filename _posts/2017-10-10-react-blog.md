---
layout: post
title:  "React blog"
date:   2017-10-09 10:00:00
categories: react
tags: [react, reactjs, webpack, jsx, javascript]
---

## Initialize npm project and install dependencies

### Install nvm

We're going to use node 8.x

**For Unix**

{% highlight bash %}
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.5/install.sh | bash
{% endhighlight %}

Set the env var

{% highlight bash %}

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" # This loads nvm

{% endhighlight %}

#### Install node 8

{% highlight bash %}

nvm install 8.3.0

{% endhighlight %}

#### Create a project folder and `.nvmrc` file inside the folder

{% highlight javascript %}
mkdir my-react-blog
{% endhighlight %}


{% highlight bash %}
touch .nvmrc
{% endhighlight %}

Inside this file, we're going to put

{% highlight bash %}
8.3.0
{% endhighlight %}

and let's use the installed version of node

{% highlight bash %}
nvm use 8.3.0
{% endhighlight %}

#### Let's install Yarn as our node package management

{% highlight bash %}
npm i -g yarn
{% endhighlight %}

#### Initialize npm project

{% highlight bash %}
yarn init
{% endhighlight %}

Just fill it with your data!

#### Install dependencies for production

{% highlight bash %}
yarn add isomorphic-fetch prop-types react react-dom react-router
{% endhighlight %}

#### Install dependencies for ES2015 preset transpiling and compiling

{% highlight bash %}
yarn add -D babel-core babel-polyfill babel-preset-es2015 babel-preset-react babel-preset-stage-0 node-sass
{% endhighlight %}

#### Install dependencies for webpack and loaders

{% highlight bash %}
yarn add -D webpack webpack-dev-server babel-loader css-loader file-loader html-loader html-webpack-plugin react-hot-loader react-transform-hmr sass-loader style-loader url-loader
{% endhighlight %}

## Initial setup / startup files

#### Create the file `app/index.jsx` with

{% highlight js %}
import React from 'react';
import { render } from 'react-dom';
import style from './style.css';

const App = ({ header }) => (
  <div>
    <h1>{header}</h1>
    <p>By Juan Roa from DevHack</p>
  </div>
);

render(<App header="Hello React!" />, document.getElementById('app'));
{% endhighlight %}

If we want to return a list of items in React Fiber (16.x), just return an array of elements instead of enclosing tags like divs.

{% highlight js %}
return [
  <li key="A">List element/component 1</li>,
  <li key="B">List element/component 2</li>,
];

{% endhighlight %}

#### Let's apply a basic style and create `app/style.css` with

{% highlight css %}
h1 {
  color: red;
}
{% endhighlight %}

#### Create the file `assets/index.template.html` with

{% highlight html %}
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>React Blog</title>
  </head>
  <body>
  <div id="app"></div>
  </body>
</html>
{% endhighlight %}

#### Create the file `webpack.config.js` with

{% highlight js %}
const webpack = require('webpack');
const autoprefixer = require('autoprefixer');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  devtool: 'eval-source-map',

  entry: [
    'react-hot-loader/patch',
    // activate HMR for React

    'webpack-dev-server/client?http://localhost:8080',
    // bundle the client for webpack-dev-server
    // and connect to the provided endpoint

    'webpack/hot/only-dev-server',

    'babel-polyfill',

    `${__dirname}/app/index.jsx`,
  ],
  output: {
    path: `${__dirname}/build`,
    filename: 'bundle.js',
  },

  resolve: {
    extensions: ['.jsx', '.scss', '.js'],
  },

  module: {
    rules: [
      {
        test: /\.html$/,
        loader: 'html-loader',
      },
      {
        test: /\.js|jsx?$/,
        use: [
          {
            loader: 'babel-loader',
            options:
            {
              presets: ['es2015', 'react', 'stage-0'],
              env: {
                development: {
                  plugins: ['react-hot-loader/babel'],
                },
              },
            },
          },
        ],
        exclude: /node_modules/,
      },
      {
        test: /(\.scss|\.css)$/,
        use: [
          { loader: 'style-loader' },
          {
            loader: 'css-loader',
            options:
            {
              sourceMap: true,
              modules: true,
              importLoaders: 1,
              localIdentName: '[name]__[local]___[hash:base64:5]',
            },
          },
        ],
      },
    ],
  },

  plugins: [
    new HtmlWebpackPlugin({
      template: `${__dirname}/assets/index.template.html`,
    }),
    new webpack.NamedModulesPlugin(),
    // prints more readable module names in the browser console on HMR updates
    new webpack.NoEmitOnErrorsPlugin(),
    // do not emit compiled assets that include errors
    new webpack.HotModuleReplacementPlugin(),
    new webpack.LoaderOptionsPlugin({
      debug: true,
    }),
  ],

  devServer: {
    host: 'localhost',
    port: 8080,
    historyApiFallback: true,
    inline: true,
    hot: true,
  },
};

{% endhighlight %}

Now, we're able to see our first React app running and displaying `Hello React` with webpack + hot reloading. We are also able to see our changes in the browser without reloading it manually.
