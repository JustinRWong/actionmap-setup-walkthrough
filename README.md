# Summer 2020 CS 169

##  CHIP 10.5 Actionmap Setup Walk through

The purpose of this is to help walk through setting up your environment when following along the docs, while having quicker communication. 

If you bump into an issue, please create an issue to start a thread.


### Setup:
```sh
## Clone the repo
git clone https://github.com/cs169/hw-agile-iterations-group03.git
cd hw-agile-iterations-group03


## Ruby prereqs
rvm install "ruby-2.6.5" ## install correct ruby version
rvm use 2.6.5 ## use ruby version
gem install bundler:2.1.4 ## install bundler


## Node prereqs... see https://github.com/nvm-sh/nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash  ## install  node
## add nvm to .bash_profile if needed by running `vim ~/.bash_profile` and
# export NVM_DIR=~/.nvm
# source ~/.nvm/nvm.sh


## based on package.json
nvm -v ## make sure nvm is installed
nvm install 12.13.1 ## nvm install specific node version
npm install -g yarn@1.22.4 ## npm install specific yarn verify 
node -v ## verify node v12.13.1
yarn -n ## yarn version 1.22.4

## Dependencies 
bundle config set path vendor/bundle  ## set a bundle config path
bundle config set without production  ## naturally set without prod
bundle install ## install gems/ruby dependencies
yarn install ## install node dependencies

## Starting app
## db migration
bundle exec rails db:migrate
bundle exec rails db:seed

## Test to see if app works then visit localhost:3000
bin/webpack-dev-server ## in one termiinal
bundle exec rails s ## in another terminal
## Ctrl + C to close the server

```


### Style Checking / Linters:
```sh
## ADD ALL the . files  from https://paper.dropbox.com/doc/HW-Agile-Iterations-Setup-Clarifications-rKI15rOpO0ViTi8eLi3a7
## Make sure you have the following files:
## .browserslistrc
## .bundle
## .eslintignore
## .eslintrc.js
## .gitignore
## .haml-lint.yml
## .rubocop.yml
## .git(should exist when you clone)


bundle exec rubocop 
bundle exec rubocop -a 
bundle exec haml-lint
## Should see 0 errors 

yarn run lint_fix ## this checks  any styling issues

## Sometimes, you may just want to have a file watcher that runs linter check and tests as you work on the project. Guard is a tool that allows you to run file watchers that run automated tasks whenever a file or directory is modified. The Guardfile specifies file watchers and tasks to run. To initiate guard, run the following command in a separate terminal: 
bundle exec guard
```


#### More Info About Guard:

    Guard: file watcher that runs linter check and tests as you work on the project. 

    Guard is a tool that allows you to run file watchers that run automated tasks whenever a file or directory is modified. 
    The Guardfile specifies file watchers and tasks to run. 

    To initiate guard, run the following command in a separate terminal:

    `bundle exec guard`

### Credential Management and Initial Setup

####  Make sure heroku CLI is available.
    If you have Heroku CLI set up, follow https://devcenter.heroku.com/articles/heroku-cli to install it

    After, running `heroku -v` should show the heroku version you have installed.



```sh
## Set up your own credential file
rm -rf config/credentials.yml.enc
EDITOR=vim rails credentials:edit

## Add more credenentials
## Add the ones required for this setup:
```

#### Google Credentials

   For google oauth credentials, go to https://developers.google.com/identity/protocols/oauth2. 

   Make sure in your OAuth Consent Screen, you make your User Type `External` 

   Add a URI when creeating an OAuth client ID: https://your-heroku-app.herokuapp.com/auth/google_oauth2/callback 
   
   Add the following keys to the file you edit with vim
   - GOOGLE_CLIENT_ID
   - GOOGLE_CLIENT_SECRET
   - GOOGLE_API_KEY

#### Github Credentials 

   For github credentials, go to https://github.com/settings/developers and click `New OAuth App`. 

   Your Homepage URL="https://your-heroku-app.herokuapp.com/"
   
   Your Authorization callback URL=https://your-heroku-app.herokuapp.com/auth/github/callback

  - GITHUB_CLIENT_ID
  - GITHUB_CLIENT_SECRET

** example credentials**
```sh
# aws:
#   access_key_id: 123
#   secret_access_key: 345

# Used as the base secret for all MessageVerifiers in Rails, including the one protecting cookies.
secret_key_base: *****************************

GITHUB_CLIENT_ID: *********
GITHUB_CLIENT_SECRET: *********

GOOGLE_CLIENT_ID: ***********
GOOGLE_CLIENT_SECRET: ****************
GOOGLE_API_KEY: *********
## escape, then :wq to save and quit
```

#### Verify that the credentials can be read in the rails console again
  
  Once saved, open a rails console to verify that it works. The output  of each of the console should be the credentials you just saved

```sh
bundle exec rails c
> Rails.application.credentials[:secret_key_base]
> Rails.application.credentials.dig(:production, :GOOGLE_CLIENT_ID)
> Rails.application.credentials.dig(:development, :GOOGLE_CLIENT_SECRET)
```


#### Heroku
     With heroku CLI installed, you should be ready to push.


```sh
heroku login ## should open up in the browser after you press any keys

## when instide e the  root directory, `heroku create ` will create a new project
heroku create OPTINAL_NAME ## 

## heroku build info for dependencies
heroku buildpacks:add heroku/nodejs ## add Heroku nodejs buildpack
heroku buildpacks:add heroku/ruby   ## add Heroku ruby build pack
heroku buildpacks ## to confirm packs have been added

## remove things from gitiignore
git rm -r --cached .
git add --all
git commit -m "working locally, init credentials, and heroku init"
git push origin YOUR_BRANCH_NAME ## for your repo on github
git push heroku master ## to push to heroku. this may take several minutes to run.

heroku run rails db:migrate ## this may take several minutes to run
heroku run rails db:seed
```

## Expected Outcome

   Feel free to explore mine at <a href=https://actionmap03.herokuapp.com/>https://actionmap03.herokuapp.com/ </a>.

   Specifically, go to login and try logginng in with either google or your github in an incognito window. You should be able to log in with your Google and Github logint.
