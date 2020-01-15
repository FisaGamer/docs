# Server link

## Introduction

In order to be able to link your server with your site under Azuriom, you will have three possibilities:

* By Ping - **This is the most limited solution**, it just allows you to get 
the players connected to your server. _(does not allow to execute commands)_

* By Rcon - **This is the intermediate solution**, it allows you to retrieve the information 
of your server and execute commands.

* By Plugin - **This is the optimal solution**, it will allow you to have the functionality of the Rcon 
but with more advanced functionalities _(example: an advanced statistics system)_

## Link By Ping

To be able to link your server with your site under Azuriom by ping, 
you just have to add a new server with "Ping" as the link type.
and fill in the requested information _(the default port is 25565)_.

## link by Rcon

To be able to link your server with your website under Azuriom by Rcon, 
You must:

1. Go to the `server.properties` file of your server

2. Configure this file as follows:
    * Set `enable-rcon` to `true`.
    * Put `rcon.password` with `your-password`.
    * Set `rcon.port` with `your-port` _(default 25575)_
    * Backup and restart your server
   
3. Go to your site and add a new server with the link type "Rcon".
and fill in the requested information. _(Default Rcon port is 25575)_.

## Plugin link

### What is AzLink ?

AzLink is a site-to-server link plugin specially designed for Azuriom. 
to allow you to link your server to your site simply, quickly and securely.
AzLink supports Bukkit, BungeeCord, Sponge and Velocity in the same plugin. A legacy version is available
for Bukkit 1.7.10.

### Installation

1. Download AzLink from [our site](https://azuriom.com/azlink)

2. Install it on your server in the `Plugin/` folder and restart your server.

3. Go to your site and add a new server with the link type "AzLink", 
follow the link steps and fill in the requested information.