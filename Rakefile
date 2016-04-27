require 'bundler/setup'
require 'git'
require 'meetup_client'
require 'erubis'
require './mocks/sample-meetup.rb'

task :generate_static_site do
  # read meatup events

  # generate posts
  # puts "sample_meetup.results=" + SAMPLE_MEETUP['results'].to_s;
  # posts << SAMPLE_MEETUP['results'].first['title'];

  template = File.read("templates/post.md.erb")
  template = Erubis::Eruby.new(template)

  SAMPLE_MEETUP['results'].map{|event| event['name']}.each { |title|
    puts template.result(post_title: title)
  }
  # /generate posts

  # push posts to git
end
