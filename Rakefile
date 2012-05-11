require 'yaml'

desc "Set up auto-deploy script"
task :setup do
  notify_user "Thanks for using our Jekyll auto-generate/deploy script.\nYou'll be up and running in no time, just a few questions first..."
  set_user_information
  install_hub
  notify_user "Setting up your custom heroku buildpack"
  setup_buildpack
  notify_user "Setting up your sinatra app"
  setup_sinatra
  notify_user "Pushing your sinatra app to listen on heroku"
  push_sinatra_to_heroku
  clean_up
end

private

def ask message
  print "#{message}"
  STDIN.gets.chomp
end

def silent_system_call(command)
  system(command, :out => ["log", "w"])
end

def notify_user(notification)
  puts "*************"
  puts notification
  puts "*************"
end

def set_user_information
  @user_name = get_info("Github Username")
  @blog_git = get_info("Github READ ONLY Blog Url")
  heroku_name = get_info "Name of Heroku App"
  @heroku_url = heroku_name_to_git_url(heroku_name)
  @route_name = get_info "Preferred Route Name"
end

def get_info(prompt)
  (ask "#{prompt}: ").tap do |response|
    system("echo #{response}")
  end
end

def heroku_name_to_git_url(heroku_name)
  "git@heroku.com:#{heroku_name}.git"
end

def install_hub
  silent_system_call("brew install hub")
end

def setup_buildpack
  silent_system_call("hub clone austenito/heroku-buildpack-ruby-octopress buildpack")
  Dir.chdir("buildpack")
  silent_system_call("hub fork")
  silent_system_call("git remote add upstream git@github.com:#{@user_name}/heroku-buildpack-ruby-octopress.git")
  file_name = "config.yml"
  text = File.read(file_name)
  text.gsub!("your blog", @blog_git)
  File.open(file_name, "w") {|file| file.write(text) }
  silent_system_call("rake setup")
  silent_system_call("git add .")
  silent_system_call("git commit -m 'Prepare config for auto-deploy'")
  silent_system_call("git push --force upstream master")
  return_to_root
end

def setup_sinatra
  silent_system_call("hub clone austenito/heroku-octopress-autodeploy sinatra")
  Dir.chdir("sinatra")
  silent_system_call("git remote add heroku #{@heroku_url}")
  silent_system_call("heroku config:add BUILDPACK_URL=git://github.com/#{@user_name}/heroku-buildpack-ruby-octopress.git")
  file_name = "config.yml"
  text = File.read(file_name)
  text.gsub!("foo", @route_name)
  File.open(file_name, "w") {|file| file.write(text) }
  silent_system_call("rake --rakefile setup.rake")
  silent_system_call("git add .")
  silent_system_call("git commit -m 'Prepare config for auto-deploy'")
end

def push_sinatra_to_heroku
  silent_system_call("git push --force heroku master")
  return_to_root
end

def clean_up
  silent_system_call("rm -Rf sinatra && rm -Rf buildpack")
  system("rm log")
end

def return_to_root
  Dir.chdir("..")
end