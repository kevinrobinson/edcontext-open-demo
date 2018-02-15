# edcontext-open

This repository is for experimenting!  It contains three pieces:
- A webapp for visualizing and understanding MCIEA survey data
- Tasks for processing and indexing raw survey data
- Experimental work to conduct survey samples via text message

This is a Rails project, deployed on Heroku.  There's a private GitHub repo that contains more historical information; contact @jaredcosulich for more info.

## Demo
Visit https://edcos-demo.herokuapp.com/

Demo login:
- username: demo@demo.edcontext.org
- password: demo

## Screenshots
#### Login
![docs/login.png](login)

#### Districts and Schools
![docs/home.png](home)

#### School overview
![docs/school.png](school)

#### Drill into measures
![docs/drill1.png](drill)

#### Down into item-level responses
![docs/drill2.png](drill2)

## Local development
```
$ bundle install
$ bundle exec rake db:create db:migrate db:seed
```
At this point you can run the app and login.  There won't be any data yet though; keep reading!

## Loading Data
Postgres is the primary data store for the webapp, but the definitions of the questions and measures are stored in `.json` files and raw survey data is stored in `.csv` files.  These are collected offline, and then processed by the rake tasks to load that data into Postgres for use by the webapp.

There are several different kinds of data needed:
- `measures.json` - defines the constructs and measures
- `questions.json` - defines the questions and options
- `student_responses.csv` - survey response data (private only)
- `teacher_responses.csv` - survey response data (private only)

You can load these into the database and index them for use in the webapp by running rake tasks.

You can start by generating fake data:
```
$ bundle exec rake data:generate
```

## Demo deploy
Make a new Heroku app and deploy:
1. Clone the repo
2. Create a new Heroku app
3. Add Postgres
4. Deploy
5. Run `heroku run rake db:migrate db:seed data:generate`
6. Run `heroku run rails console` and add a demo user (eg, `User.create!(email: 'demo@demo.edcontext.org', password: '123456')`)
7. Try it out!

If you want to deploy from the private repo, delete the git history first.


## Production data
This requires access to private data.  Loading all the real response data take a while, so you can start by loading only a sample of the data for one particular school with:

```
$ bundle exec rake data:load_sample
```

This loads all the data:

```
$ bundle exec rake data:load
```
