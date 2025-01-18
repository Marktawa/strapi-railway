# How to self-host Strapi on Railway

In this tutorial, you will learn how to deploy a Strapi app to Railway. Strapi is a CMS you can use to easily build APIs for your apps. Railway is hosting platform that supports all kinds of apps.

## Pre-requisites

- Node.js installed on your dev machine. (v18 or v20)[Download and install Node.js using this link](https://nodejs.org/en/download)
- GitHub Account. [Create GitHub account here](https://github.com/join)
- GitHub CLI[Download and Install GitHub CLI here](https://cli.github.com)
- Railway Account. [Sign up for Railway here](https://railway.com/login)
- Railway CLI. [Download and Install Railway CLI here](https://github.com/railwayapp/cli)

## Hello, Strapi

Open up your working directory and create a new Strapi project.
```shell
npx create-strapi-app@latest hello --quickstart --no-run
```

Generate an admin user for your project.
```shell
cd hello
```
```shell
npm run strapi admin:create-user -- --firstname=Kai --lastname=Doe --email=chef@strapi.io --password=Gourmet1234
```

Generate a Single Type named `hello`.
```shell
npm run strapi generate
```

```
content-type
Content type display name hello
? Content type singular name hello
? Content type plural name hellos
? Please choose the model type Collection Type
? Do you want to add attributes? Yes
? Name of attribute global
? What type of attribute (Use arrow keys)
? What type of attribute text
? Do you want to add another attribute? No
? Where do you want to add this model? Add model to new API
? Name of the new API? hello
? Bootstrap API related files? Yes
```

Add a Text field called global.

Enable public access to the API.
```ts
//./src/api/hello/routes/hello.ts
import { factories } from '@strapi/strapi';

export default  factories.createCoreRouter('api::hello.hello', {
  config: {
    find: {
      auth: false
    },
    create: {
      auth: false
    }
  }
});
```

Publish it with the value "hie". Run your Strapi server.
```shell
npm run develop
```

In a different terminal session run this:
```shell
curl -X POST http://localhost:1337/api/hello \
  -H 'Content-Type: application/json' \
  -d '{"data": {"message": "hie"}}'
```

