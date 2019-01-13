# Vue JS starter cheatsheet
Untuk memudahkan saat memulai project vue baru.

  - Init project
  - Install minimum dev dependencies
  - Setting webpack.config.js
  - Add build command

### Init project

Memerlukan nodejs dan npm untuk memulai project. Untuk instalasi nodejs lihat [Nodejs](https://nodejs.org)

```sh
$ npm init
```

### Install required dev-dependencies
```
$ npm install @babel/core babel babel-core babel-loader babel-plugin-add-module-exports uglifyjs-webpack-plugin vue vue-loader vue-template-compiler webpack webpack-cli --save-dev
```

### Setting webpack.config.js
Buat file webpack.config.js di folder project. Konfigurasinya sebagai berikut
```javascript
const webpack = require('webpack');
const path = require('path');
const VueLoaderPlugin = require('vue-loader/lib/plugin');

module.exports = {
   entry: './src/index.js', //set dengan file root vue
   output: {
      path: path.resolve(__dirname, '/path/to/output'),
      publicPath: '/public/', //set sesuai nama public folder yang digunakan
      filename: 'outputName.js' //set nama output file 
   },
   module: {
      rules: [
         {
            test: /\.vue$/,
            loader: 'vue-loader',
            options: {
               loaders: {
               }
               // other vue-loader options go here
            }
         },
         {
            test: /\.js$/,
            loader: 'babel-loader',
            exclude: /node_modules/
         },
         {
            test: /\.(png|jpg|gif|svg)$/,
            loader: 'file-loader',
            options: {
               name: '[name].[ext]?[hash]'
            }
         }
      ]
   },
   plugins: [
      // make sure to include the plugin!
      new VueLoaderPlugin()
   ],
   resolve: {
      alias: {
         "vue$": "vue/dist/vue.common.js"
      }
   },
   devServer: {
      historyApiFallback: true,
      noInfo: true
   },
   performance: {
      hints: false
   },
   devtool: '#eval-source-map',
   watch: true,
   watchOptions: {
      ignored: /node_modules/
   }
}

if (process.env.NODE_ENV === 'production') {
   //module.exports.devtool = '#source-map'
   // http://vue-loader.vuejs.org/en/workflow/production.html
   module.exports.plugins = (module.exports.plugins || []).concat([
      new webpack.DefinePlugin({
         'process.env': {
            NODE_ENV: '"production"'
         }
      }),
      new webpack.optimize.UglifyJsPlugin({
         sourceMap: true,
         compress: {
            warnings: false
         }
      }),
      new webpack.LoaderOptionsPlugin({
         minimize: true
      })
   ])
}
```


### Set build command
tambahkan dua command tersebut kedalam 'script' pada package.json
```json
...
scripts:{
    ...
    "build": "node_modules/.bin/webpack --mode=development",
    "prod-build": "node_modules/.bin/webpack --mode=production"
    ...
}
...
```

command untuk menjalankan build
- untuk dev build
```sh
$ npm run build
```

- untuk production build
```
$ npm run build-prod
```
