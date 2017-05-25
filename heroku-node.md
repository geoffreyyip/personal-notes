### How to Deploy

* Initialize a local git repository
* `heroku create`: create a remote repository associated with your local git repo
* Create a `package.json` and `index.js` so Heroku knows it's a Node app
* Add a `Procfile` with `web: node index.js` to explictly tell Heroku what process to run
* `git push heroku master`: push recent changes to Heroku's servers
* `heroku ps:scale web=1`: set an instance to run the app
* `heroku open` (optional): open app in local web browser. You can also set an optional route to open. (E.g. `heroku open llamas` will lead to `localhost:5000/llamas`)

