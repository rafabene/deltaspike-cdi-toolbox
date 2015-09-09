desc 'Setup the environment to run Awestruct'
task :setup => [:bundle_install, :git_setup] do |task, args|
  # Don't execute any more tasks, need to reset env
  #   exit 0
end


task :bundle_install do |task, args|
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
task :generate do |task, args|
  system 'asciidoctor -r asciidoctor-diagram -T asciidoctor-backends/slim -a data-uri -a linkcss! slides.adoc'
  puts "slides.html generated."
end

desc 'Execute guard gem'
task :preview do |task, args|
  system 'open slides.html'
  system 'bundle exec guard'
end

