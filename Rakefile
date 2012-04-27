require 'yaml'

desc "Set up auto-deploy script"
task :setup do
  puts "************"
  puts "Thanks for using our Jekyll auto-generate/deploy script."
  puts "You'll be up and running in no time, just a few questions first..."
  puts "*************"
  user_name = ask "Github Username: "
  system("echo #{user_name}") 

  blog_git = ask "Github READ ONLY Blog Url: "
  system("echo #{blog_git}")

  branch = ask "Choose a branch for deploying: "
  system("echo #{branch}")

  heroku_url = ask "Heroku Git URL: "
  system("echo #{heroku_url}")

  route_name = ask "Preferred Route Name: "
  system("echo #{route_name}")

  # Get Hub for Git Commands
  system("brew install hub")

  # ADD POST RECEIVE URL TO BLOG

  # Setup Buildpack
  system("hub clone austenito/heroku-buildpack-ruby-octopress buildpack")
  Dir.chdir("buildpack")
  system("hub fork")
  system("git remote add upstream git@github.com:#{user_name}/heroku-buildpack-ruby-octopress.git")
  file_name = "config.yml"
  text = File.read(file_name)
  text.gsub!("your blog", blog_git)
  text.gsub!("master", branch)
  File.open(file_name, "w") {|file| file.write(text) }
  system("rake setup")
  system("git add .")
  system("git commit -m 'Prepare config for auto-deploy'")
  system("git push --force upstream master")
  Dir.chdir("..")

  # Setup Sinatra
  system("hub clone austenito/heroku-octopress-autodeploy sinatra")
  Dir.chdir("sinatra")
  system("hub fork")
  system("git remote add heroku #{heroku_url}")
  system("heroku config:add BUILDPACK_URL=git://github.com/#{user_name}/heroku-buildpack-ruby-octopress.git")
  file_name = "config.yml"
  text = File.read(file_name)
  text.gsub!("foo", route_name)
  File.open(file_name, "w") {|file| file.write(text) }
  system("rake --rakefile setup.rake")
  system("git add .")
  system("git commit -m 'Prepare config for auto-deploy'")
  system("git push --force heroku master")
  Dir.chdir("..")
end

task :destroy do
  system("rm -Rf sinatra && rm -Rf buildpack")
end

def ask message
  print "#{message}"
  STDIN.gets.chomp
end
