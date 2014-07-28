Notes from http://guides.rubyonrails.org/getting_started.html

# Crating a new rails project

  $ rails new <application_name>

# Installing rails
## A rails project Anatomy

/blog/
 ./Gemfile       These files allow you to specify what gem dependencies are needed for your Rails application. These files are used by the Bundler gem
 ./Gemfile.lock
 ./README.rdoc   This is a brief instruction manual for your application. You should edit this file to tell others what your application does, how to set it up, and so on.
 ./Rakefile      This file locates and loads tasks that can be run from the command line. The task definitions are defined throughout the components of rails. Rather than changing Rake, you should add your own tasks by adding files to the lib/tasks directory of your application.
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

Rails provides a method called **resources** which can declares a standard REST resource.
Adding the 'article' resource should look like this:

```diff
  config/routes
  + resources :articles
```

Running rake routes will list all the RESTful actions that are available.
Resources are declared in plural as rake will make meaningful use of the distinction

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

## 5.1 Laying down the ground work
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

The controller ArticlesController blog/app/controllers/articles_controller.rb is where we
define the actions of this controller.

To define a new action we just need to add a method that corresponds to such action inside
the ArticlesController class

```ruby
def new

end
```

Rails expect controllers to have associated views, so running
http://localhost:3000/articles/new will yield an error about missing templates (AKA views)
The following is a representation of the message formated for easy reading as well as
notes on about descriptions.

  Missing template articles/new, application/new with {
    :locale=>[:en],       #Default spoken languaje
    :formats=>[:html],    #Format of the template to be served in response


    :handlers=>[          #Tells what template handlers coulde be used to render a
                          #template
      :erb,               # erb => most commonly used for html templates
      :builder,           # builder => xml templates
      :raw,
      :ruby,
      :jbuilder,
      :coffee             # coffe => CoffeeScript to build JavaScript templates
    ]}.

  Searched in: *          # Where has rails looked for the missing template
  "/path/to/blog/app/views"

<<<<<<< HEAD
This simple template will satisfy rails:

  echo "<h1>New Article</h1>" > blog/app/views/articles/new.html.erb

The extension html indicate the output format and the erb extension it's handler.

## 5.2 The first form

Form builder is the handler to write form templates. Using templates instead of basic
html will allow us to link a form to the ORM driver ( or activerecord ?).

The *form_for* method can link a form to a specific object/resource

`` form_builder
  <%= form_for :article do |f| %>
    <p>
        <%= f.label :title %><br>         #Field title label
        <%= f.text_field :title %><br>    #field title input field
        ...
``

However this form has no action and will be redirected to itself.

`` form_builder
  <%= form_for :article, url: articles_path  do |f| %>
``
Usually a value for *url:* would be _{action: "create"}_, Instead in this case the
*helper* 'articles_path' is used, ( God knows why we are learning shortcuts this early but
the again generators are shortcuts )

articles_path actually resolves to  http://0.0.0.0:3000/articles/new

"The articles_path helper tells Rails to point the form to the URI Pattern associated with
the articles prefix"

## 5.3 Creating articles
To create the required action simply add the create method definition on
app/controllers/articles_controller.rb

`` ruby
  def create
    render plain: params[:article].instepct
  end
``

NOTE: this action is only using the render command, so not storing it anywhere.

The expected result is to see a json syntax on the browser but for some reason I am still
getting the Template missing. Despite the tutorial stating this should not happened with
the render method.

*render*'s plain: option was introduced on rails 4.1, currently using 4.0.1, text is the
corresponding option.

## 5.4 Creating the Article model

Models are referred in singular mode

Creating the model
$ bin/rails generate model Article title:string text:text
$ ./bin/rails generate model Article title:string text:text
      invoke  active_record
      create    db/migrate/20140727235520_create_articles.rb
      create    app/models/article.rb
      invoke    test_unit
      create      test/models/article_test.rb
      create      test/fixtures/articles.yml

## 5.5 Running a Migration

A migration is a class that is intended to represent, create and modify a table.
This class includes a time stamp, in order to keep track of changes to the table.
this actions are reversible trough rake.

$ bin/rake db:migrate

==  CreateArticles: migrating =================================================
-- create_table(:articles)
   -> 0.0018s
==  CreateArticles: migrated (0.0019s) ========================================

By default this changes are applied to the development environment.

rake db:migrate RAILS_ENV=production # will apply changes to the production environment

## 5.6 Saving data in the controller

At this point we can make use of the Article (Article's model) object inside our controller

Security 'gotchas' will kick in as, the rails requires explicit references
to the parameters we wish to store in the database.

`` ruby
  private
  def article_params
    params.require(:article).permit(:title, :text)
  end
``
require and permit will allow both :title and :text in this action

## 5.7 Showing Articles

After implementing the require/permit options, the application will still complain about
not having the 'show' action which the spec can be found on the rake routes output from
back when we added the article resource

`` ruby
  def show
    @article = Article.find(params[:id])
  end
``
=======
Rails will expect the template to be stored on:
app/views/articles/new.html.erb

On the erb template, form_for :controller helps us to hone down the form for our said
controller.
>>>>>>> 0aa922b25052c31a6b173cae5dfe4363fc10a5b5


## 5.3 Creating articles
