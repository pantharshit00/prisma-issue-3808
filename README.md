## Introduction

This is reproduction for https://github.com/prisma/prisma/issues/3808

## Steps for Reproduction

1. Deploy to model to local container
2. Run these to get some data:

```graphql
mutation {
  createOrganisation(
    data: { name: "Prisma", user: { create: { name: "Harshit" } } }
  ) {
    id
  }
}
```

3. Now query this data and grab the ids for next step:

```graphql
{
  users {
    id
    organisations {
      name
      id
    }
  }
}
```

4. Execute this (replace ids)

```graphql
mutation {
  executeRaw(
    query: "SELECT User.id, Organisation.id AS organisationId, Organisation.name AS organisationName, User.name FROM ( SELECT 'cjqwgkjh0000e0935xgg3zn0z' AS id, 'Harshit' AS name ) AS User INNER JOIN ( SELECT 'cjqwgkjf4000d0935g042buvl' AS id, 'Prisma' AS name ) as Organisation"
  )
}
```
