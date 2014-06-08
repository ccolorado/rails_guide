

# Crating a new rails project

  $ rails new <application_name>

# Installing rails
## A rails project Anatomy

/blog/
 ./Gemfile       These files allow you to specify what gem dependencies are needed for your Rails application. These files are used by the Bundler gem 
 ./Gemfile.lock
 ./README.rdoc   This is a brief instruction manual for your application. You should edit this file to tell others what your application does, how to set it up, and so on.
 ./Rakefile      This file locates and loads tasks that can be run from the command line. The task definitions are defined throughout the components of rails. Rather than changin RAke, you should add your own tasks by adding files to the lib/tasks directory of your application.
 ./app           Contains the controllers, models, views, helpers, mailers and assets for your application
 ./bin           Contains the rails scripts that start your application and can contain other scripts you use to deploy or run your application
 ./config        Configure your applications routes, database, and more.
 ./config.ru     Rack configuration for Rack based servers used to start the application
 ./db            Contains your current database schema, as well as the database migrations
 ./lib           Extend modules for your application
 ./log           Application log files.
 ./public        The only folder seen by the world as-is. Contains static files and compiled assets
 ./test          Unite test, fixtures, and other tests apparatus.
 ./tmp           Temporary files like cache, pid and session files.

 ./vendor        A place for all third-party code. In a typical Rails application this includes vendored gems

# 4 Hello Rails

## 4.1 Starting up the server

  $ rails server


## 4.2 Say hello to rails

Minimum requirements:
* Controller
* View

A _controller_ purpose is to handle the requests.
Routing decides what controller receives each request

A _view_ should just display information, and they are written the eRuby language (Embedded Ruby)


Creating a controller with a controller generator:
Syntax: rails generate controller NAME [action action] [options]

  $ rails generate controller welcome index

Rails then will create several files

      create  app/controllers/welcome_controller.rb
       route  get "welcome/index"
      invoke  erb
      create    app/views/welcome
      create    app/views/welcome/index.html.erb
      invoke  test_unit
      create    test/controllers/welcome_controller_test.rb
      invoke  helper
      create    app/helpers/welcome_helper.rb
      invoke    test_unit
      create      test/helpers/welcome_helper_test.rb
      invoke  assets
      invoke    coffee
      create      app/assets/javascripts/welcome.js.coffee
      invoke    scss
      create      app/assets/stylesheets/welcome.css.scss

Files

* app/controllers/welcome_controller.rb           These files include the controller itself 
* app/views/welcome/index.html.erb                The controller view directory as well as the view.
* test/controllers/welcome_controller_test.rb     Unit testing files and directories
* app/helpers/welcome_helper.rb                   Helpers ?
* app/assets/javascripts/welcome.js.coffee        Javascript/Coffescript assets
* app/assets/stylesheets/welcome.css.scss         CSS assets

Settings

route  get "welcome/index"

## 4.3 Setting the application Home Page

Editing the routes of the application can be performed at the routes file:
config/routes.rb

After creating the welcome controller and editing it's view ( app/views/welcome/index.html.erb ), 
we can now access its content on http://localhost:3000/welcome/index. But to set it up as a default for 
http://localhost:3000/ _root_ must be pointed to this particular view
This file contains various commented examples. We need to use root's example

```DSL
  root 'wellcome#index'
```

# 5 Getting Up and Running

ATM we have created controller, an action and a view
Now we need to learn what a resource is.
A _Resourece_ is the term used for a collection of similar objects, such as articles, people or animal.
You can Create, Read, Update or Destroy items A.K.A CRUD

Rails provides a method called **resources** which can delete a standard REST resource.
Adding the 'article' resource should look like this:

```diff
  config/routes
  + resources :articles
```

Running rake routes will list all the RESTful actions that are available.
resources are declared in plural as rake will make meaningfull use of the distinction

  $ rake routes

      Prefix Verb   URI Pattern                  Controller#Action
    articles GET    /articles(.:format)          articles#index
             POST   /articles(.:format)          articles#create
 new_article GET    /articles/new(.:format)      articles#new
edit_article GET    /articles/:id/edit(.:format) articles#edit
     article GET    /articles/:id(.:format)      articles#show
             PATCH  /articles/:id(.:format)      articles#update
             PUT    /articles/:id(.:format)      articles#update
             DELETE /articles/:id(.:format)      articles#destroy
        root GET    /                            welcome#index

## 5.2 Laying down the ground work
Restful design would suggest subject/verb syntax of our resources.
Some work towards that has already been layout 
Going to: 
http://localhost:3000/articles/new

Will yield the error:
_Routing Error, uninitialized constant ArticlesController_

This should be expected as the routes have not been assigned to controller.
We will need to create a controller that will correspond with our articles resource.To
generate the necessary controller ( ArticleController ). The following command must be
issued.

  $ rails g controller articles

Notice the use of g instead of generate and articles being referred in plural just as the
resource definition on the config/routes.rb file.

**Made a typo ? rails destroy controller <typo> will take care of that**
