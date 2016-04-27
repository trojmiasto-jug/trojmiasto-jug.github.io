require 'bundler/setup'
require 'git'
require 'meetup_client'
require 'erubis'
require 'date'

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

  events['results'].each { |event|
    title = event['name']
    description = event['description']
    datetime = Time.at(event['time'] / 1000);
    post_file_name = datetime.strftime("%Y-%m-%d") + '-' + title.gsub(/[\#\ \x00\/\\:\*\?\"<>\|]/, '_')
    File.write("_posts/#{post_file_name}.md", template.result(post_title: title, date: datetime.to_s, description: description))
  }

  # push posts to git
  g = Git.open('.')
  g.config('user.name', 'Travis')
  g.config('user.email', 'jug@tavis-ci.org')
  g.config('remote.origin.url', 
    "https://#{ENV['GH_TOKEN']}@github.com/trojmiasto-jug/trojmiasto-jug.github.io.git")
  g.add(:all=>true)
  g.commit('Travis commit with meetup events')
  g.push
end
