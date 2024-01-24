---
sidebar_position: 1
---

# Setting up our development environment

We provide and environment for both Linux and MacOS.

### What you'll need

- [Git](https://git-scm.com/)

- [mkcert](https://github.com/FiloSottile/mkcert)

- [Docker](https://www.docker.com/get-started/) version 20 or above and [Docker compose](https://docs.docker.com/compose/) version 2.1 or above:

  - Check your docker version using `docker -v` ;
  - Check your docker-component version using `docker-compose -v` ;

:::tip

docker-compose is now included in docker engine, but our scripts still need `docker-compose` command.

Check if a version corresponds to your environment on this page: https://github.com/docker/compose/releases/tag/v2.17.0 
Run following commands (ajusting first url if needed):

```bash
curl -L https://github.com/docker/compose/releases/download/v2.17.0/docker-compose-linux-x86_64 > docker-compose
sudo mv docker-compose /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo chown root: /usr/local/bin/docker-compose
```

:::

  - If you are using MacOS we recommend using [Docker Desktop on Mac](https://docs.docker.com/desktop/mac/install/).

- [Python](https://www.python.org/downloads/) version 3.9 or above, [pipenv](https://pypi.org/project/pipenv/) and [Fabric](https://www.fabfile.org/installing.html) 2.7 or above:

  - Check your python version using `python -V`.
  - Check your pipenv version using `pipenv --version`.

- [Node.js](https://nodejs.org/en/download/) version 18 and [yarn](https://yarnpkg.com/) package manager version 1.x:
  - Check your NodeJS version using `node -v`.
  - Check your yarn version using `yarn -v`.

:::caution If you have node version above 18
When you build front for the first time you will have an error like that `Error: error:0308010C:digital envelope routines::unsupported`

On Unix-like (Linux, macOS, etc..) set this env:
`export NODE_OPTIONS=--openssl-legacy-provider`
:::

_For developers using MacOS we use the Symfony CLI for a better & faster Developer Experience. That's why we require MacOS users to install some PHP dependencies on the host, instead of using our docker container directly._

- [PHP](https://www.php.net/) version 7.4 and [composer](https://getcomposer.org/) version 2.1 or above and [Symfony CLI](https://symfony.com/download) version 5.x or above:
  - Check your PHP version using `php -v`.
  - The following PHP extensions must be installed and enabled `redis`, `imagick`, `amqp`.
  - Check your composer version using `composer --version`.
  - Check you Symfony CLI version using `symfony -V`.

:::caution If you try to install php 7.4 with brew the formula is no longer available, instead run:
```bash
brew tap shivammathur/php && brew install shivammathur/php/php@7.4 && brew link php@7.4
```
:::

### Getting started

#### 1. Clone the repository:

```bash
git clone git@github.com:cap-collectif/cap-collectif.git
cd cap-collectif
```

#### 2. Using our development CLI

[Cap Collectif](https://github.com/cap-collectif/cap-collectif) is a huge repository using a complex infrastructure with many technologies. In order to unify as much as possible our development experience between CI, Linux and MacOS, we implemented a CLI to simplify our interactions with Docker containers.

To run the CLI, you need to activate the Pipenv shell:

```bash
pipenv install # install dependencies from Pipfile
pipenv shell # activate this project's virtualenv
```

You can run several Fabric tasks such as:

```bash
fab local.system.doctor
```

This task will dump the version of every dependency that you need ton install on your host.

:::tip

In order to list every available tasks you can use `fab -l local`.

:::

#### 3. Setup your environment variables

We use a lot of environment variables !

You need to generate a default `.env.local` file that contains the required variables to launch the repository:

```
fab local.app.setup-default-env-vars
```

This file is not versioned so you can update it freely.

:::info

To enable some services (like geolocation with Google Map) you will need to insert some credentials instead of `INSERT_A_REAL_SECRET` values in your `.env.local` file.

See [environment variables documentation](/getting-started/environment-variables).

:::

#### 4. Setup DNS and SSL

The development environment is only accessible via HTTPS and the following hostnames: `capco.dev`, `capco.test` and `capco.prod`.

That's why you need to setup some virtual hosts:

```bash
fab local.system.configure-vhosts
```

This task will automatically update your `/etc/hosts` file.

```bash
fab local.system.generate-ssl
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
fab local.app.deploy
```

:::tip

For MacOS users, it's faster to run all those tasks on your host:

```bash
yarn install --purelockfile
COMPOSER_MEMORY_LIMIT=-1 composer install
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

First, install dependencies:

```bash
yarn install
```

Then, download the up to date translation files:

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
yarn build:prod
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

#### 10. Configure AC in your browser

In Firefox:
* Go to `about:preferences#privacy`
* Search for "certificates"
* Click "View Certificates..."
* In the Certificate Manager, click the Authorities tab
* Click the "Import" button to import your certificate
  * Select `~/.local/share/mkcert/rootCA.pem` file
  * You might be prompted to set the trust level upon importing the
  certificate. Select identify websites.
* Restart Firefox

#### 11. Open your first page

It's time to open your browser with: https://capco.dev

It should work !

In the next steps of this tutorial, we are going to explain how to work with every part of the stack.
