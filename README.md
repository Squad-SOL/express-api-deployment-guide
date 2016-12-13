[![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)](https://generalassemb.ly/education/web-development-immersive)

# Express Deployment with Heroku

You've learned a lot about how to build a Node application over the last few
weeks. Now let's 'go public' and share our apps with the world!

## Prerequisites

- [Express Api](https://github.com/ga-wdi-boston/express-api)
-   This guide assumes you have followed [these installation instructions](https://github.com/ga-wdi-boston/express-api-template#installation) unequivocally.
## Objectives

-   Create a Heroku app from the command line
-   Push the latest code to Heroku
-   Migrate the production database

## Getting Set Up

Before you can begin deploying your applications to Heroku, there are some
things you'll need to do first.

1.  **Create a Heroku account**, at [https://www.heroku.com](https://www.heroku.com).
    You will be sent an activation email, so be sure to check your inbox so that
    you can activate your account.
1.  Install the Heroku Command Line Tools: run `brew install heroku`.
1.  **Log into Heroku** by running `heroku login` from the console and providing
    your Heroku credentials when asked. Once you log in, if you're prompted
    to add these credentials to your keychain, say yes. *You will not be able*
    *to see your password*
1.  **sign up for mlab at mlab.com** / click link in verification email
1.  **Credit card information needs to be submitted on heroku** - looks like as long as you don’t MASSIVELY populate your database, you’re credit card will not be charged
1.  **Make sure you have heroku tool belt installed for MAC**

## Deploying to Heroku

Now you're set up to use Heroku. To deploy a new application
to Heroku:


-   [ ] Run `heroku create` in the command line in the root of your Express API to
    create a new (blank) app on Heroku.
-   [ ] Push your latest code to Heroku (`git push heroku master`)
-   [ ] Tell Heroku to run your migration files (`heroku run rake db:migrate`).
    If you have any other rake tasks that need to run (e.g. `rake db:seed`), run
    those with `heroku run` as well.
-  [ ] commit to your local master branch
-  [ ] in terminal, run : `heroku create`
-  [ ] in terminal run : `git remote -v` - make sure that heroku remote repo is pointing towards the newly created heroic url.   IF NOT, run this in terminal : `git remote set-url heroku <copy and paste the URL of your heroiku hosted app that you just created in step above>`
-  [ ] go to your heroku dashboard, click on newly created heroku app, click on resources tab, search for and add mLab for Add-Ons
-  [ ] **OR** `$ heroku addons:create mongolab:sandbox`
-  [ ] in terminal, run : `git push heroku master`  (should build your site)
-  [ ] due to the first line of code in the `server.js` file, the default deployment environment will be `production`
-  [ ] in terminal, run : `echo SECRET_KEY=$(/usr/local/opt/openssl/bin/openssl rand -base64 66 | tr -d '\n')` this should generate a secret_key
-  [ ] in terminal run: `heroku config:set <copy and paste secret_key generated from last command>` .   should start with “SECRET_KEY and span about 40 randomized characters”
-  [ ] you need to set your client url so that your deployed API will ONLY accept requests from the correct domain. IF you're client is deployed on Github, your url will be:
      `https://<% github username %>.github.io`
-  [ ] Set your client url by:
      `heroku config:set CLIENT_URL="https://<% github username %>.github.io"`
-  [ ] You should have three config variables set in heroku (`heroku>settings>config vars`): MONGODB_URI, SECRET_KEY, CLIENT_URL
-  [ ] Once all three of these are set, run in terminal: `heroku restart`
-  [ ] Then in terminal, run: `heroku open`

## Heroku Command Reference

A full list of Heroku commands can be access by running `heroku --help`; below
are some of the more common ones.

|                Commands                |                                                 Behavior                                                 |
| -------------------------------------- | -------------------------------------------------------------------------------------------------------- |
|           `heroku apps:info`           |                                  Get info about ALL of our Heroku apps.                                  |
| `heroku apps:info --app {name_of_app}` |                                  Get info about a specific Heroku app.                                   |
| `heroku apps:open --app {name_of_app}` |            Open any given Heroku app <br> (other than the one we're currently working with.)             |
|             `heroku logs`              |                                   Logs from the currently running app.                                   |
|              `heroku ps`               |                              Processes running in your heroku application.                               |
|           `heroku releases`            |      Each time you deploy to heroku you are creating a "release". This command shows all releases.       |
|            `heroku pg:info`            | Access Postgres from within Heroku and show the heroku plan, connections, pg version, data size, tables. |
|            `heroku pg:psql`            |                                      ... and open a `psql` console.                                      |
|            `heroku run ...`            |                                    Run a program from within Heroku.                                     |
|            `heroku config`             |                           Environmental variables in your current Heroku app.                            |
|            `heroku config:set SECRET_KEY_BASE=$(rake secret)`            |                                    Set Secret Key                                     |
|            `heroku config:set SECRET_TOKEN=$(rake secret)`            |                                    Set TOKEN                                     |
|            `heroku config:set CLIENT_ORIGIN=https://yourgithubname.github.io`            |                                    Set CLIENT_ORIGIN                                     |
|            `heroku apps:rename newname (optional)`            |                                    Rename heroku app name (entirely optional)                                     |
|            `heroku restart`            |                                    restart heroku                                     |
|            `heroku open`            |                                    Open your heroku app in default browser                                     |
|            `heroku --help`            |                                    Displays a heroku CLI usage summary.                                     |


## WARNING: Ephemeral Filesystem.

One serious limitation of Heroku is that it provides an 'ephemeral filesystem';
in other words, if you try to save a new file inside the repo (e.g. an uploaded
image file), it will disappear when your app is restarted or redeployed.

As an example, try running the following commands:

```sh
heroku run bash
touch happy.txt; echo 'is happy' > happy.txt
cat happy.txt
```

Then, hit Ctrl-D to get out of heroku bash shell. If you re-open the shell and
run `ls -l`, `happy.txt` will be missing!

The typical workaround is to save files in cloud storage such as [Amazon
S3](https://aws.amazon.com/s3/).

## Troubleshooting

-  [Heroku Addons](https://devcenter.heroku.com/articles/managing-add-ons) and [mLab MongoDB](https://elements.heroku.com/addons/mongolab)
- [Previous Issues](https://github.com/ga-wdi-boston/group-project/issues?utf8=%E2%9C%93&q=is%3Aissue%20deploy%2C%20heroku)



## Additional Resources

-   [Heroku Command Line](https://devcenter.heroku.com/categories/command-line)

## [License](LICENSE)

1.  All content is licensed under a CC­BY­NC­SA 4.0 license.
1.  All software code is licensed under GNU GPLv3. For commercial use or
    alternative licensing, please contact legal@ga.co.
