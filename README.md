# MONGO-SYX

Mongo-syx is a utility script able to copy one mongo database into another running on a different host.  
It is based on [mongo_sync](https://github.com/sheharyarn/mongo-sync) and uses mongodump and mongorestore under the hood.
The yml parser comes from [bash-yaml](https://github.com/jasperes/bash-yaml)

## Configuration

The configuration is divised in multiple files, each one describing one database on one environment.

A configuration file must be named following the pattern `config-${ENV_NAME}.yml`, where ENV_NAME is an arbitrary name that will be used to refer to this file.  

Here is what the structure of the file looks like: 

```YAML
db: admin
host:
  url: localhost
  port: 27017
access:
  username: admin
  password: admin

confirmation:
  write: true

tunnel:
  enable: false
  access:
    username: remote_ssh_user
    port: 22
```

 - `db`: name of the mongo database to reach
 - `host`: address of the machine hosting the database
 - `access`: credentials to log into the database
 - `confirmation`: optionally activate a strong confirmation for a given operation on the database (supported operations ATM: `write`)
 - `tunnel`: optional ssh tunnel to connect to the host

## Usage

`./mongo-syx origin destination`

This command will tell mongo-syx to read from `origin` and write to `destination`.  
To do this, it will read the configuration files `config-origin.yml` and `config-destination.yml`.

There are two level of confirmation asking before the script starts its work.  
The first one is a soft confirmation skippable with a `-y` arg.  
The second one is a strong and unskippable confirmation that can be enabled from the configurations.

## Notes

 - `mongo-syx` requires `mongodump` and `mongorestore` to be installed in your system
 - The script ***overwrites*** the target DB

