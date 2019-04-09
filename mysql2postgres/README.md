# mysql-to-postgres - MySQL to PostgreSQL Data Translation

[![Build Status](https://travis-ci.org/maxlapshin/mysql2postgres.svg)](https://travis-ci.org/maxlapshin/mysql2postgres)
[![Dependency Status](https://gemnasium.com/maxlapshin/mysql2postgres.svg)](https://gemnasium.com/maxlapshin/mysql2postgres)

MRI or jruby supported. The minimum Ruby version supported in `master` branch is `2.1.7`,
and the next release will have the same requirement.

With a bit of a modified rails `database.yml` configuration, you can integrate `mysql-to-postgres`into a project.

## Installation

**Currently failing, see #81...**

### Via RubyGems

```sh
gem install mysqltopostgres
```

### From source

```sh
git clone https://github.com/maxlapshin/mysql2postgres.git
cd mysql2postgres
bundle install
gem build mysqltopostgres.gemspec
sudo gem install mysqltopostgres-0.3.0.gem
```

## Sample Configuration

Configuration is written in [YAML format](http://www.yaml.org/ "YAML Ain't Markup Language")
and passed as the first argument on the command line.

```yaml
default: &default
  adapter: jdbcpostgresql
  encoding: unicode
  pool: 4
  username: terrapotamus
  password: default
  host: 127.0.0.1

development: &development
  <<: *default
  database: default_development

test: &test
  <<: *default
  database: default_test

production: &production
  <<: *default
  database: default_production

mysql_data_source: &pii
  hostname: localhost
  port: 3306
  socket: /tmp/mysqld.sock
  username: username
  password: default
  database: awesome_possum

mysql2psql:
  mysql:
    <<: *pii

  destination:
    production:
      <<: *production
    test:
      <<: *test
    development:
      <<: *development

  tables:
  - countries
  - samples
  - universes
  - variable_groups
  - variables
  - sample_variables

  # If suppress_data is true, only the schema definition will be exported/migrated, and not the data
  suppress_data: false

  # If suppress_ddl is true, only the data will be exported/imported, and not the schema
  suppress_ddl: true

  # If force_truncate is true, forces a table truncate before table loading
  force_truncate: false

  preserve_order: true

  remove_dump_file: true

  dump_file_directory: /tmp

  report_status: json    # false, json, xml

  # If clear_schema is true, the public schema will be recreated before conversion
  # The import will fail if both clear_schema and suppress_ddl are true.
  clear_schema: false
```

Please note that the MySQL connection will be using socket in case the host is not defined (`nil`) or it is `'localhost'`.

## Testing


## License

Licensed under [the MIT license](MIT-LICENSE).
