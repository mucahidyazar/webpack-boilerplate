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

### PLUGINS

Buradada webpack plugin'lerimizi ayarliyoruz.

```js
  plugins: [...]
```

#### html-webpack-plugin

```cmd
  npm i -D html-webpack-plugin
```

Bu package ile pluginslerimizde asagidaki kod bloguyla html sayfasi ekliyoruz.

```js
const HtmlWebpackPlugin = require("html-webpack-plugin");

plugins: [
  new HtmlWebpackPlugin({
    title: "webpack Boilerplate",
    favicon: paths.public + "/img/favicon.png",
    template: paths.src + "/template.html", // template file
    filename: "index.html", // output file
  }),
  ...
];
```

title = kismindan sayfalarimizin title'larini ayarlayabiliriz fakat src de yazdigimiz html'lerin title kismi asagidaki gibi olmalidir

```html
<title><%= htmlWebpackPlugin.options.title %></title>
```

#### clean-webpack-plugin

```cmd
  npm i -D clean-webpack-plugin
```

Bu plugin kullanilmayan assetsleri yeniden olusturdugunda siler/temizler
Removes/cleans build folders and unused assets when rebuilding

```js
const CleanWebpackPlugin = clean-webpack-plugin");

plugins: [
  ...,
  new CleanWebpackPlugin(),
];
```

#### clean-webpack-plugin

```cmd
  npm i -D clean-webpack-plugin
```

from 'daki belirtilen yoldaki dosyalari to'daki hedef klasorune kopyalar
Copies files from target to destination folder

```js
const CopyWebpackPlugin = require("copy-webpack-plugin");

plugins: [
  ...,

  new CopyWebpackPlugin({
    patterns: [
      {
        from: paths.public,
        to: "assets",
        globOptions: {
          ignore: ["*.DS_Store"],
        },
      },
    ],
  }),
];
```

### MODULES

Buradada webpack icin modullerimizi ve kurallarimizi ayarliyoruz.

```js
modules: [(rules: { ... })];
```

#### babel-loader

```cmd
npm i -D babel-loader @babel/core @babel/preset-env @babel/preset-env @babel/plugin-proposal-class-properties
```

Babel ayarlarimizi webpack ile ayarlariz. Detayli ayarlamalar icin documentation'a bakiniz.
JavaScript: Use Babel to transpile JavaScript files

```js
  modules: [{
    rules: {
      ...,
      { test: /\.js$/, exclude: /node_modules/, use: ["babel-loader"] },
    }
  }];
```

#### STYLES

Once asagida ki gerekli package'leri yukluyoruz

```cmd
npm i -D css-loader style-loader postcss-loader postcss-preset-env node-sass sass-loader less less-loader
```

test = ile hangi css uzantılarinı tanıması gerektigini soyledik
use = icinde hangi loader'ların kullanılacaklarını belirttik
css-loader: css dosyalarını anlayıp çözümleyebilmek için kullanılır
style-loader: dom içine css-loader ile çözümlenen css dosyalarını ekler

NOT: use kısmında kullanılan style ve css loader’ların sıralaması çok önemlidir. Aslında buradaki sıralama loader kuralları gereği ters çalışmaktadır. Yani önce css-loader çalışmaktadır. Bunun sebebi ise önce css-loader ile css dosyaları çözümlenir daha sonra bu çözümlenen dosyalar style-loader ile dom içine enjekte edilir. Sıralama kuralı tüm loader’lar için geçerlidir css ile ilgili değildir.

```js
  modules: [{
    rules: {
      ...,
      {
        test: /\.(s?)css$/,
        use: [
          "style-loader",
          {
            loader: "css-loader",
            options: { sourceMap: true, importLoaders: 1 },
          },
          { loader: "postcss-loader", options: { sourceMap: true } },
          { loader: "sass-loader", options: { sourceMap: true } },
        ],
      },
    }
  }];
```

"postcss-loader" i use kismina eleman olarak ekliyoruz ve postcss.config.js dosyasi olsuturup asagidaki ayarlari oraya kopyaliyoruz.

```js
module.exports = {
  plugins: [require("autoprefixer")],
};
```

#### IMAGES

Images ayarlarini rules olarak webpack'e asagidaki gibi ayarladiktan sonra js dosyalarimizin icine import ederek kullanabiliriz.
Ve asagida belirttigimiz image uzantilarini outputPath belirtmedigimiz icin path.build yolu olan dist klasorunun direk icine image dosyalarimizi bundle ederek tutar.

```js
  modules: {
    rules: {
      ...,
      { test: /\.(?:ico|gif|png|jpg|jpeg)$/i, type: "asset/resource" },
    }
  };
```

#### FONTS

Fonts ayarlarini rules olarak webpack'e asagidaki gibi ayarladiktan sonra js dosyalarimizin icine import ederek kullanabiliriz.
Ve asagida belirttigimiz font uzantilarini belirledigimiz outputPath yolundaki klasorun icine font dosyalarimizi bundle ederek tutar.

```js
  modules: {
    rules: {
      ...,
      {
        test: /\.(ttf|eot|svg|gif|woff|woff2)(\?v=[0-9]\.[0-9]\.[0-9])?$/,
        use: [
          {
            loader: "file-loader",
            options: {
              outputPath: "assets/fonts/",
            },
          },
        ],
      },
    }
  };
```

```js
//Images
```
