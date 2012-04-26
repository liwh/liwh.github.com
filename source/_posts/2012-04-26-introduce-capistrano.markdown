---
layout: post
title: "introduce-capistrano"
date: 2012-04-26 11:05
comments: true
categories: deploy
---

### Introduce Capistrano

The Capistrano always use to auto update the remote machine enviroment.

##### Install

First of all, You need to know ,The capistrano gem only use on your local machine ,not the remote machine .It uses the Net::SSH and Net::SFTP to connect the remote servers and run shell commands on the remote servers, only the machines you want to use trigger a deploy.

On your local machine,run this

```bash
local$ gem install capistrano
```

generate the configure file

```bash
local$ cap deploy
```

The command generate two file in your project , Capfile and config/deploy.rb

Now you can use the shell command below find what task created.

```bash
local$ cap -T
cap bundle:install       # Install the current Bundler environment.
cap deploy               # Deploys your project.
cap deploy:check         # Test deployment dependencies.
cap deploy:cleanup       # Clean up old releases.
cap deploy:cold          # Deploys and starts a `cold' application.
cap deploy:migrate       # Run the migrate rake task.
cap deploy:migrations    # Deploy and run pending migrations.
cap deploy:pending       # Displays the commits since your last deploy.
cap deploy:pending:diff  # Displays the `diff' since your last deploy.
cap deploy:rollback      # Rolls back to a previous version and restarts.
cap deploy:rollback:code # Rolls back to the previously deployed version.
cap deploy:setup         # Prepares one or more servers for deployment.
cap deploy:symlink       # Updates the symlink to the most recently deployed ...
cap deploy:update        # Copies your project and updates the symlink.
cap deploy:update_code   # Copies your project to the remote servers.
cap deploy:upload        # Copy files to the currently deployed version.
cap deploy:web:disable   # Present a maintenance page to visitors.
cap deploy:web:enable    # Makes the application web-accessible again.
cap invoke               # Invoke a single command on the remote servers.
cap shell                # Begin an interactive Capistrano session.

Some tasks were not listed, either because they have no description,
or because they are only used internally by other tasks. To see all
tasks, type `cap -vT'.

Extended help may be available for these tasks.
Type `cap -e taskname' to view it.
```

##### Write Deploy Thing

```ruby
$:.unshift(File.expand_path('./lib', ENV['rvm_path'])) # Add RVM's lib directory to the load path.
require 'rvm/capistrano'  # Load RVM's capistrano plugin.
set :rvm_ruby_string, '1.9.3' # Or whatever env you want it to run in.
set :rvm_type, :user
set :rails_env, :production

# Bundler tasks
require 'bundler/capistrano' #intergration rvm and bundler
set :application, "" # set your application name
set :repository,  "" #set the repository address

set :scm, :git
# Or: `accurev`, `bzr`, `cvs`, `darcs`, `git`, `mercurial`, `perforce`, `subversion` or `none`
#If you’re using your own private keys for git you might want to tell Capistrano to use agent forwarding with this command.
# Agent forwarding can make key management much simpler as it uses your local keys instead of keys installed on the server.
set :ssh_options, { :forward_agent => true }

set :branch,      "develop" # You need to tell cap the branch to checkout during deployment:
set :deploy_via, :remote_cache  #This is probably the best option as it will only fetch the changes since the last.
set :git_enable_submodules, 1 # If you’re using git submodules you must tell cap to fetch them.


set :use_sudo, false #

# This is needed to correctly handle sudo password prompt
#somethime if you don't use this ,it call the bug.
#[liwh@xx.xx.xx.xx] executing command
#[xx.xx.xx.xx :: err] Permission denied, please try again.
#[xx.xx.xx.xx :: err] Permission denied, please try again.
#[xx.xx.xx.xx :: err] Permission denied (publickey,gssapi-with-mic,password).
#[xx.xx.xx.xx :: err] fatal: The remote end hung up unexpectedly

default_run_options[:pty] = true

set :user, "deployer"  # The server's user for deploys
set :scm_passphrase, "p@ssw0rd"  # The deploy user's password
role :web, host                          # Your HTTP server, Apache/etc
role :app, host
set  :deploy_to, "/home/#{user}/#{application}" #defaults to /u/apps/#{application}. This variable defines the target deployment directory.

set :unicorn_conf, "#{deploy_to}/current/config/unicorn.rb"
set :unicorn_pid, "#{deploy_to}/shared/pids/unicorn.pid"
#role :app, ""                          # This may be the same as your `Web` server
#role :db,  "your primary db-server here", :primary => true # This is where Rails migrations will run
#role :db,  "your slave db-server here"

# if you want to clean up old releases on each deploy uncomment this:
# after "deploy:restart", "deploy:cleanup"

# if you're still using the script/reaper helper you will need
# these http://github.com/rails/irs_process_scripts

# If you are using Passenger mod_rails uncomment this:
# namespace :deploy do
#   task :start do ; end
#   task :stop do ; end
#   task :restart, :roles => :app, :except => { :no_release => true } do
#     run "#{try_sudo} touch #{File.join(current_path,'tmp','restart.txt')}"
#   end
# end
# Unicorn control tasks
```

##### The rollback task

Sometimes,when we got a error on the deploy task ,it will rollback the task. The rollback do the things on the image below
![image](/images/rollback_task.png)

### Links

[deploy-with-capistrano](http://help.github.com/deploy-with-capistrano/)
[Deploying with Sinatra + Capistrano + Unicorn](http://software.tulentsev.com/2012/03/deploying-with-sinatra-capistrano-unicorn/)
[lighting-fast-zero-downtime-deployments-with-git-capistrano-nginx-and-unicorn](http://ariejan.net/2011/09/14/lighting-fast-zero-downtime-deployments-with-git-capistrano-nginx-and-unicorn)
