---
sidebar_position: 1
---

# Setting up our development environment

### What you'll need

- [Docker](https://www.docker.com/get-started/) version 20 or above and [Docker compose](https://docs.docker.com/compose/):
  - Check your docker version using `docker -v` ;
  - Check your docker-component version using `docker-compose -v` ;
  - If you are using MacOS we recommand using [Docker Desktop on Mac](https://docs.docker.com/desktop/mac/install/).

- [Python](https://www.python.org/downloads/) version 3.9 or above and [Fabric](https://www.fabfile.org/installing.html) 2.6 or above:
  - Check your python version using `python -V`.
  - Check your Fabric version using `fab -V`.

- [Node.js](https://nodejs.org/en/download/) version 17 or above and [yarn](https://yarnpkg.com/) package manager:
  - Check your NodeJS version using `node -v`.
  - Check your yarn version using `yarn -v`.

For developers using MacOS we also prefer to install PHP dependencies on the host, instead of using docker containers.

- [PHP](https://www.php.net/) version 7.4 and [composer](https://getcomposer.org/) version 2.1 or above:
  - Check your PHP version using `php -v`.
  - Check your composer version using `composer --version`.

### Getting started

#### 1. Clone the repository :

```bash
git clone git@github.com:cap-collectif/cap-collectif.git
```

Our repo is quite huge, so it will take some times…

#### 2. Using our development CLI

Cap Collectif is a huge repository using a complex infrastructure with many technologies. In order to unify our development experience between Linux, MacOS and our CI we implemented our custom CLI to simplify interaction with Docker containers.

Inside `cap-collectif` directory, you can run several Fabric tasks, to list every available tasks use `fab -l`.

#### 3. Setup your environment variables

We are going to run a command in order to setup a `.env.local` file :

```
fab local.system.setup-env-var
cat .env.local # make any change to setup services
```

#### 4. Setup DNS and SSL

Setup certificates and hostnames :

```
fab local.system.setup-env-var
cat .env.local # make any change to setup services
```

#### 5. Install development dependencies

```bash
fab local.system.deploy
```

or, if you prefer to run individual tasks :

```bash
composer install
yarn install --purelockfile
```

#### 6. Build and launch our docker infrastructure

```bash
fab local.infrastructures.build
fab local.infrastructures.up
```

#### 7. Add some development data

Let's add our development database with some fixtures :

```bash
fab local.database.generate
```

#### 8. Setup the frontend

```bash
yarn trad
yarn build-relay-schema
yarn build:prod
```

#### 9. Open your first page

open https://capco.dev

It should work ! In the next step we are going to explain how to work with every part of the stack.