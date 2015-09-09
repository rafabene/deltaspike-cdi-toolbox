desc 'Setup the environment to run Awestruct'
task :setup => [:bundle_install, :git_setup] do
  # Don't execute any more tasks, need to reset env
  exit 0
end


task :bundle_install do
  if File.exist? 'Gemfile'
    system 'bundle install'
  else
    $install_gems.each do |gem|
      puts"Installing #{gem}..."
      system "gem install #{gem}"
    end
  end
end

desc 'Initialize any git submodules'
task :git_setup do
  system 'git submodule foreach \'git fetch --tags\''
  system 'git submodule update --init'
end

desc 'Build and preview the slides locally in development mode'
task :generate => [:check] do
  system 'asciidoctor -r asciidoctor-diagram -T asciidoctor-backends/slim -a data-uri -a linkcss! slides.adoc'
  puts "slides.html generated."
end

desc 'Execute guard gem'
task :preview  => [:check] do
  Rake::Task["generate"].execute
  system 'open slides.html'
  system 'bundle exec guard'
end

desc 'Check to ensure the environment is properly configured'
task :check do

  begin
    unless File.directory?("dzslides") && File.directory?("asciidoctor-backends")
      puts "Git submodules not initialized. Run rake setup"
      exit 1
    end
  end

  begin
    require 'bundler'
    Bundler.setup
  rescue LoadError
    $use_bundle_exec = false
  rescue StandardError => e
    puts e.message
    exit e.status_code
  end

end

