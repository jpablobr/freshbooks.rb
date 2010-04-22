require 'rubygems'
require 'rake'

begin
  require 'jeweler'
  Jeweler::Tasks.new do |gemspec|
    gemspec.name = "jpablobr-freshbooks"
    gemspec.summary = "jpablobr-freshBooks is a Ruby interface to the FreshBooks API."
    gemspec.description = "jpablobr-freshBooks is a Ruby interface to the FreshBooks API. It exposes easy-to-use classes and methods for interacting with your FreshBooks account."
    gemspec.email = "xjpablobrx@gmail.com"
    gemspec.homepage = "http://github.com/jpablobr/freshbooks.rb"
    gemspec.authors = ["Jose Pablo Barrantes"]
  end
  Jeweler::GemcutterTasks.new
rescue LoadError
  puts "Jeweler not available. Install it with: sudo gem install jeweler -s http://gemcutter.org"
end

Dir["#{File.dirname(__FILE__)}/tasks/*.rake"].sort.each { |ext| load ext }
