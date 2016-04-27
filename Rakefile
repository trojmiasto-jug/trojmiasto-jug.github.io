require 'bundler/setup'
require 'git'
require 'meetup_client'
require 'erubis'

task :generate_static_site do
	MeetupClient.configure do |config|
		config.api_key = ENV['MEETUP_KEY']
	end

	params = {
		group_urlname: 'Trojmiasto-Java-User-Group',
    format: 'json'
	}
	meetup_api = MeetupApi.new
	events = meetup_api.events(params)

  template = File.read("_templates/post.md.erb")
  template = Erubis::Eruby.new(template)

  events['results'].map{|event| event['name']}.each { |title|
    puts template.result(post_title: title)
  }
  
  # /generate posts

  # push posts to git
end
