# frozen_string_literal: true

source 'https://rubygems.org'

# Uncomment the database that you have configured in config/database.yml
# ----------------------------------------------------------------------
db_drivers = {
  "mysql" => "mysql2",
  "sqlite" => "sqlite3"
}

#gem db_drivers[ENV['CI'] && ENV['DB']] || 'pg'

gem 'mysql2'
# Removes a gem dependency
def remove(name)
  @dependencies.reject! { |d| d.name == name }
end

# Replaces an existing gem dependency (e.g. from gemspec) with an alternate source.
def gem(name, *args)
  remove(name)
  super
end

# Bundler no longer treats runtime dependencies as base dependencies.
# The following code restores this behaviour.
# (See https://github.com/carlhuda/bundler/issues/1041)
spec = Bundler.load_gemspec(File.expand_path("../fat_free_crm.gemspec", __FILE__))
spec.runtime_dependencies.each do |dep|
  gem dep.name, *dep.requirement.as_list
end

# Remove premailer auto-require
gem 'premailer', require: false

# Remove fat_free_crm dependency, to stop it from being auto-required too early.
remove 'fat_free_crm'

group :development do
  # don't load these gems in travis
  unless ENV["CI"]
    gem 'puma'
    gem 'capistrano'
    gem 'capistrano-bundler'
    gem 'capistrano-rails'
    gem 'capistrano-rvm'
    gem 'guard'
    gem 'guard-rspec'
    gem 'guard-rails'
    gem 'rb-inotify', require: false
    gem 'rb-fsevent', require: false
    gem 'rb-fchange', require: false
  end
end

group :development, :test do
  gem 'rails-controller-testing'
  gem 'rspec-rails'
  gem 'rspec-activemodel-mocks'
  gem 'headless'
  gem 'byebug'
  gem 'pry-rails' unless ENV["CI"]
  gem 'factory_girl_rails', '~> 4.7.0' # 4.8.0+ stubbed models are not allowed to access the database - User#destroyed?()
  gem 'rubocop', '~> 0.52.0' # Pinned because upgrades require regenerating rubocop_todo.yml
  gem 'rainbow'
end

group :test do
  gem 'capybara'
  gem 'selenium-webdriver'
  gem 'chromedriver-helper'
  gem 'database_cleaner', '= 1.6.1'
  gem 'acts_as_fu'
  gem 'zeus', platform: :ruby unless ENV["CI"]
  gem 'timecop'
end

group :heroku do
  gem 'rails_12factor'
end

gem 'sass-rails'
gem 'coffee-rails'
gem 'uglifier'
gem 'execjs'
gem 'therubyracer', platform: :ruby unless ENV["CI"]
gem 'nokogiri', '>= 1.8.1'
gem 'activemodel-serializers-xml'
gem 'bootsnap', require: false
gem 'tzinfo-data', platforms: %i[mingw mswin x64_mingw jruby]