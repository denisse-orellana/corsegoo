# Notes of the course

## Git reset
To revert any changes on the last comment
```console
git clean -fd
git reset --hard
```
Go to the previous comment:
```console
git reset HEAD^ --hard
git push -f
```
Undone the last action (delete commit):
```console
git reset --soft HEAD~
git push -f
```

## Bootstrap: Yarn and webpacker
1. console:
```console
yarn add jquery popper.js bootstrap
mkdir app/javascript/stylesheets
echo > app/javascript/stylesheets/application.scss
```
2. environment.js:
```console
const { environment } = require('@rails/webpacker')
const webpack = require("webpack")
environment.plugins.append("Provide", new webpack.ProvidePlugin({
    $: 'jquery',
    jQuery: 'jquery',
    'window.jQuery': 'jquery',
    Popper: ['popper.js', 'default']
  }))
module.exports = environment
```
3. add (not replace) in application.html.erb:
```console
= stylesheet_pack_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' 
```
4. application.js:
```console
import 'bootstrap/dist/js/bootstrap'
import 'bootstrap/dist/css/bootstrap'
require("stylesheets/application.scss")
```
5. add @popperjs/core
```console
yarn add @popperjs/core
```