require 'bundler/setup'
require 'git'
require 'meetup_client'
require 'erubis'
require './mocks/sample-meetup.rb'

task :generate_static_site do
  # read meatup events

  # generate posts
  # puts "sample_meetup.results=" + SAMPLE_MEETUP['results'].to_s;
  # /generate posts

  template = File.read("templates/post.md.erb")
  template = Erubis::Eruby.new(template)
  puts template.result(post_title: "Test title")

  # push posts to git
end
