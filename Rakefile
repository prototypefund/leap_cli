require "pty"
require "fileutils"
require "rake/testtask"

##
## HELPER
##

def run(cmd)
  PTY.spawn(cmd) do |output, input, pid|
    begin
      while line = output.gets do
        puts line
      end
    rescue Errno::EIO
    end
  end
rescue PTY::ChildExited
end

##
## GEM BUILDING AND INSTALLING
##

$spec_path = 'leap_cli.gemspec'
$base_dir  = File.dirname(__FILE__)
$spec      = eval(File.read(File.join($base_dir, $spec_path)))
$gem_path  = File.join($base_dir, 'pkg', "#{$spec.name}-#{$spec.version}.gem")

require File.join([$base_dir, 'lib','leap_cli','load_paths'])

def built_gem_path
  Dir[File.join($base_dir, "#{$spec.name}-*.gem")].sort_by{|f| File.mtime(f)}.last
end

desc "Build #{$spec.name}-#{$spec.version}.gem into the pkg directory"
task 'build' do
  FileUtils.mkdir_p(File.join($base_dir, 'pkg'))
  FileUtils.rm($gem_path) if File.exist?($gem_path)
  run "gem build -V '#{$spec_path}'"
  file_name = File.basename(built_gem_path)
  FileUtils.mv(built_gem_path, 'pkg')
  puts "#{$spec.name} #{$spec.version} built to pkg/#{file_name}"
end

desc "Install #{$spec.name}-#{$spec.version}.gem into either system-wide or user gems"
task 'install' do
  if !File.exist?($gem_path)
    puts("Could not file #{$gem_path}. Try running 'rake build'")
  else
    options = '--verbose --conservative --no-rdoc --no-ri'
    if ENV["USER"] == "root"
      run "gem install #{options} '#{$gem_path}'"
    else
      home_gem_path = Gem.path.grep(/home/).first
      puts "You are installing as an unprivileged user, which will result in the installation being placed in '#{home_gem_path}'."
      print "Do you want to continue installing to #{home_gem_path}? [y/N] "
      input = STDIN.readline
      if input =~ /[yY]/
        run "gem install #{$gem_path} #{options} --install-dir '#{home_gem_path}' "
      else
        puts "bailing out."
      end
    end
  end
end

desc "Uninstall #{$spec.name}-#{$spec.version}.gem from either system-wide or user gems"
task 'uninstall' do
  if ENV["USER"] == "root"
    puts "Removing #{$spec.name}-#{$spec.version}.gem from system-wide gems"
    run "gem uninstall '#{$spec.name}' --version #{$spec.version} --verbose -x -I"
  else
    puts "Removing #{$spec.name}-#{$spec.version}.gem from user's gems"
    run "gem uninstall '#{$spec.name}' --version #{$spec.version} --verbose --user-install -x -I"
  end
end

##
## TESTING
##

Rake::TestTask.new do |t|
  t.pattern = "test/unit/*_test.rb"
  t.warning = false
end
task :default => :test

##
## DOCUMENTATION
##

#
# NOTE: this does not currently work!
#
# to export the docs, run `leap _doc --format=LeapCli::MarkdownDocumentListener`
# manually from within a provider directory.
#
#desc "Dumps all command line options to doc/leap.md"
#task :doc do
#  Dir.chdir($base_dir + "/doc") do
#    if system('../bin/leap _doc --format=LeapCli::MarkdownDocumentListener')
#      puts "updated doc/leap.md"
#    else
#      puts "error"
#    end
#  end
#end
