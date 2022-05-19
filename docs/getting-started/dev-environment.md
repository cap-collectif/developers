---
sidebar_position: 1
---

# Setting up our development environment

We provide and environment for both Linux and MacOS.
### What you'll need

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

#### 1. Clone the repository :

```bash
git clone git@github.com:cap-collectif/cap-collectif.git
```

Our repo is quite huge, so it will take some times…

#### 2. Using our development CLI

Cap Collectif is a huge repository using a complex infrastructure with many technologies. In order to unify our development experience between Linux, MacOS and our CI we implemented our custom CLI to simplify interaction with Docker containers.

Inside `cap-collectif` directory, you can run several Fabric tasks, to list every available tasks use `fab -l`.

Make sur to run `pipenv shell` first.

#### 3. Setup your environment variables

We are going to run a command in order to generate a default `.env.local` file :

```
fab local.system.setup-env-var
cat .env.local
```

To enable some services like geolocation with Google Map you will need to insert some credentials instead of `INSERT_A_REAL_SECRET` values  in your `.env.local` file.

#### 4. Setup DNS and SSL

Setup your development certificates and hostname (`capco.dev`, `capco.test` and `capco.prod`) :

```bash
fab local.system.configure-vhosts
```

This will update your `/etc/hosts` file.

```bash
fab local.system.sign-ssl
```

This will generate SSL certificates for your host machine.

#### 5. Build and launch our docker infrastructure

First build the docker cache :
```bash
fab local.infrastructures.build
```

Now you can launch the docker stack :

```bash
fab local.infrastructures.up
```

#### 6. Install code dependencies

_On Linux run :_

```bash
fab local.system.deploy
```

_On MacOS, we recommend to run individual tasks on your host :_

```bash
yarn install --purelockfile
composer install
php bin/console assets:install public --symlink
fab local.qa.compile-graphql
fab local.app.rabbitmq-queues
```

#### 7. Add some development data

Let's add our development database with some fixtures :

```bash
fab local.database.generate
```

#### 8. Setup the frontend

```bash
yarn trad # download up to date translation files
yarn build-relay-schema # generate up to date relay files which are not committed
yarn build:prod # generate up to date production js bundle
```

### MACOS Only

Configure proxy

#### 9. Open your first page

open https://capco.dev

It should work ! In the next step we are going to explain how to work with every part of the stack.