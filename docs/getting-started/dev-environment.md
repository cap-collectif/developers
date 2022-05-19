---
sidebar_position: 1
---

# Setting up our development environment

We provide and environment for both Linux and MacOS.
### What you'll need

- [Git](https://git-scm.com/)

- [Docker](https://www.docker.com/get-started/) version 20 or above and [Docker compose](https://docs.docker.com/compose/) version 2.1 or above:
  - Check your docker version using `docker -v` ;
  - Check your docker-component version using `docker-compose -v` ;
  - If you are using MacOS we recommend using [Docker Desktop on Mac](https://docs.docker.com/desktop/mac/install/).

- [Python](https://www.python.org/downloads/) version 3.9 or above and [pipenv](https://pypi.org/project/pipenv/):
  - Check your python version using `python -V`.
  - Check your pipenv version using `pipenv --version`.
  <!-- [Fabric](https://www.fabfile.org/installing.html) 2.6 or above and -->
  <!-- - Check your Fabric version using `fab -V`. -->

- [Node.js](https://nodejs.org/en/download/) version 17 or above and [yarn](https://yarnpkg.com/) package manager:
  - Check your NodeJS version using `node -v`.
  - Check your yarn version using `yarn -v`.

_For developers using MacOS we use the Symfony CLI for a better & faster Developer Experience. That's why we require MacOS users to install some PHP dependencies on the host, instead of using our docker container directly._

- [PHP](https://www.php.net/) version 7.4 and [composer](https://getcomposer.org/) version 2.1 or above and [Symfony CLI](https://symfony.com/download):
  - Check your PHP version using `php -v`.
  - The following PHP extensions must be installed and enabled `redis`, `imagick`, `amqp`.
  - Check your composer version using `composer --version`.
  - Check you Symfony CLI version using `symfony -V`.


### Getting started

#### 1. Clone the repository:

```bash
git clone git@github.com:cap-collectif/cap-collectif.git
```

#### 2. Using our development CLI

[Cap Collectif](https://github.com/cap-collectif/cap-collectif) is a huge repository using a complex infrastructure with many technologies. In order to unify as much as possible our development experience between CI, Linux and MacOS, we implemented a CLI to simplify our interactions with Docker containers.

Inside `cap-collectif` directory, you can run several Fabric tasks such as:

```bash
fab local.system.doctor
```

This task will dump the version of every dependency that you need ton install on your host.

:::caution

If running Fabric tasks does not work, make sur to run `pipenv shell` first.

:::

:::tip 

In order to list every available tasks you can use `fab -l`.

:::

#### 3. Setup your environment variables

We use environment variables a lot ! 

You can generate a default `.env.local` file that contains the required variables to launch the repository:

```
fab local.system.setup-env-var
```

:::info

To enable some services (like geolocation with Google Map) you will need to insert some credentials instead of `INSERT_A_REAL_SECRET` values  in your `.env.local` file.

<!-- TODO add some documentation… -->

:::

#### 4. Setup DNS and SSL

The development environment is only accessible via HTTPS and the following hostnames: `capco.dev`, `capco.test` and `capco.prod`. 

That's why you need to setup some virtual hosts:

```bash
fab local.system.configure-vhosts
```

This task will automatically update your `/etc/hosts` file.


```bash
fab local.system.sign-ssl
```

This task will generate all SSL certificates you need on your host machine.

#### 5. Build and launch our docker infrastructure

It's time to build and run our docker containers !

First build the docker cache and launch all the docker containers:

```bash
fab local.infrastructures.build
fab local.infrastructures.up
```

:::info

The first time you run those tasks, it will take quite some time… You may want a coffee break ?

:::

#### 6. Install code dependencies

You can run this task:

```bash
fab local.system.deploy
```

:::tip

For MacOS users, it's faster to run all those tasks on your host:

```bash
yarn install --purelockfile
composer install
php bin/console assets:install public --symlink
fab local.qa.compile-graphql
fab local.app.rabbitmq-queues
```

:::

#### 7. Add some development data

It's time to add some data !

The following task will create the database, setup the SQL schema, add some fixtures and populate our search indexes:

```bash
fab local.database.generate
```

:::tip 

Use this task, every time you want to reset your database or take a fresh start.

:::

#### 8. Setup the frontend

First, download the up to date translation files:

```bash
yarn trad
```

:::caution

Only Cap Collectif employees can add new translations right now.

:::

Before we can build our JS bundle, we need to generate some not committed files, run the compiler with:

```bash
yarn build-relay-schema
```

:::info

Our frontend code use [Relay](https://relay.dev/) which requires a compiling step that read our `*.graphql` and `*.js` files to create some `__generated__` directories and `*.graphql.js` files.

:::

Finally we can run Webpack to build our JS bundle:

```bash
yarn build
```

#### 9. Setup the MacOS http proxy

_For MacOS users only:_

The `fab local.infrastructures.up` launch the Symfony CLI proxy.

But you need and extra step to make sure every requests to `capco.dev` goes through the Symfony CLI proxy.

Run this command in a tab (you will need to keep it running):

```bash
cd infrastructure; python -m http.server 80
```

:::info

It will start a simple HTTP server to expose a `proxy.pac` file, that we will use to proxy the requests.

:::

Update your proxys configuration (under "Network") with "Proxy auto config" and value `http://localhost/proxy.pac`. 


:::tip

Make sur to update your proxys configuration for both **Wifi** and **LAN**.

:::

:::caution Some browsers require extra work

For Google chrome users, go to `chrome://net-internals/#proxy`, click `Re-apply settings` to apply the proxy.

:::
#### 10. Open your first page

It's time to open your browser with: https://capco.dev 

It should work ! 

In the next steps of this tutorial, we are going to explain how to work with every part of the stack.