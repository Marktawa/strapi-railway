# How to self-host Strapi on Railway

In this tutorial, you will learn how to deploy a Strapi app on Railway. Strapi is a CMS you can use to easily build APIs for your apps. Railway is hosting platform that supports all kinds of apps.

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

Run your Strapi app.
```shell
npm run develop
```

Visit http://localhost:1337/admin in your browser to login.
![Strapi Admin Login](https://res.cloudinary.com/craigsims808/image/upload/v1737184283/strapi/strapi-railway/strapi-admin-login_mxcmfn.png)

Create a new collection type name it `Global`. Add a single `text` field named `text`.

Click on `Content-Type Builder` > `Create new collection type` in the navigation.

![Create Global Collection](https://res.cloudinary.com/craigsims808/image/upload/v1737184659/strapi/strapi-railway/create-global-collection_iuejqp.png)

Add a new entry to the `Global` collection. Give it the value, "Hello Strapi".

Go to  `Content Manager` > `Collection types` - `Global` in the navigation.
Click on Create new entry.

![Add entry to global collection](https://res.cloudinary.com/craigsims808/image/upload/v1737249089/strapi/strapi-railway/global-collection-entry_g9aq2a.png)

Enable Public Read access to your `Global` collection API. Click on `Settings` > `Users & Permissions Plugin` > `Roles` > `Public` > `Permissions`.

![Enable Public Access to API](https://res.cloudinary.com/craigsims808/image/upload/v1737249835/strapi/strapi-railway/enable-public-access-to-api_uq2tek.png)

In your terminal test out the API by running the following command:
```shell
curl localhost:1337/api/globals
```

You should see the following JSON response:
```json
{
    "data": [
        {
            "id": 2,
            "documentId": "cnuj5blzet9zbtyv3311rpjp",
            "text": "Hello Strapi",
            "createdAt": "2025-01-18T07:22:01.233Z",
            "updatedAt": "2025-01-18T07:22:01.233Z",
            "publishedAt": "2025-01-18T07:22:01.241Z"
        }
    ],
    "meta": {
        "pagination": {
            "page": 1,
            "pageSize": 25,
            "pageCount": 1,
            "total": 1
        }
    }
}
```


