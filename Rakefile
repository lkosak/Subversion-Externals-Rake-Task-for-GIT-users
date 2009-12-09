require(File.join(File.dirname(__FILE__), 'config', 'boot'))

require 'rake'
require 'rake/testtask'
require 'rake/rdoctask'
require 'tasks/rails'

module Externals
  def dependencies
    return {} unless File.exist?(".svn_externals")
    file = File.new(".svn_externals", "r")
    deps = {}
    while (line = file.gets)
      path,src = line.chomp.split(/ /)
      deps[path] = src
    end
    deps
  end
  
  def grab (path, src)
    if File.exist?(path)
      puts "#{path} already exists"
      return
    end
    cmd = "svn checkout #{src} #{path}"
    puts "Fetching #{path}"
    `#{cmd}`
  end
  
  def sync (path)
    puts "Updating #{path}"
    puts `svn update #{path}`
  end
end

namespace :externals do
  
  include Externals
  
  desc "Run Initial Import of Externals"
  task :init do
    dependencies.each do |path,src|
      dependencies.grab(path,src)
    end
  end
  
  desc "Update Externals"
  task :update do
    dependencies.each do |path,src|
      if !File.exist?(path)
        dependencies.grab(path,src)
      else
        dependencies.sync(path)
      end
    end
  end
  
  desc "Update Marketing"
  task :update_marketing do
    dependencies.sync('application/modules/subscribe/views')
  end
end
