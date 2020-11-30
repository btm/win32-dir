require "rake/clean"
require "rake/testtask"

CLEAN.include("**/*.gem", "**/*.log")

namespace "gem" do
  desc "Build the win32-dir gem"
  task create: [:clean] do
    require "rubygems/package" unless defined?(Gem::Package)
    Dir["*.gem"].each { |f| File.delete(f) }
    spec = eval(IO.read("win32-dir.gemspec"))
    Gem::Package.build(spec)
  end

  desc "Install the win32-dir gem"
  task install: [:create] do
    file = Dir["*.gem"].first
    sh "gem install -l #{file}"
  end
end

desc "Run the example program"
task :example do
  sh "ruby -Ilib examples/dir_example.rb"
end

Rake::TestTask.new do |t|
  t.warning = true
  t.verbose = true
end

begin
  require "yard" unless defined?(YARD)
  YARD::Rake::YardocTask.new(:docs)
rescue LoadError
  puts "yard is not available. bundle install first to make sure all dependencies are installed."
end

begin
  require "chefstyle"
  require "rubocop/rake_task"
  desc "Run Chefstyle tests"
  RuboCop::RakeTask.new(:style) do |task|
    task.options += ["--display-cop-names", "--no-color"]
  end
rescue LoadError
  puts "chefstyle gem is not installed. bundle install first to make sure all dependencies are installed."
end

task :console do
  require "irb"
  require "irb/completion"
  ARGV.clear
  IRB.start
end

task default: :test
