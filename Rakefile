# -*- encoding: utf-8 -*-
require 'rubygems'
begin
  require 'bundler/setup'
rescue LoadError
  puts 'You must `gem install bundler` and `bundle install` to run rake tasks'
end

require 'rake'
require 'rake/testtask'
require 'rake/rdoctask'
require "bundler/gem_tasks"

desc 'Test the ubiquo_scaffold gem.'
Rake::TestTask.new(:test) do |t|
  t.libs << 'lib'
  t.pattern = 'test/**/*_test.rb'
  t.verbose = true
end

desc 'Default: run unit tests.'
task :default => :test

desc 'Generate documentation for the ubiquo_scaffold gem.'
Rake::RDocTask.new(:rdoc) do |rdoc|
  rdoc.rdoc_dir = 'rdoc'
  rdoc.title    = 'UbiquoScaffold'
  rdoc.options << '--line-numbers' << '--inline-source'
  rdoc.rdoc_files.include('README')
  rdoc.rdoc_files.include('lib/**/*.rb')
end

def update_version(function, name)
  major, minor, tiny = File.read("VERSION").strip.split(".").map { |i| i.to_i }
  eval "#{name} #{function}= 1"
  File.open("VERSION", "w") { |f| f.puts [major, minor, tiny].join(".") }
  puts `cat VERSION`
end

{ :bump => "+", :debump => "-"}.each do |key, value|
  namespace key do
    [:major, :minor, :tiny].each do |position|
      eval <<-CODE
        desc "#{key.to_s.capitalize} #{position} number by 1"
        task :#{position} do
          update_version("#{value}", "#{position}")
        end
      CODE
    end
  end
end
