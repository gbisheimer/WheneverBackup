## Sample configuration for PostgreSQL

### Dockerfile
```
FROM byscontrol/wheneverbackup:postgres-9.4
MAINTAINER ByS Control "info@bys-control.com.ar"
```


### models/psql_backup.rb
```RUBY
# encoding: utf-8

##
# Backup Generated: byserp_backup
# Once configured, you can run the backup with the following command:
#
# $ backup perform -t byserp_backup [-c <path_to_configuration_file>]
#
# For more information about Backup's components, see the documentation at:
# http://meskyanichi.github.io/backup
#
Model.new(:psql_backup, 'Description for psql_backup') do

  ##
  # PostgreSQL [Database]
  #
  database PostgreSQL do |db|
    # To dump all databases, set `db.name = :all` (or leave blank)
    db.name               = "database"
    db.username           = "postgres"
    db.password           = "postgres"
    db.host               = "db"
    db.port               = 5432
    #db.socket             = "/tmp/pg.sock"
    # When dumping all databases, `skip_tables` and `only_tables` are ignored.
    #db.skip_tables        = ["skip", "these", "tables"]
    #db.only_tables        = ["only", "these", "tables"]
    #db.additional_options = ["-xc", "-E=utf8"]
  end

  ##
  # SCP (Secure Copy) [Storage]
  #
  # store_with SCP do |server|
  #   server.username   = "username"
  #   server.password   = "password"
  #   server.ip         = "www.server.com"
  #   server.port       = 22
  #   server.path       = "~/backups/"
  #   server.keep       = 5

    # Additional options for the SSH connection.
    # server.ssh_options = {}
  #end

  ##
  # OpenSSL [Encryptor]
  #
  encrypt_with OpenSSL do |encryption|
    encryption.password      = "password"            # From String
    #encryption.password_file = "/path/to/password/file" # Or from File
    encryption.base64        = true
    encryption.salt          = true
  end

  ##
  # Gzip [Compressor]
  #
  compress_with Gzip

  ##
  # Mail [Notifier]
  #
  # The default delivery method for Mail Notifiers is 'SMTP'.
  # See the documentation for other delivery options.
  #
  notify_by Mail do |mail|
    mail.on_success           = false
    mail.on_warning           = true
    mail.on_failure           = true

    mail.from                 = "no-reply@domain.com"
    mail.to                   = "destination@domain.com"
    mail.address              = "smtp.domain.com"
    mail.port                 = 587
    mail.domain               = "domain.com"
    mail.user_name            = "username"
    mail.password             = 'password'
    mail.authentication       = "plain"
    #mail.encryption           = :starttls
  end

end
```


### config/schedule.rb
```RUBY
# Use this file to easily define all of your cron jobs.
#
# It's helpful, but not entirely necessary to understand cron before proceeding.
# http://en.wikipedia.org/wiki/Cron

# Example:
#
# set :output, "/path/to/my/cron_log.log"
#
# every 2.hours do
#   command "/usr/bin/some_great_command"
#   runner "MyModel.some_method"
#   rake "some:great:rake:task"
# end
#
every 1.day, :at=>'20:00 pm' do
  command "backup perform --trigger psql_backup"
end

# Learn more: http://github.com/javan/whenever
```
