# splitcss-webpack-plugin

Divides main css file into smaller files and allows main css to import smaller css files - for IE restrictions on css files size - using [webpack] and [postcss].

## Installation

```sh
npm install splitcss-webpack-plugin -D
```

## Usage

Add an instance of `CSSSplitWebpackPlugin` to your list of plugins in your webpack configuration file _after_ `ExtractTextPlugin`.

```javascript
var ExtractTextPlugin = require('extract-text-webpack-plugin');
var CSSSplitWebpackPlugin = require('../').default;

module.exports = {
  entry: './index.js',
  context: __dirname,
  output: {
    path: __dirname + '/dist',
    publicPath: '/foo',
    filename: 'bundle.js',
  },
  module: {
    loaders: [{
      test: /\.css$/,
      loader: ExtractTextPlugin.extract('style-loader', 'css-loader'),
    }],
  },
  plugins: [
    new ExtractTextPlugin('styles.css'),
    new CSSSplitWebpackPlugin({size: 4000}),
  ],
};
```

The following configuration options are available:

**size**: `default: 4000` The maximum number of CSS rules allowed in a single file. To make things work with IE this value should be somewhere around `4000`.

**imports**: `default: false` If you originally built your app to only ever consider using one CSS file then this flag is for you. It creates an additional CSS file that imports all of the split files. You pass `true` to turn this feature on, or a string with the name you'd like the generated file to have.

**filename**: `default: "[name]-[part].[ext]"` Control how the split files have their names generated. The default uses the parent's filename and extension, but adds in the part number.

**preserve**: `default: false`. Keep the original unsplit file as well. Sometimes this is desirable if you want to target a specific browser (IE) with the split files and then serve the unsplit ones to everyone else.
