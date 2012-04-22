require 'yaml'

desc "Set up auto-deploy script"
task :setup do
  puts "************"
  puts "Thanks for using our Jekyll auto-generate/deploy script."
  puts "You'll be up and running in no time, just a few questions first..."
  puts "*************"
  # user_name = ask "Github Username: "
  # system("echo #{user_name}")

  blog_git = ask "Github Blog Url: "
  system("echo #{blog_git}")

  # branch = ask "Choose a branch for deploying: "
  # system("echo #{branch}")

  # heroku_url = ask "Heroku Git URL: "
  # system("echo #{heroku_url}")

  # route_name = ask "Preferred Route Name: "
  # system("echo #{route_name}")

  # #Get Hub for Git Commands
  system("brew install hub")

  # Clone and Fork Sinatra and Buildpack
  system("hub clone austenito/heroku-octopress-autodeploy sinatra")
  system("cd sinatra && hub fork && cd ..")
  system("hub clone austenito/heroku-buildpack-ruby-octopress buildpack")
  system("cd buildpack && hub fork && cd ..")

  # Configure buildpack
  system("cd buildpack")

  file_name = "./config.yml"
  text = File.read(file_name)
  text.gsub!("your blog", blog_git)
  File.open(file_name, "w") {|file| file.write(text) }
end

task :destroy do
  system("rm -Rf sinatra && rm -Rf buildpack")
end

def ask message
  print "#{message}"
  STDIN.gets.chomp
end