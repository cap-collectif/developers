---
sidebar_position: 5
---

# Developing our GraphQL API

Our GraphQL schemas are located at `src/Capco/AppBundle/Resources/config/graphql`.

We have :

- `dev`
- `internal`
- `public`
- `preview`

All GraphQL files are `*.types.yaml`.

Every time you change the schema in yaml, don't forget to run:

```bash
yarn relay
```

It will generate all PHP and JS code.

## Learn More

To learn more about GraphQL, take a look at the following resources:

- [GraphQL Documentation](https://graphql.org/learn/) - learn about GraphQL.
- [GraphQL Hero](https://graphqlhero.com/) - a French course about GraphQL.
