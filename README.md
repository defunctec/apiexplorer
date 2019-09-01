#### NOTE: While open-source, some features of this block explorer are not easily compatible with running your own version locally, and this is no longer recommended (except for expert users). ####

--


# Setup Instructions #

## Install ##

### OSX ###
- Follow the instructions [here](http://docs.python-guide.org/en/latest/starting/install/osx/) to install [Homebrew](http://brew.sh/) and then (re)install python.
- If for some reason the step above does not install pip and virtualenv, follow the instructions [here](https://pip.pypa.io/en/latest/installing.html#python-os-support) to get pip and then install virtualenv using pip install virtualenv.
- Install the foreman gem for accessing environmental variables:  `$ gem install foreman`
- Optional (for webhooks): install ngrok with `brew install ngrok` (thanks Homebrew!)
- Optional (advanced features): Install the latest stable version of postgres 9. (http://postgresapp.com/)[Postgresapp for Mac] is quite easy to install.

### Ubuntu ###
- `$ sudo apt-get update && sudo apt-get upgrade -y`
- `$ sudo apt-get install postgresql libpq-dev`
- `$ apt install software-properties-common`
- `$ add-apt-repository ppa:deadsnakes/ppa`
- `$ apt install python3.7`

# Change default python version to python 3.7
- `$ echo "alias python3=python3.7" >> .bashrc`
- `$ echo "alias python=python3.7" >> .bashrc`
- `$ source .bashrc`

# Install explorer
- `$ git clone https://github.com/defunctec/apiexplorer.git`
- `$ cd apiexplorer`
 
# virtualenv setup
- `$ apt install virtualenv`
- `$ virtualenv -p python3 venv`
- `$ source venv/bin/activate`

# pip build dependenices
- `$ apt-get install libncurses5-dev`
- `$ apt-get install python3-pip python3.7-dev`

# install pip dependenices
- `$ pip3 install -r requirements.txt`

# setup postgres database and update password
- `$ sudo -u postgres psql`
- `$ CREATE DATABASE explorer_local;`
- `$ CTRL+z (To exit)`

## Configure ##
- Run `$ git remote -v` , it should look like this:
```
origin	git@github.com:blockcypher/explorer.git (fetch)
origin	git@github.com:blockcypher/explorer.git (push)
```
- Create a `.env` file in the project root directory with the following:
```
DEBUG=True
TEMPLATE_DEBUG=True
DJ_DEFAULT_URL=postgres://postgres:YOURLOCALPASSWORDHERE@localhost:5432/explorer_local
SECRET_KEY=RANDOMLY_GENERATED_50CHAR_STRING
SITE_DOMAIN=pick_this_yourself.ngrok.com
BLOCKCYPHER_API_KEY=PUT_YOURS_HERE
```
(these are for your local machine, production is a little different as `settings.py` is smartly designed to default to production settings)

- Create DB tables from code (replace `foreman` with `heroku` for running on production, which should basically never happen again):

# Install foreman
- `$ apt install ruby-foreman`

- You may run into errors with the next commands, reinstall psycopg2 if you fail.
- `$ pip uninstall psycopg2`
- `$ pip install psycopg2`

# Create tables and run migrations:
- `$ foreman run python3 manage.py migrate`

# Run the webserver locally:
- `$ foreman run python3 manage.py runserver`

Now visit: http://YOURSERVERIP:8000/

To receive webhooks locally, you need to also run `ngrok` in the terminal (use the same `SITE_DOMAIN` from your `.env` file above):
```
$ ngrok -subdomain=pick_this_yourself 8000
```

Now visit http://pick_this_yourself.ngrok.com to confirm it's working (you could even do this on your phone)

## Check Out the Admin Section ##

- Create a superuser admin for yourself, by entering the following into `$ foreman run python3 manage.py shell`:

```python
from users.models import AuthUser
AuthUser.objects.create_superuser(email='YOURCHOICE', password='PASSWORDGOESHERE')
```

Now visit http://YOURSERVERIP:8000/admin


## Submit Your First Pull Request ##

First, pull the latest version of the code from github:
```
$ git pull origin master
```

Make a new branch:
```
$ git checkout -b my_branch
```

Make some trivial change and commit it:
```
$ git commit -am 'my changes'
```

Push it up to github:
```
$ git push origin my_branch
```

You can submit your pull request here:
https://github.com/blockcypher/explorer

Congrats, you're all setup!

# Post Setup Instructions #

## Build Awesome Features ##

You're on your own for that.

## Git Foo ##

Compare your local version of site to what's on github:
```
$ git log origin/master..HEAD --oneline
```
