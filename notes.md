# Notes of the course

## Models
```console
rails g scaffold Course title description:text



```

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

## Add Bootstrap: Yarn and webpacker
1. console:
```console
yarn add jquery popper.js bootstrap
mkdir app/javascript/stylesheets
echo > app/javascript/stylesheets/application.scss
```
2. environment.js:
```js
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
```js
import 'bootstrap/dist/js/bootstrap'
import 'bootstrap/dist/css/bootstrap'
require("stylesheets/application.scss")
```
5. add @popperjs/core
```js
yarn add @popperjs/core
```
Guide: https://blog.corsego.com/rails-6-install-bootstrap-with-webpacker-tldr
Error case: https://exerror.com/module-not-found-cant-resolve-popper-js/

## Add Fontawesome
1. console:
```console
yarn add @fortawesome/fontawesome-free
```
2. javascript/packs/application.js:
```console
import "@fortawesome/fontawesome-free/css/all"
```
3. Check if it works:
```console
Add couple of icons in any .html.erb (view) file:
<i class="far fa-address-book"></i>
<i class="fab fa-github"></i>
```
Guide: https://blog.corsego.com/rails-6-install-fontawesome-with-webpacker

## Gem haml
```console
gem "haml-rails", "~> 2.0"
$ rails haml:erb2haml
```
https://haml2erb.org/
https://github.com/haml/haml-rails
http://html2haml.herokuapp.com/

## Gem simple_form
```console
gem 'simple_form'
bundle install
rails generate simple_form:install --bootstrap
```
Guide: https://github.com/heartcombo/simple_form

## Add rich text editor
```console
rails action_text:install

__application.js__
require("trix")
require("@rails/actiontext")

__actiontext.scss__
@import "trix/dist/trix";

__courses/form:__
  = f.label :description
  = f.rich_text_area :description
```

## Gem faker
```console
gem 'faker'
rails db:seed
```
https://github.com/faker-ruby/faker

## Gem devise
```ruby
gem 'devise'
bundle install
rails generate devise:install

config/environments/development.rb:
  config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }

application.html
  %p.notice= notice
  %p.alert= alert

rails generate devise User
rails db:migrate

Rails.application.routes.draw do
  devise_for :users

rails g migration addUserToCourses user:references

__seed.rb__
User.create!(
  email: 'admin@example.com',
  password: 'password',
  password_confirmation: 'password'
)
30.times do
  Course.create!([{
    title: Faker::Educator.course_name,
    description: Faker::TvShows::GameOfThrones.quote,
    user_id: User.first.id
  }])

rails db:drop db:create db:migrate db:seed

__user.rb__
class User < ApplicationRecord
  has_many :courses
  def to_s
    email
  end

__course.rb__
class Course < ApplicationRecord
  belongs_to :user
  def to_s
    title
  end

__courses_controller.rb__
def create
  @course = Course.new(course_params)
  @course.user = current_user
```
Guide: https://github.com/heartcombo/devise

## Add custome messages
__layouts/_messages.html__
```ruby
- flash.each do |name, msg|
  - if msg.is_a?(String)
    %div{:class => "alert alert-#{name.to_s == 'notice' ? 'success' : 'danger'}", :role => "alert"}
      %button.close{"aria-hidden" => "true", "data-dismiss" => "alert", :type => "button"} ×
      = content_tag :div, msg, :id => "flash_#{name}" 
```