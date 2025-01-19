# How to self-host Strapi on Railway

![Cover](/docs/cover.png)

Self-hosting Strapi on Railway is an efficient and cost-effective way to deploy and manage your Strapi-powered projects. Strapi, an open-source headless CMS, provides developers with a flexible platform to build and customize APIs, while Railway simplifies the deployment process with its robust and developer-friendly infrastructure.

In this guide, you'll learn how to set up Strapi locally, prepare your project for deployment, and host it on Railway using two methods: the Railway CLI and GitHub integration. By following this tutorial, you'll have a fully operational Strapi app running online, ready to serve content and expose APIs to your applications.

Whether you're new to Strapi or an experienced developer looking to optimize your deployment workflow, this guide provides clear and actionable steps to achieve your goals. Let’s dive in!

## Pre-requisites

- Node.js installed on your dev machine. (v18 or v20)[Download and install Node.js using this link](https://nodejs.org/en/download)
- GitHub Account. [Create GitHub account here](https://github.com/join)
- GitHub CLI[Download and Install GitHub CLI here](https://cli.github.com)
- Railway Account. [Sign up for Railway here](https://railway.com/login)
- Railway CLI. [Download and Install Railway CLI here](https://github.com/railwayapp/cli)

## Hello, Strapi

Open up your working directory and create a new Strapi project named `hello`.
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

## Deploy to Railway using CLI

Railway offers many ways to deploy your web app. You can use the dashboard, the CLI, the API etc. This article: [Railway Deployment Options](https://docs.railway.com/quick-start) explains all the numerous ways you can deploy an app on the Railway platform.

In this step, we will use the Railway CLI. If you prefer using the dashboard skip to the next section.

Stop your Strapi server using `CTRL` + `C`. 

In your terminal, install the Railway CLI tool:
```shell
curl -fsSL https://railway.com/install.sh | sh
```

Login to your Railway account:
```shell
railway login --browserless
```

Create a new Railway project:
```shell
railway init
```

Link your Strapi project folder to your Railway project:
```shell
railway link
```

Deploy your Strapi app.
```shell
railway up --detach
```

The deployment will fail, as you will need to update your Railway project with the necessary environment variables. 
The following variables need to be added to your Strapi project in Railway: `HOST`, `PORT`, `APP_KEYS`, `API_TOKEN_SALT`, `ADMIN_JWT_SECRET`, `TRANSFER_TOKEN_SALT`, `DATABASE_CLIENT`, `DATABASE_FILENAME`, `DATABASE_SSL` and `JWT_SECRET`.

Copy the values from your `.env` file:
```
HOST=0.0.0.0
PORT=1337
APP_KEYS=/M/DMAN536wRHe+Vf9K2Cg==,uogMfTxg3jJvNfctla2xAw==,ne6731wcSzH2tQ6dgDiEYQ==,srycFvmx8KTrsq6N60C8DA==
API_TOKEN_SALT=XJtQnpUf8OEXL7b7LBWunQ==
ADMIN_JWT_SECRET=+DSzXmSsQOsS0V6O0npL2g==
TRANSFER_TOKEN_SALT=fceG+mK0nhBh7c8AbX1HAw==
DATABASE_CLIENT=sqlite
DATABASE_SSL=false
DATABASE_FILENAME=.tmp/data.db
JWT_SECRET=Ppx1F1bpyYnjumiGIJSk3g==
```

Select `Variables` in your Railway dashboard to update your project with the required environment variables.

![Add environment variables to Railway](https://res.cloudinary.com/craigsims808/image/upload/v1737253718/strapi/strapi-railway/add-railway-variables_ofw07w.png)

After updating the environment variables, redeploy your Strapi app.
```shell
railway up
```

When the Strapi app deployment is ready, generate a domain.
```shell
railway domain
```

### Test Strapi API hosted on Railway

Test out your Railway-hosted Strapi API in your terminal. Run the following command:

```shell
curl <your-railway-project-url>/api/globals
```

Replace `<your-railway-project-url>` with the URL generated by Railway. You should get the same response as you did when you tested the project locally.

In your browser visit `<your-railway-project-url>/admin` to login to your project.

![Logged in to Strapi Admin on Railway](https://res.cloudinary.com/craigsims808/image/upload/v1737255130/strapi/strapi-railway/logged-in-strapi-on-railway_kx2xiy.png)

## Deploy to Railway using GitHub

We will use the GitHub and the Railway dashboard for this option.

### Turn Strapi project into GitHub repo

Before you deploy the project to Railway, you must turn it into a GitHub repo.

Stop your Strapi server using `CTRL` + `C`. 

Initialize your project directory as a Git repo.
```shell
git init
```

Comment out or delete the text `.tmp` in your `.gitignore` file so that source control includes the SQLite database for your Strapi app.

Commit your project files.
```shell
git add .
```

```shell
git commit -m "Initial commit"
```

Authenticate with GitHub
```shell
gh auth login
```

Upload your repo to GitHub
```shell
gh repo create hello --public --source=. --remote=origin
```

```shell
git push --set-upstream origin master
```

Replace `hello` with your desired repo name.


### Create a new Railway project

Visit https://dev.new in your browser. This will redirect you to https://railway.com/new and you will see a **New Project** modal with deployment options.

![Railway New Project Modal](https://res.cloudinary.com/craigsims808/image/upload/v1735561144/freelance/falcon-feather/railway-create-new-project-dashboard_x3f2le.png)

Select **Deploy from GitHub repo** and choose the repo you created previously.

![Deploy From GitHub repo](https://res.cloudinary.com/craigsims808/image/upload/v1737251920/strapi/strapi-railway/deploy-from-github-repo_v33pmh.png)

The Railway platform will read the contents of your repo, initialize the project, build and then deploy it automatically as a service. The deployment will fail as a few settings need to be done in your Railway project.

![Railway Build unsuccessful](https://res.cloudinary.com/craigsims808/image/upload/v1737253268/strapi/strapi-railway/railway-fail_pok9lh.png)

### Add Variables

The following variables need to be added to your Strapi project in Railway: `HOST`, `PORT`, `APP_KEYS`, `API_TOKEN_SALT`, `ADMIN_JWT_SECRET`, `TRANSFER_TOKEN_SALT`, `DATABASE_CLIENT`, `DATABASE_FILENAME`, `DATABASE_SSL` and `JWT_SECRET`.

Use the following values:
```
HOST=0.0.0.0
PORT=1337
APP_KEYS=/M/DMAN536wRHe+Vf9K2Cg==,uogMfTxg3jJvNfctla2xAw==,ne6731wcSzH2tQ6dgDiEYQ==,srycFvmx8KTrsq6N60C8DA==
API_TOKEN_SALT=XJtQnpUf8OEXL7b7LBWunQ==
ADMIN_JWT_SECRET=+DSzXmSsQOsS0V6O0npL2g==
TRANSFER_TOKEN_SALT=fceG+mK0nhBh7c8AbX1HAw==
DATABASE_CLIENT=sqlite
DATABASE_SSL=false
DATABASE_FILENAME=.tmp/data.db
JWT_SECRET=Ppx1F1bpyYnjumiGIJSk3g==
```

Select `Variables` in your Railway dashboard to update your project with the required environment variables.

![Add environment variables to Railway](https://res.cloudinary.com/craigsims808/image/upload/v1737253718/strapi/strapi-railway/add-railway-variables_ofw07w.png)

After updating the variables you can deploy your Strapi project. The build will be successful.

![Railway Deploy Success](https://res.cloudinary.com/craigsims808/image/upload/v1737254044/strapi/strapi-railway/railway-deploy-success_glzvvf.png)

### Generate a Domain for your project

Select **Settings** inside your Railway project's service. Under **Networking** click **Generate Domain**. This allows you to access your service on the internet.

![Generate Domain](https://res.cloudinary.com/craigsims808/image/upload/v1735586216/freelance/falcon-feather/generate-domain_mdpf51.png)

Railway will generate a domain name for your app. The URL will appear a few seconds after clicking **Generate Domain**. You will use this URL to access your Strapi API on the internet. 

![View Generated Domain](https://res.cloudinary.com/craigsims808/image/upload/v1737254224/strapi/strapi-railway/railway-generate-domain_ucmj81.png)

### Test Strapi API hosted on Railway

Test out your Railway-hosted Strapi API in your terminal. Run the following command:

```shell
curl <your-railway-project-url>/api/globals
```

Replace `<your-railway-project-url>` with the URL generated by Railway. You should get the same response as you did when you tested the project locally.

In your browser visit `<your-railway-project-url>/admin` to login to your project.

![Logged in to Strapi Admin on Railway](https://res.cloudinary.com/craigsims808/image/upload/v1737255130/strapi/strapi-railway/logged-in-strapi-on-railway_kx2xiy.png)

## Conclusion  

Self-hosting Strapi on Railway is a straightforward and efficient way to deploy your project and make it accessible online. By leveraging Railway's tools and services, you can simplify the deployment process, manage your environment variables, and scale your application as needed. Whether you prefer using the CLI or GitHub for deployment, this guide equips you with the knowledge to set up a robust and flexible Strapi instance.  

Now that your Strapi app is live, you can focus on building and delivering exceptional content and APIs to power your applications. Happy coding! For more advanced configurations or scaling needs, refer to the [Railway Documentation](https://docs.railway.com) or the [Strapi Documentation](https://docs.strapi.io/). Happy coding!

## Resources  

Here are the resources mentioned in this guide to help you get started and explore further:  

0. [GitHub Repo with Source Code](https://github.com/Marktawa/strapi-railway)
1. [Node.js Official Website](https://nodejs.org/en/download)  
2. [GitHub Official Website](https://github.com/join)  
3. [GitHub CLI Documentation](https://cli.github.com)  
4. [Railway Official Website](https://railway.com/login)  
5. [Railway CLI GitHub Repository](https://github.com/railwayapp/cli)  
6. [Strapi Official Website](https://strapi.io)  
7. [Railway Deployment Options](https://docs.railway.app/quick-start)  

These references provide the tools and documentation needed to set up, deploy, and manage your Strapi application effectively.
