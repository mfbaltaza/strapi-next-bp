# Starter Next 14, Tailwind, Typescript and Strapi

## Hello Strapi

Strapi Community Edition is a free and open-source headless CMS enabling you to manage any content, anywhere.

- **Self-hosted or Cloud**: You can host and scale Strapi projects the way you want. You can save time by deploying to [Strapi Cloud](https://cloud.strapi.io/signups?source=github1) or deploy to the hosting platform you want**: AWS, Azure, Google Cloud, DigitalOcean.
- **Modern Admin Pane**: Elegant, entirely customizable and a fully extensible admin panel.
- **Multi-database support**: You can choose the database you prefer: PostgreSQL, MySQL, MariaDB, and SQLite.
- **Customizable**: You can quickly build your logic by fully customizing APIs, routes, or plugins to fit your needs perfectly.
- **Blazing Fast and Robust**: Built on top of Node.js and TypeScript, Strapi delivers reliable and solid performance.
- **Front-end Agnostic**: Use any front-end framework (React, Next.js, Vue, Angular, etc.), mobile apps or even IoT.
- **Secure by default**: Reusable policies, CORS, CSP, P3P, Xframe, XSS, and more.
- **Powerful CLI**: Scaffold projects and APIs on the fly.

## Features

- **Content Types Builder**: Build the most flexible publishing experience for your content managers, by giving them the freedom to create any page on the go with [fields](https://docs.strapi.io/user-docs/content-manager/writing-content#filling-up-fields), components and [Dynamic Zones](https://docs.strapi.io/user-docs/content-manager/writing-content#dynamic-zones).
- **Media Library**: Upload your images, videos, audio or documents to the media library. Easily find the right asset, edit and reuse it.
- **Internationalization**: The Internationalization (i18n) plugin allows Strapi users to create, manage and distribute localized content in different languages, called "locales
- **Role Based Access Control**: Create an unlimited number of custom roles and permissions for admin and end users.
- **GraphQL or REST**: Consume the API using REST or GraphQL


## Getting Started

1. Clone the repository locally:

2. Run `setup` command to setup frontend and backend dependencies:

```bash
  yarn setup
```

3. Next, navigate to your `/backend` directory and set up your `.env` file. You can use the `.env.example` file as reference:

```bash
HOST=localhost
PORT=1337
APP_KEYS="toBeModified1,toBeModified2"
API_TOKEN_SALT=tobemodified
ADMIN_JWT_SECRET=tobemodified
JWT_SECRET=tobemodified
TRANSFER_TOKEN_SALT=tobemodified
```

4. Start your project by running the following command:

```bash
  yarn build
  yarn develop
```

You will be prompted to create your first admin user.

![admin-user](https://user-images.githubusercontent.com/6153188/231865420-5f03a90f-b893-4057-9634-9632920a7d97.gif)

Great. You now have your project running. Let's add some data.

## Seeding The Data

We are going to use our DEITS feature which will alow to easily import data into your project.

You can learn more about it in our documentation [here](https://docs.strapi.io/dev-docs/data-management).

In the root of our project we have our `seed-data.tar.gz` file. We will use it to seed our data.

1. Open up your terminal and make sure you are still in you `backend` folder.

2. Run the following command to seed your data:

```bash
  yarn strapi import -f ../seed-data.tar.gz
```

![after-import](https://user-images.githubusercontent.com/6153188/231865491-05cb5818-a0d0-49ce-807e-a879f7e3070c.gif)

This will import your data locally. Log back into your admin panel to see the newly imported data.

Here is a quick video covering initial setup and data seeding.

https://github.com/strapi/nextjs-corporate-starter/assets/6153188/80f00bf5-d17b-449d-8a0d-7f0d9614f40b

## Setting Up The Frontend

Next we need to switch to our `/frontend` directory and create our `.env` file and paste in the following.

```bash
NEXT_PUBLIC_STRAPI_API_TOKEN=your-api-token
NEXT_PUBLIC_PAGE_LIMIT=6
NEXT_PUBLIC_STRAPI_FORM_SUBMISSION_TOKEN=your-form-submission-token
NEXT_PUBLIC_STRAPI_API_URL=http://localhost:1337

```

Before starting our Next JS app we need to go inside our Strapi Admin and create two tokens that we will be using for **form submission** and displaying our **content**.

Inside your Strapi Admin Panel navigate to Settings -> API Tokens and click on the `Create new API Token`.

![api-tokens](https://user-images.githubusercontent.com/6153188/231865572-cebc5538-374c-4050-91cd-c303fae25a3d.png)

Here are our Token Settings

Name: Public API Token Content
Description: Access to public content.
Token duration: Unlimited
Token type: Custom

In Permissions lets give the following access.

| Content         |   Permissions    |
| --------------- | :--------------: |
| Article         | find and findOne |
| Author          | find and findOne |
| Category        | find and findOne |
| Global          |       find       |
| Page            | find and findOne |
| Product-feature | find and findOne |

![permissions](https://user-images.githubusercontent.com/6153188/231865625-a3634d89-0f40-4a6d-a356-8f654abd88b9.gif)

Once you have your token add it to your `NEXT_PUBLIC_STRAPI_API_TOKEN` variable name in the `.env` file.

**Alternatively:** you can create a READ only Token that will give READ permission to all your endpoints.

In this particular project this is not an issue. Although the above is the recommended way, just wanted to show you this option here as well.

When creating a Token, just select the `Read-only` option from token type drop down.

<img width="1093" alt="create-read-only-token" src="https://github.com/strapi/nextjs-corporate-starter/assets/6153188/3ea6c029-b296-4bbc-a5ce-33eedac52a03">

Next create a token that will allow us to submit our form.

Name: Public API Form Submit
Description: Form Submission.
Token duration: Unlimited
Token type: Custom

In Permissions lets give the following access.

| Content              | Permissions |
| -------------------- | :---------: |
| Lead-Form-Submission |   create    |

Add your token to your `NEXT_PUBLIC_STRAPI_FORM_SUBMISSION_TOKEN` variable name in the `.env` file.

Once your environment variables are set you can start your frontend application by running `yarn dev`.

You should now see your Next JS frontend.

![frontend](https://user-images.githubusercontent.com/6153188/231865662-d870051f-4503-4a01-bc6b-635c7c5ca40d.png)

## Start Both Projects Concurrently

We can also start both projects with one command using the `concurrently` package.

You can find the setting inside the `package.json` file inside the root folder.

```json
{
  "scripts": {
    "frontend": "yarn dev --prefix ../frontend/",
    "backend": "yarn dev --prefix ../backend/",
    "clear": "cd frontend && rm -rf .next && rm -rf cache",
    "setup:frontend": "cd frontend && yarn",
    "setup:backend": "cd backend && yarn",
    "setup": "yarn install && yarn setup:frontend && yarn setup:backend",
    "dev": "yarn clear && concurrently \"cd frontend && yarn dev\" \"cd backend && yarn develop\""
  },
  "dependencies": {
    "concurrently": "^7.6.0"
  }
}
```
Go to the root folder and install the package, `yarn`
You can start both apps by running `yarn dev`.
