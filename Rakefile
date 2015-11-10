require 'bundler/gem_tasks'

# Don't push the gem to rubygems
ENV["gem_push"] = "false" # Utilizes feature in bundler 1.3.0

require 'appraisal'

require 'rspec/core/rake_task'
RSpec::Core::RakeTask.new(:spec) do |spec|
  spec.pattern = FileList['spec/**/*_spec.rb']
end

task :default => :spec

require 'yard'
YARD::Rake::YardocTask.new

desc 'Start Pry with runtime dependencies loaded'
task :console, :script do |t, args|
  command  = 'bundle exec pry'
  command += "-r #{args[:script]}" if args[:script]
  sh command
end


# Let bundler's release task do its job, minus the push to Rubygems,
# and after it completes, use "gem inabox" to publish the gem to our
# internal gem server.
Rake::Task["release"].enhance do
  spec = Gem::Specification::load(Dir.glob("*.gemspec").first)
  sh "gem inabox pkg/#{spec.name}-#{spec.version}.gem"
end
