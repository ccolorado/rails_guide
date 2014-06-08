

# Crating a new rails project

  $ rails new <application_name>

# Installing rails
## A rails project Anatomy

/blog/
 ./Gemfile       These files allow you to specify what gem dependencies are needed for your Rails application. These files are used by the Bundler gem 
 ./Gemfile.lock
 ./README.rdoc   This is a bref instruction manual for your application. You should edit this file to tell others what your application does, how to set it up, and so on.
 ./Rakefile      This file locates and loads tasks that can be run from the command line. The task definitions are defined throughout the components of rails. Rather than changin RAke, you should add your own tasks by adding files to the lib/tasks directory of your application.
 ./app           Contains the controllers, models, views, helpers, mailers and assets for your application
 ./bin           Contains the rails scripts that start your app and can contain other scripts you use to deploy or run your application
 ./config        Configure your applications routes, database, and more.
 ./config.ru     Rack configuration for Rack based servers used to start the application
 ./db            Contains your current database schema, as well as the database migrations
 ./lib           Extend modules for your application
 ./log           Application log files.
 ./public        The only folder seen by the world as-is. Contains static files and compliled assets
 ./test          Unite test, fixtures, and other tests apparatus.
 ./tmp           Temporary files like cache, pid and session files.

 ./vendor        A place for all third-party code. In a typical Rails application this includes vendored gems
