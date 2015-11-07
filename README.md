# Start a Datomic Transactor

## Heroku Spaces (mandatory)

This technology exploits the features of [Heroku Spaces](https://www.heroku.com/private-spaces) to securely inter-connect dynos. 

To use this project you need to create you app in a Heroku Space.

### Custom buildpack (mandatory)

To enable this buildpack on an existing project:

````heroku buildpacks:set https://github.com/opengrail/heroku-buildpack-datomic -a myapp````

For more options and information see the [Heroku web site on third party buildpacks](https://devcenter.heroku.com/articles/third-party-buildpacks#using-a-custom-buildpack)

### Configuration

#### Datomic free configuration - playtime only!!

By default Datomic free will be started. *Data will not be stored between startups*

You can optionally configure the version of Datomic you wish to deploy (default is `0.9.5327`)

````heroku config:set DATOMIC_VERSION version-number````

#### Datomic Pro configuration (recommended)

You can get a free copy of the [Datomic Pro starter edition](http://www.datomic.com/get-datomic.html) or bring your supported license.

````heroku config:set DATOMIC_LICENSE_PASSWORD license-password````

````heroku config:set DATOMIC_LICENSE_USER license-user````

````heroku config:set DATOMIC_TRANSACTOR_KEY license-key````

You can optionally configure the version of Datomic you wish to deploy (default is `0.9.5327`)

````heroku config:set DATOMIC_VERSION version-number````

#### Heroku Addons - Pro editions only (recommended)

To run any of the Pro editions, this project needs a Postgres database. 

The buildpack used by the project only supports the `Heroku Postgres (free or paid)`

The buildpack will automatically configure the Postgres instance for use with Datomic (if not already done) 

### Dyno size (recommended)

Transactors require dynos with a minimum of 1Gb RAM in all cases. In production, 4Gb or more is preferred.

### Dyno count (optional)

To manage failover for the Pro edition (not starter or free), 2 Transactors can be started. 

This can be achieved manually with the [`Heroku toolbelt`](https://devcenter.heroku.com/articles/procfile#scaling-a-process-type)

If the lead transactor fails, the other worker will takeover.

When clients connect to the storage they will be automatically transitioned to the new lead transactor.

Check the [Datomic documentation](http://docs.datomic.com/ha.html) for more details on the HA strategy used by Datomic 

# And push...

With the necessary set-up done, pushing this project to your new app will start a Datomic Transactor with PostgresSQL as storage.