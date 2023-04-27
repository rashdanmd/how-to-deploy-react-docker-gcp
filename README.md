# How to deploy a react app with Google cloud

This React app has been created as a guide and reference for deploying a React app with GCP

## Technology 
- React
- Docker*
- Nginx
- Google Cloud Provider

*Install [Docker Desktop]('https://www.docker.com/products/docker-desktop/') on your machine


## Setup Docker files
Create the following files:
- .dockerignore - ignore files such as node modules etc.
- Dockerfile - multi stage build


### .dockerignore

```
node_modules
npm-debug.log
```

### Dockerfile
```Docker
# Stage 0 - Build frontend Assets
FROM node: 12.16.3-alipne as build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build


# Stage 1 - Serve frontend Assets
FROM fholzer/nginx-brotli:v1.12.2

WORKDIR /etc/nginx
ADD nginx.conf /etc/nginx/nginx.conf

COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 443
CMD ["nginx", "-g", "daemon off;"]
```


## Setup niginx file

Create the following file:
- niginx.conf

## Build and run

### Build Docker image
```
docker build -t react-docker-gcp:latest .
```
### Run Docker image
```
docker run -p 8080:443 react-docker-gcp:latest
```

Head to your broswer and type the followig url: [http://localhost:8080]('http://localhost:8080/'). Your app should now be running.

## Setting up GCP
go to [cloud.google.com](cloud.google.com) and navigate to your console.
- Create a new project - fill in name input
- Once project is created, you should now see project info the dashboard
- At the top of the page search Cloud Run
- Click create service
- Select 2nd option, 'Continuously deploy new revisions from a source repository', followed by 'Set up with cloud build'
- Select Github in Repository Provider dropdown
- Select repository
- Fill in service name
- Select region accordingly

Build configuration
- Select main in Branch dropdown
- Select Dockerfile for the build type
- Type /Dockerfile in source location

Advanced options
- Type 443 in Container port field

Authentication
- select 'Allow unauthorised invocations'


Finally click create

## References

[Cloud Guru](https://www.youtube.com/watch?v=7BAwiZhuREg&list=PLghwmWptCabcpLX5pPxbLu_ym5uMI23AV&index=2&t=371s)
