## Index

- [Installation](#installation)
- [Some Url about Next.JS](#some-url-about-nextjs)
- [About Structure](#about-structure)
- [About CSS](#about-css)
- [About Language System](#about-language-system)
- [About Icons](#about-icons)
- [Creating New Page](#creating-new-page)
- [Setting up Your Environment Variables](#setting-up-your-environment-variables)

## Installation

    git clone https://github.com/mucahidyazar/webpack-boilerplate.git
    npm install -D webpack webpack-cli
    npm install .
    npm run dev

## Some Url about webpack

- [webpack](https://webpack.js.org/)
- [Documentation](https://webpack.js.org/concepts/)

## About Structure

- public => keep css, font, img and sass files inside this folder
- src => keep html files inside this folder

## About Icons

- public/icon => main icon folder
- [Icomoon](https://icomoon.io/app) You should import your icon files on there and generate your new files to add there again

## Creating New Page

- Create a new html file inside the src folder. And describe it in the webpack.common.js

For example

```js
  new HtmlWebpackPlugin({
    title: "webpack Boilerplate",
    favicon: paths.public + "/img/favicon.png",
    template: paths.src + "/about.html", // template file
    filename: "about.html", // output file
  }),
```

# ABOUT WEBPACK

#### webpack.common.js

You can describe your common settings for webpack in here

#### webpack.dev.js

You can describe your development settings for webpack in here

#### webpack.prod.js

You can describe your production settings for webpack in here

## webpack settings

All config files are inside the config folder on root

### paths.js

Burada webpack.common.js, webpack.dev.js ve webpack.prod.js'de kullanilmak uzere 'root'daki 'src', 'public' ve 'build' klasorlerini path modulu ile tanimladik.

### webpack.common.js

Burada webpack ve babel ile islenecek ve daha sonra html dosyamizin icine eklenecek Javascript dosyasinin yolunu belirliyoruz

```js
  entry: [paths.public + "/js/index.js"],
```

Buradada webpack ve babel'imizin dosyalari isleyip cikti verecegi yeri belirliyoruz.

```js
  output: {
    path: paths.build,
    filename: "[name].bundle.js",
    publicPath: "/",
  },
```

Buradada webpack plugin'lerimizi ayarliyoruz.

```js
  plugins: [...]
```

Burada da bir htm sayfasini HtmlWebpack Plugini ile eklemeyi yapiyoruz.

```cmd
  npm i -D html-webpack-plugin
```

```js
const HtmlWebpackPlugin = require("html-webpack-plugin");

plugins: [
  new HtmlWebpackPlugin({
    title: "webpack Boilerplate",
    favicon: paths.public + "/img/favicon.png",
    template: paths.src + "/template.html", // template file
    filename: "index.html", // output file
  }),
];
```

title = kismindan sayfalarimizin title'larini ayarlayabiliriz fakat src de yazdigimiz html'lerin title kismi asagidaki gibi olmalidir

```html
<title><%= htmlWebpackPlugin.options.title %></title>
```
