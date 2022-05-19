---
sidebar_position: 6
---

# Developing our NextJS backend

# Admin-next

This is our [Next.js](https://nextjs.org/) implementation for our new administration.

In production all requests to `/admin-next/*` are executed by this application.

This is our most recent code, so we use TypeScript instead of Flow.

## Getting Started

### Requirements

First, make sure your `/etc/hosts` contains:

```
127.0.0.1 admin-next.capco.dev
```

Then check all docker containers are running (`fab local.infrastructures.up`), because `admin-next` is using Redis to access the user's session.

### Daily commands

:::caution

All commands requires to be in the `admin-next` folder.

:::

First, start the development SSL proxy, it adds HTTPS support. Run this command in a tab (you will need to keep it running):

```bash
yarn start-ssl-proxy
```

Admin-next also uses it's own relay compiler, so don't forget to run it:

```bash
yarn relay
```

Then in an other tab, start the NextJS development server:

```bash
yarn dev
```

Open [https://admin-next.capco.dev:3001/](https://admin-next.capco.dev:3001/) with your browser to see the result.

The `pages/*` directory is mapped to `/*` with auto reloading. 

## How to install deps

We use an `admin-next` workspace, so use something like:

```bash
yarn workspace admin-next add next@latest
```

## Learn More

To learn more about Next.js, take a look at the following resources:

- [Next.js Documentation](https://nextjs.org/docs) - learn about Next.js features and API.
- [Learn Next.js](https://nextjs.org/learn) - an interactive Next.js tutorial.