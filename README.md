# Grunt (MYSQL) Database Dumps and Imports

> Dump and import local or remote MYSQL databases using Grunt.

**IMPORTANT NOTE**: the authors of this Plugin assume **no responsibility** for any actions which result from the usage of this script. Specifically, the integrity of the generated dumps cannot be guaranteed.
Use this plugin *at your own risk*. It is *strongly* recommended that you test the script in a non-critical environment prior to rolling out for production use. Make sure it generates dumps that match your requirements before relying on it as a backup solution.

## Getting Started
This plugin requires Grunt `~0.4.1`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-mysql-dump-import --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-mysql-dump-import');
```

## Documentation

### Overview
In your project's Gruntfile, add a section named `db_dump` and `db_import` to the data object passed into `grunt.initConfig()`.

The tasks expect a series of `targets`, one for each of the locations which you want to dump the database from.

```js
grunt.initConfig({
  db_dump: {
    local: {

    },
    my_target_1: {

    },
    my_target_2: {

    },
    // etc
  },
  db_import: {
    local: {

    },
    my_target_1: {

    },
    my_target_2: {

    },
    // etc
  }
});
```


### Available Tasks

The plugin makes two new tasks available via Grunt: `db_dump` and `db_import`. The interface for these commands are:

```shell
grunt db_dump:target_name
```

This will dump the database according to the options found in the `target_name` target.
You can dump databases for all targets by simply calling `grunt db_dump`.

```shell
grunt db_import:target_name
```

This will import the dumpfile according to the options found in the `target_name` target.


### Usage

You *must* define targets as shown above. Each target defines the connection information to the targeted database and the destination file name of the generated dump:

```js
"local": {
  "options": {
    "title": "Local DB",
  
    "database": "db_name",
    "user": "db_username",
    "pass": "db_password",
    "host": "db_host",
  
    "backup_to": "/db/backups/local.sql"
  }
},
```

You can also connect to a remote host using SSH by specifying a `ssh_host` option in your target:

```js
"production": {
  "options": {
    "title": "Production DB",
  
    "database": "db_name",
    "user": "db_username",
    "pass": "db_password",
    "host": "db_host",
    
    "ssh_host": "db_ssh_host",
    
    "backup_to": "/db/backups/production.sql"
  }
}
```



#### Full Usage Example

The structure below represents a typical usage example for the task configuration. Obviously you should replace the placeholders with your own database/environment configurations.

```js
grunt.initConfig({
  // Load database config from external JSON (optional)
  db_config: grunt.file.readJSON('config/db.json'),

  db_import: {
    options: {
      // common options should be defined here
    },
    
    // "Local" target
    "local": {
      "options": {
          "title": "Local DB",
        
        "database": "<%= db_config.local.db_name %>",
        "user": "<%= db_config.local.username %>",
        "pass": "<%= db_config.local.password %>",
        "host": "<%= db_config.local.host %>",
        
        "backup_to": "/db/backups/local.sql"
      }
  },

  db_dump: {
    options: {
      // common options should be defined here
    },
    
    // "Local" target
    "local": {
      "options": {
          "title": "Local DB",
        
        "database": "<%= db_config.local.db_name %>",
        "user": "<%= db_config.local.username %>",
        "pass": "<%= db_config.local.password %>",
        "host": "<%= db_config.local.host %>",
        
        "backup_to": "/db/backups/local.sql"
      }
    },
    
    "stage": {
      "options": {
        "title": "Production DB",
        
        "database": "<%= db_config.stage.db_name %>",
        "user": "<%= db_config.stage.username %>",
        "pass": "<%= db_config.stage.password %>",
        "host": "<%= db_config.stage.host %>",
        
        "backup_to": "/db/backups/stage.sql"
      }
    },
    
    "production": {
      "options": {
        "title": "Production DB",
        
        "database": "<%= db_config.production.db_name %>",
        "user": "<%= db_config.production.username %>",
        "pass": "<%= db_config.production.password %>",
        "host": "<%= db_config.production.host %>",
        
        "ssh_host": "<%= db_config.production.ssh_host %>",
        
        "backup_to": "/db/backups/production.sql"
      }
    }
  },
})
```

### Configuration

Each target expects a series of configuration options to be provided to enable the task to function correctly. These are detailed below:

#### title
Type: `String`  
Description: A proper case name for the target. Used to describe the target to humans in console output whilst the task is running.

#### database
Type: `String`  
Description: the name of the database for this target.

#### user
Type: `String`  
Description: the database user with permissions to access the database

#### pass
Type: `String`  
Default: *(empty password)*  
Description: the password for the database user (above)

#### host
Type: `String`  
Description: the hostname for the location in which the database resides. Typically this will be `localhost`

#### port
Type: `Integer`  
Default: `3306`  
Description: the port the mysql server listens to

#### ssh_host
Type: `String`  
Description: ssh connection string in the format `SSH_USER@SSH_HOST`. The task assumes you have ssh keys setup which allow you to remote into your server without requiring the input of a password. As this is an exhaustive topic we will not cover it here but you might like to start by reading [Github's own advice](https://help.github.com/articles/generating-ssh-keys).

#### backup_to
Type: `String`
Default: `"db/backups/<%= grunt.template.today('yyyy-mm-dd') %> - <%= grunt.task.current.target %>.sql"`
Description: full destination file path of the generated dump. This option can include templates such as `<%= grunt.template.today('yyyy-mm-dd') %>` or `<%= grunt.task.current.target %>`.

Within the dumptask, this is the location of the file that will be imported.

### Options

*No global option as of now*

## Contributing

Contributions to this plugin are most welcome. This is very much a Alpha release and so if you find a problem please consider raising a pull request or creating a Issue which describes the problem you are having and proposes a solution.

In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code using [Grunt](http://gruntjs.com/).

## Release History

* 2015-04-30   v1.1.1 Added DB importing ability
* 2015-02-20   v1.1.0 Fixes on password argument in command line - Using -hex-blob option for mysqldump call - ability to add extra arguments to command line call
* 2013-11-27   v1.0.0   Releasing. Yeah.
* 2013-11-27   v0.0.1   Plugin made private until it reaches a publishable state. Update README and package info.

### grunt-deployments history
* 2013-11-12   v0.2.0   Fix escaping issues, ability to define `target` via options, README doc fixes, pass host param to mysqldump.
* 2013-06-11   v0.1.0   Minor updates to docs including addtion of Release History section.
* 2013-06-11   v0.0.1   Initial Plugin release.

## License

This plugin is a stripped down and modified version of @getdave's plugin [grunt-deployments](https://github.com/getdave/grunt-deployments)
It is released under the same MIT license, as found in the LICENSE-MIT file.
*Copyright (c) 2013 Digital Cuisine*


## Contributors

* Olivier Lance (@olance/@digitalcuisine)
* @craigloftus
* @zonito
