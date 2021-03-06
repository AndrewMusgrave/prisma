---
alias: ahs1jahkee
description: Learn how to deploy your GraphQL server with Zeit Now.
---

# Deploy a GraphQL server with Zeit Now

Once you're done implementing your GraphQL server and have tested it enough locally to make it available to the general public, you need to _deploy_ it to the web.

In this tutorial, you're going to learn how you can use [Now](https://zeit.co/now) - an amazing one-click deployment tool from the [Zeit](https://zeit.co/) team - to deploy your GraphQL server.

The tutorial has two parts:

1. **Basic**: Learn how to do a simple and straightforward deployment with `now` based on the [`node-basic`](https://github.com/graphql-boilerplates/node-graphql-server/tree/master/basic) GraphQL boilerplate project
2. **Advanced**: Learn how to deploy with `now` with environment variables based on the [`node-advanced`](https://github.com/graphql-boilerplates/node-graphql-server/tree/master/advanced) GraphQL boilerplate project

## Install Now Desktop

The first thing you need to do is download the [Now Desktop](https://github.com/zeit/now-desktop) app and login.

<Instruction>

Open [`https://zeit.co/download`](https://zeit.co/download) in your browser and hit the **DOWNLOAD**-button.

</Instruction>

> **Note**: Now Desktop includes the `now` CLI.

![](https://imgur.com/UpRzQsY.png)

## Part 1: Basic Deployment with Now

### Bootstrap the GraphQL server

In this tutorial, you'll use the [`node-basic`](https://github.com/graphql-boilerplates/node-graphql-server/tree/master/basic) GraphQL boilerplate project as a sample server to be deployed. The easiest way to get access to this boilerplate is by using the `graphql create` command from the [GraphQL CLI](https://github.com/graphql-cli/graphql-cli/).

The boilerplate project is based on [`graphql-yoga`](https://github.com/graphcool/graphql-yoga/), a lightweight GraphQL server based on Express.js, `apollo-server` and `graphql-tools`.

<Instruction>

If you haven't already, go ahead and install the GraphQL CLI. Then, bootstrap your GraphQL server with `graphql create`:

```bash
npm install -g graphql-cli
graphql create hello-basic --boilerplate node-basic
```

</Instruction>

<Instruction>

When prompted where (i.e. to which [Prisma server](!alias-eu2ood0she)) to deploy your Prisma service, choose the **Demo server** option. Note that this requires you to authenticate with [Prisma Cloud](https://www.prisma.io/cloud/) as that's where the Demo server is hosted.

</Instruction>

The above `graphql create` command creates a new directory called `hello-basic` where it places the source files for your GraphQL server as well as the configuration for the belonging Prisma service.

You now have a proper foundation to deploy your GraphQL server.

### Deploy the server with `now`

The `now` command uploads your source files and invokes the `start` script defined in your `package.json` to start the remote server.

<Instruction>

All you need to is navigate into the `hello-basic` directory and invoke `now`:

```bash
cd hello-basic
now
```

</Instruction>

> **Note**: If this is the first time you're using `now`, it will ask you to authenticate with their service.

That's it, your GraphQL server is now deployed and available under the URL printed by the CLI 🎉  The URL looks similar to `https://hello-basic-__ID__.now.sh` (where `__ID__` is a random ID for your service generated by `now`).

##  Part 2: Advanced: Using environment variables

### Bootstrap the GraphQL server

<Instruction>

In your terminal, navigate into a new directory and download the `node-advanced` boilerplate as follows:

```sh
graphql create hello-advanced --boilerplate node-advanced
```

</Instruction>

<Instruction>

Like before, when prompted where to deploy the service, choose the **Demo server**.

</Instruction>

The service is now deployed to a demo server.

### Deploy the server with `now`

This time, your service requires certain environment variables to be set. If you just ran `now` like in the previous section, the deployment would not succeed - or rather the Playground that would be deployed wouldn't work correctly because it doesn't know against which Prisma service it should be running. Because this information is now provided in terms of environment variables.

That's what you can use the `--dotenv` option of the `now` command for! It takes as an argument a `.env` file where environment variables are specified.

> `.env` files are a convention / best practice for specifying environment variables. Many tools (such as Docker or other deployment tooling) "understand" `.env` files - and so does `now` when using the [`--dotenv`](https://zeit.co/docs/features/env-and-secrets#--dotenv-option) flag.

<Instruction>

All you need to is navigate into the `hello-advanced` directory and invoke `now` with that argument:

```bash
cd hello-advanced
now --dotenv .env
```

</Instruction>

If you deployed your Prisma service locally with Docker, your `.env` file will contain local references for the environment variables `PRISMA_ENDPOINT` (such as `http://localhost:60000/hello-advanced/dev`). In that case you can create another `.env` file (e.g. called `.env.prod`) and make sure `PRISMA_ENDPOINT` is set to the proper remote URL (of course that requires that you properly deploy the Prisma service to some public server on the web before)  and then refer to that one during deployment:

```sh
now --dotenv .env.prod
```

You can find some documentation about how to handle environment variables and secrets when using `now` [here](https://zeit.co/docs/features/env-and-secrets).
