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