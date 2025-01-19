
# Deploy Strapi on Railway

![cover](/docs/cover.png)

## Description

This repo contains the source code used in the tutorial [How to self-host Strapi on Railway](https://dev.to/markmunyaka)

## Pre-requisites

- Node.js installed on your dev machine. (v18 or v20) [Download and install Node.js using this link](https://nodejs.org/en/download)
- GitHub Account. [Create GitHub account here](https://github.com/join)
- GitHub CLI[Download and Install GitHub CLI here](https://cli.github.com)
- Railway Account. [Sign up for Railway here](https://railway.com/login)
- Railway CLI. [Download and Install Railway CLI here](https://github.com/railwayapp/cli)

## How to use repo

Open up your terminal and clone the repository.
```shell
git clone https://github.com/Marktawa/strapi-railway
```

Install dependencies.
```shell
cd strapi railway/hello
```

```shell
npm install
```

Add environment variables
```shell
cp .env.example .env
```

Import data
```shell
npm run strapi import -- -f ../hello.tar.gz.enc
```

Run Strapi server
```shell
npm run develop
```

Visit `http://localhost:1337` in your browser.

## Author

Mark Munyaka  
https://dev.to/markmunyaka