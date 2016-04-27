require 'bundler/setup'
require 'git'
require 'meetup_client'
require 'erubis'

task :generate_static_site do
  # read meatup events

  # generate posts
  template = File.read("templates/post.md.erb")
  template = Erubis::Eruby.new(template)
  puts template.result(post_title: "Test title")

  # push posts to git
end
