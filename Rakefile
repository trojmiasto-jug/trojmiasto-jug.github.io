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
      	format: 'json',
      	status: 'upcoming,past'
	}
	meetup_api = MeetupApi.new
	events = meetup_api.events(params)

  template = File.read("_templates/post.md.erb")
  template = Erubis::Eruby.new(template)

  events['results'].map{|event| event['name']}.each { |title|
    post_file_name = title.gsub(/[\ \x00\/\\:\*\?\"<>\|]/, '_')
    File.write("_posts/#{post_file_name}.md", template.result(post_title: title));
  }

  # push posts to git
#    callable_attr :remote_url do
#      url = `git config remote.origin.url`.strip.gsub(/^git:/, 'https:')
#      next url.gsub(%r{^https://([^/]+)/(.*)$}, 'git@\1:\2') if ssh_key_file?
#      next url.gsub(%r{^https://}, "https://#{ENV['GH_TOKEN']}@") if ENV.key? 'GH_TOKEN'
#      next url
#    end

  sh "git config --local user.name 'Travis'"
  sh "git config --local user.email 'jug@travis-ci.org'"

  g = Git.open('.')
  g.add(:all=>true)
  g.commit('Travis commit with meetup events')
  g.push
end
