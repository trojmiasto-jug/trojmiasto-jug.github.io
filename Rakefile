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
	# Header may not contain colon sign in title as jekyll will mess up whole page. 
	# Colon needs to be escaped either as HTML entity: '&#58;' or whole title must be wrapped in quotes
	#
	# Former is good as allows to use only one variable, but '&#58;' will show up in page title, which is not looking nice.
	# Later approach requires two variables, but allows for control of how we want to display the page
    escaped_title = if title.include? ":" then '\''+title+'\'' else title end

    description = event['description']
    datetime = Time.at(event['time'] / 1000);
    post_file_name = datetime.strftime("%Y-%m-%d") + '-' + title.gsub(/[\#\ \x00\/\\:\*\?\"<>\|]/, '_')
    File.write("_posts/#{post_file_name}.md", template.result(post_title: title, escaped_title: escaped_title, date: datetime.to_s, description: description))
  }
end
