# HEROKU

**start here**
[provision a database](https://devcenter.heroku.com/articles/getting-started-with-nodejs#provision-a-database)


## QUICK START
* use powershell with `heroku` cli commands -- your config doesn't like it

```bash
	heroku login				# opens browser and logs you in
	git clone/init/whatever		# compose some init files for your app
	heroku create <my-app>		# create app <optional name, must be globally unique>
	# DEPLOY
	git push heroku master		# deploys code
	heroku ps:scale web=1		# verify it's up and running
	heroku open					# handy shortcut to open the app in a browser
	heroku open profile			# open your app to subdir profile
	heroku logs --tail			# view logs
	# LOCAL DEV
	npm i						# install dependencies when running locally
	heroku local web			# run your app locally (on 5000 by default)
	ctrl-c						# kill the local app
	git add .
	git commit -m "cool ascii faces"
	git push heroku master		# make a change and deploy it
	# CONFIG VARS
	heroku config:set TIMES=2	# set config var for heroku deployment env
	heroku config				# view config vars for heroku deployment env
```

## HEROKU AND GIT

## BASICS
* **procfile**: a root-level text file that defines the command that is used to start your app
	- in the following example, the process type `web` will be attached to the HTTP routing stack of heroku to receive web traffic once deployed
	- a procfile may contain multiple process types
		```bash
			web: node index.js
		```
* **config**: define config vars such as encryption keys or external resource addresses
	- these vars are saved in root level `.env` file
	- while running locally, you can just change the contents of the `.env` file
	- to add vars to your deployed app, use:
		```bash
			heroku config:set TIMES=2
			heroku config				# view config vars
		```

## NODE APPS
* heroku knows a node app is a node app because of the `package.json` file
* the default `package.json` can be created with `npm init --yes`
	```js
		// package.json
		{
			"name": "node-js-getting-started",
			"version": "0.3.0",
			...
			"engines": {
				"node": "12.x"
			},
			"dependencies": {
				"ejs": "^2.5.6",
				"express": "^4.15.2"
			},
			...
		}
	```
* when you deploy an app to the heroku server, it will automatically build -- when working locally do the usual `npm i` to install dependencies


## HEROKU AND POSTGRES
* [postgres and nodejs](https://blog.logrocket.com/setting-up-a-restful-api-with-node-js-and-postgresql-d96d6fc892d8/)
* [deploying with postgres](https://www.taniarascia.com/node-express-postgresql-heroku/)
* to deploy:
	- scripts to init data are in /seeders
	```shell
		# use powershell for the following

		# login to your postgres app
		heroku pg:psql postgresql-rugged-34339 --app intense-castle-24661

		# populate your prod db
		cat .\seeders\media.sql | heroku pg:psql postgresql-rugged-34339 --app intense-castle-24661

		# deploy app
		git push heroku master
	```
* you can view your deployed database with these commands:
	```shell
		heroku pg:info
		heroku pg:psql		# enter psql command shell
	```

