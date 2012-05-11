## Mr. Hyde lets you auto-deploy your Jekyll/Octopress blog on Heroku
#### Overview
* Only push to Github, Mr. Hyde takes care of the rest to update your blog on your heroku site
* No need to generate, No need to commit your public folder
* Dependencies
  * Homebrew
  * Jekyll or Octopress blog with Heroku stack cedar dyno

### How to Use
#### Prepare your blog
* Delete your public folder from your blog (you won't need it anymore)
* Put "public" in your blog's .gitignore
* Push up your blog to the master branch of a github repository
* Add the Post-Receive URL on Github
  * In the `Admin` section of your blog's Github repo add a WebHook URL for your heroku url. For example, `http://www.jumpstartlab.com/generate`. A POST request will be submitted whenever you commit changes to your blog. This is how the server will pull your blog's latest changes from Github and generate the new static content.
  * For security, you may want to generate a hash string that will be difficult for people to guess.
![Github WebHook URLs Section](https://skitch.com/athal7/83wr7/administration-athal7-curriculum)

#### Run the setup script
* curl -O https://raw.github.com/athal7/mr_hyde/master/mr_hyde && rake setup && rm mr_hyde
* Respond to the prompts

### What is happening?
* We are overwriting your heroku site with a sinatra app that, when it receives the post-receive request from your repository, uses a custom buildpack to generate your Jekyll/Octopress blog.
