## CloudFoundry PHP setup for TinyQueries

This is an application which can be used to setup TinyQueries on CloudFoundry using the [PHP Build Pack].

It is an out-of-the-box implementation of [TinyQueries PHP-libs v3.0.7.1] and is meant to be used together with the TinyQueries compile service to build a REST-api by only defining queries.

### Usage

1. Clone the app (i.e. this repo).

  ```bash
  git clone https://github.com/wdiesveld/cf-init-tinyqueries
  cd cf-init-tinyqueries
  ```

1. If you don't have one already, create a SQL database service. With Pivotal Web Services, the following command will create a free MySQL database through [ClearDb]. Currently you can either use MySQL or PostgreSQL.

  ```bash
  cf create-service cleardb spark my-test-sql-db
  ```

1. If you don't have one already, create a TinyQueries project. With Pivotal Web Services, the following command will create a free TinyQueries project through [TinyQueries].

  ```bash
  cf create-service tinyqueries free my-test-tinyqueries-project
  ```

1. Edit the manifest.yml file.  Change the 'host' attribute to something unique. Then under "services:" change "my-test-sql-db" to the name of your SQL database service. This is the name of the service that will be bound to your application and thus available to this application. Do the same for "my-test-tinyqueries-project"

1. Push it to CloudFoundry.

  ```bash
  cf push
  ```

1. After the application is deployed you can use the TinyQueries IDE to create and publish queries to this application.
  
### How It Works

When you push the application here's what happens.

1. The local bits are pushed to your target. It includes the changes we made and a build pack extension for TinyQueries.
1. The server downloads the [PHP Build Pack] and runs it.  This installs HTTPD and PHP.
1. The build pack sees the extension that we pushed and runs it.  The extension downloads the stock TinyQueries file from GitHub, unzips it and installs it into the `htdocs` directory.  It then copies the rest of the files that we pushed and replaces the default TinyQueries files with them. 
1. At this point, the build pack is done and CF runs our droplet.

[TinyQueries PHP-libs v3.0.7.1]:https://github.com/wdiesveld/tiny-queries-php-api/releases/tag/v3.0.7.1
[TinyQueries]:http://www.tinyqueries.com
[PHP Build Pack]:https://github.com/dmikusa-pivotal/cf-php-build-pack
[ClearDb]:https://www.cleardb.com/


