---
title : "How to build a multistage Dockerfile for SPA and static sites"
tags: [nginx, docker, static content, nodejs]
date: 2019-02-20 15:44:56
---

When I was a consultant, my goal was to think about the best way to save money for my client. They are not paying me because I can code. They are paying because I know I can remove a few dollars (or a few hundred) from their bills when I can.

One of the situations I've found myself in often is building a static page application (SPA). It is a typical scenario with tools like AngularJS, VueJS, and ReactJS. Clients want dynamically driven applications that don't refresh the whole page.

I've found that delivering websites with containers is a universal way of ensuring compatibility across environments, cloud or not. It also prevents a developer's environment from having to install 25 different tools/languages/SDKs.

It keeps thing concise and efficient.

If you want to [know more about Docker containers](https://docs.microsoft.com/dotnet/standard/containerized-lifecycle-architecture/what-is-docker?WT.mc_id=maximerouiller-blog-marouill), take a few minutes, in particular, to read about [the terminology](https://docs.microsoft.com/dotnet/standard/containerized-lifecycle-architecture/docker-terminology?WT.mc_id=maximerouiller-blog-marouill).

## Dockerfile template for NodeJS

```dockerfile
#build stage for a NodeJS application
FROM node:lts-alpine as build-stage
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

#production stage
FROM nginx:stable-alpine as production-stage
COPY --from=build-stage /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

There's a lot to unpack here. Let's look at the two stages separately.

> [Read more about Docker Multistage builds](https://docs.docker.com/develop/develop-images/multistage-build/).

## Build Stage (NodeJS)

Multistage docker builds allow us to split our container in two ways. Let's look at the build stage.

The first line is a classic. We're starting from a ridiculously small (~5MB) Alpine image that has node pre-installed on it. We're configuring `/app` as the working directory. Then, we do something unusual. We copy our `package*.json` files before copying everything else.

Why? Each line in a Dockerfile represents a layer. When building a layer, if a layer already exists locally, is retrieved from the cache instead of being rebuilt. By copying and installing our packages in a separate step, we avoid running `npm install` on dependencies that didn't change in the first place. Since `npm install` can take a while to install, we save some time there.

Finally, we copy the rest of our app and run the `build` task. If your application doesn't have a `build` task, change the name to whatever tasks generate a `dist` folder.

The result? We have a correctly built NodeJS application located in `/app/dist`.

## Production Stage

Now the fun begins. We've generated our SPA or Static Site with node but... our application isn't using node. It's using HTML/CSS/JS. We don't need a NodeJS image to take our application to production. Instead, let's use the [NGINX Docker Image](https://hub.docker.com/_/nginx).

From then, we copy the output from our previously defined `build-stage` `/app/dist` folder into the NGINX defined folder `/usr/share/nginx/html` as [mentioned in their docs](https://github.com/docker-library/docs/tree/master/nginx#how-to-use-this-image).

After exposing port 80, we need to run NGINX with the `daemon off;` option to have it run in the foreground and preventing the container from closing.

## Building the Dockerfile

This step is easy. Run the following command in the folder containing the `Dockerfile`.

```bash
docker build -t mydockerapp:latest .
```

## Running the Docker container locally

Running the application on your machine is of course just a simple command away.

```bash
docker run -it -p 8080:80 mydockerapp:latest
```

Opening your browser to `http://localhost:8080` will show you your website.

## Conclusion

I'm using Docker more and more for everything. I'm building applications that are single use with current technology. While technology may continue to evolve, I'm never afraid that my Docker container won't work anymore. Things have been stuck in time if only for a moment.

That means I don't have to upgrade that AngularJS 1.X app to stay cool. If it works, it works.

Are you using Docker in unusual ways? Share them with me [on Twitter](https://twitter.com/MaximRouiller)!