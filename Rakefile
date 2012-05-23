APPNAME = 'ember-jquery-ui'

require 'colored'
require 'rake-pipeline'

desc "Build #{APPNAME}"
task :build do
  Rake::Pipeline::Project.new('Assetfile').invoke
end

desc "Run tests with PhantomJS"
task :test => :build do
  unless system("which phantomjs > /dev/null 2>&1")
    abort "PhantomJS is not installed. Download from http://phantomjs.org/"
  end

  cmd = "phantomjs tests/qunit/run-qunit.js \"file://#{File.dirname(__FILE__)}/tests/index.html\""

  # Run the tests
  puts "Running #{APPNAME} tests"
  success = system(cmd)

  if success
    puts "Tests Passed".green
  else
    puts "Tests Failed".red
    exit(1)
  end
end

desc "Automatically run tests (Mac OS X only)"
task :autotest do
  system("kicker -e 'rake test' app")
end

desc "Deploy app on GitHub pages"
task :deploy do
  `rm -rf build`
  `mkdir build`
  origin = `git config remote.origin.url`.chomp
  ENV["RAKEP_MODE"] = "production"
  Rake::Task["build"].invoke
  Dir.chdir "build" do
    `git init`
    `git remote add origin #{origin}`
    `git checkout -b gh-pages`
    `cp ../index.html .`
    `cp -r ../assets .`
    `rm assets/app-tests.js`
    `git add .`
    `git commit -m 'Site updated at #{Time.now.utc}'`
    `git push -f origin gh-pages`
  end
  `rm -rf build`
end