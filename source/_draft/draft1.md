---
title : "How to build a multistage Dockerfile for SPA and static sites"
tags: [nginx, docker, static content, nodejs]
date: 2019-02-20 15:44:56
---

When you are a consultant, your goal is to think about the best way to save money for your client. They are not paying us because we can code. They are paying because we can remove a few dollars (or a few hundred) from their bills.

One of the situations we often find ourselves in is building a single page application (SPA). Clients want dynamically driven applications that don't refresh the whole page, and a SPA is often the perfect choice for them. Among the many tools used to build a SPA, we find Angular, Vue, and React.

I've found that delivering websites with containers is a universal way of ensuring compatibility across environments, cloud or not. It also prevents a developer's environment from having to install 25 different tools/languages/SDKs.

It keeps thing concise and efficient.

If you want to [know more about Docker containers](https://docs.microsoft.com/dotnet/standard/containerized-lifecycle-architecture/what-is-docker?WT.mc_id=maximerouiller-blog-marouill), take a few minutes, in particular, to read about [the terminology](https://docs.microsoft.com/dotnet/standard/containerized-lifecycle-architecture/docker-terminology?WT.mc_id=maximerouiller-blog-marouill).

The problem is that we only need Node.js to build that application, not to run it. So, how would containers solve our problem? There's a concept in Docker called [Multistage builds](https://docs.docker.com/develop/develop-images/multistage-build/) where you can separate the build process from the execution.

Here’s a template you can use to build a SPA with Node.js.

## Dockerfile template for Node.js

```dockerfile
#build stage for a Node.js application
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

## Build Stage (Node.js)

Multistage docker builds allow us to split our container in two ways. Let's look at the build stage.

The first line is a classic. We're starting from an Alpine image that has Node.js pre-installed on it.

> Note: Alpine Linux is a security-oriented, lightweight Linux distribution based on musl libc and busybox. Its main characteristic is it runs everywhere and is extremely small, around 5MB.
 

We're configuring `/app` as the working directory. Then, we do something unusual. We copy our `package*.json` files before copying everything else.

Why? Each line in a Dockerfile represents a layer. When building a layer, if a layer already exists locally, is retrieved from the cache instead of being rebuilt. By copying and installing our packages in a separate step, we avoid running `npm install` on dependencies that didn't change in the first place. Since `npm install` can take a while to install, we save some time there.

Finally, we copy the rest of our app and run the npm `build` task. If your application doesn't have a `build` task, change the name to whatever tasks generate an output folder like `dist`.

The result? We have a correctly built Node.js application located in `/app/dist`.

## Production Stage

We've generated our SPA or static site with Node.js but... our application isn't using Node.js. It's using HTML/CSS/JS. We don't need a Node.js image to take our application to production. Instead, we only need an HTTP server. Let's use the [NGINX Docker Image](https://hub.docker.com/_/nginx) as a host.

We copy the output from our previously defined `build-stage` `/app/dist` folder into the NGINX defined folder `/usr/share/nginx/html` as [mentioned in their docs](https://github.com/docker-library/docs/tree/master/nginx#how-to-use-this-image).

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

This command is doing two things. First, it runs the container in interactive mode with the `-i` flag. That flag will allow us to see the output of NGINX as it runs. Second, it maps port 8080 of your local machine to port 80 of the container.

Opening your browser to `http://localhost:8080` will show you your website.

## Conclusion

I'm using Docker more and more for everything. I'm building applications that are single use with current technology. Docker empowers me in running applications with older versions of frameworks, runtime, languages, without causing tooling versioning issue with my machine.

While technology may continue to evolve, I'm never afraid that my Docker container won't work anymore. Things have been stuck in time if only for a moment.

That means I don't have to upgrade that AngularJS 1.X app to stay cool. If it works, it works.

Are you using Docker in unusual ways? Share them with me [on Twitter](https://twitter.com/MaximRouiller)!