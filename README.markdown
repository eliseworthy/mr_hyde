# Mr. Hyde lets you auto-deploy your Jekyll/Octopress blog on Heroku
## Features
* Only push to Github, Mr. Hyde takes care of the rest to update your blog on your heroku site
* No need to generate, No need to commit your public folder

## Dependencies
* Homebrew
* Jekyll or Octopress blog
* Heroku stack cedar url for your blog

## How to Use
### Prepare your blog
* Delete your public folder from your blog
* Put "public" in your blog's .gitignore
* Push up your blog to the master branch of a github repository
* Add the Post-Receive URL on Github
  * In the `Admin` section of your blog's Github repo add a post-receive URL. For example, `http://www.jumpstartlab.com/generate`. A POST request will be submitted whenever you commit changes to your blog. This is how the server will pull your blog's latest changes from Github and generate the new static content.
  * For security, you may want to generate a hash string that will be difficult for people to guess.
![Github Post-Receive Section](https://img.skitch.com/20120414-j1fhk2mwei7e4u7n4bxg5y2ubt.jpg)

### Run the setup script
* Clone this repository whever your want
* `cd mr-hyde`
* `rake setup`
* Respond to the prompts

## What is happening?
* We are overwriting your heroku site with a sinatra app that, when it receives the post-receive request from your repository, uses a custom buildpack to generate your Jekyll/Octopress blog.

### Please keep in mind that this process is still being tested. Any feedback or suggestions on how to make it better will be appreciated.