<p align="center">
  <a href="https://www.medusa-commerce.com">
    <img alt="Medusa" src="https://i.imgur.com/USubGVY.png" width="100" />
  </a>
</p>
<h1 align="center">
  Setup a complete Shopefree (Medusa Extension) environment
</h1>
<p align="center">
This repo provides the skeleton to get you started with using <a href="https://github.com/medusajs/medusa">Medusa</a> extension for a shopify marketplace. Follow the steps below to get ready and have a complete environment running.
</p>
<p align="center">
  <a href="https://github.com/medusajs/medusa/blob/master/LICENSE">
    <img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="Medusa is released under the MIT license." />
  </a>
  <a href="https://github.com/medusajs/medusa/blob/master/CONTRIBUTING.md">
    <img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat" alt="PRs welcome!" />
  </a>
  <a href="https://discord.gg/xpCwq3Kfn8">
    <img src="https://img.shields.io/badge/chat-on%20discord-7289DA.svg" alt="Discord Chat" />
  </a>
  <a href="https://twitter.com/intent/follow?screen_name=medusajs">
    <img src="https://img.shields.io/twitter/follow/medusajs.svg?label=Follow%20@medusajs" alt="Follow @medusajs" />
  </a>
  <p align="center">
    <a href="https://heroku.com/deploy?template=https://github.com/medusajs/medusa-starter-default/tree/feat/deploy-heroku">
      <img src="https://www.herokucdn.com/deploy/button.svg" alt="Deploy">
    </a>
  </p>
</p>

## Prerequisites

This starter has minimal prerequisites and most of these will usually already be installed on your computer.

- [Install Node.js](https://nodejs.org/en/download/)
- [Install git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [Install docker](http://docker.io)

## Setting up your local development of medusa
This repo provides a docker compose file to initialize a local database a redis cache and a pgadmin containers to ease development.

- Install the Medusa CLI
  ```
  npm install -g @medusajs/medusa
  yarn global add @medusajs/medusa
  ```
 - Setup environment variables
  ```
  cp .env.template .env
  ```
 - Set the following variables into .env file
  ```
  DATABASE_URL="postgres://postgres:postgres@localhost/medusa-store" 
  REDIS_URL="redis://localhost:6379"
  ```
 - Run docker compose to setup database dependencies
  ```
  docker compose up -d postgres redis pgadmin
  ```
 - Run medusa migrations. Make sure medusa-cli is installed (see medusa cli documentation for instructions). Make sure DATABASE_URL and REDIS_URL is set in you shell.
  ```
  export DATABASE_URL=...
  export REDIS_URL=...
  medusa migrations run
  medusa seed -f ./data/seed.json
  ```
- Run your project. Environment is set. You can now develop and test code running 
  ```
  cd shop-efree-backend
  medusa develop
  curl -X GET localhost:9000/store/products
  ```

Your local Medusa server is now running on port **9000**.

### Seeding your Medusa store

---

To seed your medusa store run the following command:

```
medusa seed -f ./data/seed.json
```

This command seeds your database with some sample data to get you started, including a store, an administrator account, a region and a product with variants. What the data looks like precisely you can see in the `./data/seed.json` file.

## Seting up datasources
- Update project config in `medusa-config.js`:
  Ideally no changes needed to this file for setting a development environment
  ```
  module.exports = {
    projectConfig: {
      redis_url: REDIS_URL,
      database_url: DATABASE_URL, //postgres connectionstring
      database_type: "postgres",
      store_cors: STORE_CORS,
      admin_cors: ADMIN_CORS,
    },
    plugins,
  };
  ```
## Setting up your local environemnt with an admin and storefront samples
- Ensure to checkout submodules
  ```
  git submodule update --init --recursive
  ```

- Ensure to have a medusa server running (In case you want medusa running on a container rather than from you local shell. If you followed previous setps and have a medusa server running already ignore this step)

  When running your project the first time `docker compose` should be run with the `build` flag to build your container locally:

  ```
  docker-compose up --build -d
  ```

  When running your project subsequent times you can run docker compose with no flags to spin up your local environment in seconds:

  ```
  docker-compose up -d
  ```

Your local Medusa server is now running on port **9000**.
To setup a local development environment without docker see "Setting up your store" section

- Seed your environment
  ```
  docker exec shopefree-backend medusa seed -f ./data/seed.json
  ```
 
- Admin is now ready on localhost:7000 and storefront on localhost:8000
 
### Seeding your Medusa store with Docker

---

To add seed data to your medusa store running with Docker, run this command in a seperate terminal:

```
docker exec medusa-server-container-name medusa seed -f ./data/seed.json
```

This will execute the previously described seed script in the running `medusa-server` Docker container.

## Try it out

```
curl -X GET localhost:9000/store/products | python -m json.tool
```

After the seed script has run you will have the following things in you database:

- a User with the email: admin@medusa-test.com and password: supersecret
- a Region called Default Region with the countries GB, DE, DK, SE, FR, ES, IT
- a Shipping Option called Standard Shipping which costs 10 EUR
- a Product called Cool Test Product with 4 Product Variants that all cost 19.50 EUR

Visit [docs.medusa-commerce.com](https://docs.medusa-commerce.com) for further guides.

<p>
  <a href="https://www.medusa-commerce.com">
    Website
  </a> 
  |
  <a href="https://medusajs.notion.site/medusajs/Medusa-Home-3485f8605d834a07949b17d1a9f7eafd">
    Notion Home
  </a>
  |
  <a href="https://twitter.com/intent/follow?screen_name=medusajs">
    Twitter
  </a>
  |
  <a href="https://docs.medusa-commerce.com">
    Docs
  </a>
</p>
