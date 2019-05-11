# encoding: utf-8

require 'rubygems'
require 'bundler'
begin
  Bundler.setup(:default, :development)
rescue Bundler::BundlerError => e
  $stderr.puts e.message
  $stderr.puts "Run `bundle install` to install missing gems"
  exit e.status_code
end
require 'rake'

require 'jeweler'
Jeweler::Tasks.new do |gem|
  # gem is a Gem::Specification... see http://docs.rubygems.org/read/chapter/20 for more options
  gem.name = "i2c2"
  gem.homepage = "http://github.com/dx7/i2c2"
  gem.license = "MIT"
  gem.summary = %Q{csshX like - cluster ssh using iTerm2 panes - based on i2cssh}
  gem.description = %Q{csshX like - cluster ssh using iTerm2 panes - based on i2cssh}
  gem.email = "dx7@pm.me"
  gem.authors = ["dx7", "Wouter de Bie"]
  # dependencies defined in Gemfile
  gem.add_dependency 'rb-scpt', "~> 1.0.1"
end
Jeweler::RubygemsDotOrgTasks.new

require 'rake/testtask'
Rake::TestTask.new(:test) do |test|
  test.libs << 'lib' << 'test'
  test.pattern = 'test/**/test_*.rb'
  test.verbose = true
end

task :default => :test

require 'rdoc/task'
Rake::RDocTask.new do |rdoc|
  version = File.exist?('VERSION') ? File.read('VERSION') : ""

  rdoc.rdoc_dir = 'rdoc'
  rdoc.title = "i2c2 #{version}"
  rdoc.rdoc_files.include('README*')
  rdoc.rdoc_files.include('lib/**/*.rb')
end
