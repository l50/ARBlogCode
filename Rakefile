#!usr/bin/env ruby
require 'active_record'
require 'yaml'
require 'logger'

namespace :db do
  task :environment do
    # Take in specified database as an argument
    DB = ENV['db']
    MIGRATIONS_DIR = 'db/migrate/' + DB
  end

  task :configuration => :environment do
    @config = YAML.load_file('config/database.yml')[DB]
  end

  task :configure_connection => :configuration do
    ActiveRecord::Base.establish_connection @config
    ActiveRecord::Base.logger = Logger.new(File.open('logs/db.log', 'a'))
  end

  desc 'Migrate the database (options: VERSION=x, VERBOSE=false, db=<name of database>).'
  task :migrate => :configure_connection do
    ActiveRecord::Migration.verbose = true
    ActiveRecord::Migrator.migrate MIGRATIONS_DIR, ENV['VERSION'] ? ENV['VERSION'].to_i : nil
  end

  desc 'Rolls the schema back to the previous version (specify steps w/ STEP=n).'
  task :rollback => :configure_connection do
    step = ENV['STEP'] ? ENV['STEP'].to_i : 1
    ActiveRecord::Migrator.rollback MIGRATIONS_DIR, step
  end
end