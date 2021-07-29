# Keycloak integration with Nginx based on docker engine

Example for Nginx server configuration for serving proxy requests to server API by passing authorization requests to Keycloak.

Such use cases are very useful in case we want to provide the ability to limit access to specific microservice and require username and password before proxying the request.

<b>Keycloak</b> is an open-source identity and access management service. It offers all the features you might need, like multi-factor authentication, integration with common identity providers, user federation, brute force protection, and many others.

Docker compose file contains a simple web server written in Python, which serves basic API route. Each hit to the API counts in Redis. The purpose is to hide access to the server and provide an authentication layer before.

Nginx server will serve us to proxy requests to the server API after the authorization process is completed.To integrate Nginx with Keycloak, we need Lua dependency. Openresty is a web server built on top of Nginx. I’ve created a new docker image based on classic nginx with adding Lua dependency. Dockerfile for the image is here : ./Dockerfile

Nginx server requires a configuration file which is located in a repository. This file contains proxy configuration for “/web” endpoint. This will redirect us to the web server after successful login. Server section in the config file contains the authorization setup required by Keycloak.

<b>Important note!</b> In order to run this stuff on Windows, docker ip (172.17.0.1) should be replaced with “host.docker.internal” 

The Keycloak application served as a separate container with PostgreSql database as data storage. In order to configure Keycloak to provide authorization level for this example we need to login to the administration area with password and username defined in container environment variables. Admin url: http://localhost:3333/auth/admin/. New realm, client and user should be created as shown in screenshots in the repository. 

If everything is configured properly, on access to “http://localhost/web” we will see the login screen provided by Keycloak and after entering user details which was created in Keycloak we will be redirected to our web server.
