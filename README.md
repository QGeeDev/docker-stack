# QG Stack

This is the stack I use to run my environment on Docker, hosted on a Raspberry Pi 4.

Provides a Grav CMS webpage at the root of your domain, as well as `gitea.yourdomain.io` as a self-hosted git repository.

# Running the project

- Confirm settings for email and domain name in `docker-compose.yml`
- Confirm that image of `gitea` in `docker-compose.yml` is correct
- Initialise containers with `docker-compose up -d`

# Components

## SWAG
_Secure Web Application Gateway_ by [LinuxServer.io](https://docs.linuxserver.io/general/swag "Documentation for LinuxServer's SWAG image") is an image to provide a secure web gateway for Docker images. It allows for a large list of Docker images to be added in and used as subdomains or subfolders at the URL.

## Grav
Grav is a lightweight CMS that is packaged into a Docker image by [LinuxServer.io](https://docs.linuxserver.io/images/docker-grav "Documentation for LinuxServer's Grav image"). This is used as the primary base of my website, as well as of this Docker stack.

## Gitea
[Gitea](https://gitea.io) is a self-hosted and open source git service. [This specific image](https://github.com/strobi/docker-rpi-gitea) is made by Strobi, and is compiled for the ARMv7 architecture, allowing it to run on a Raspberry Pi

# File structure

- Volumes are mounted into the `data` folder at the root of the project
- Within `data`, each service stores its volumes in a named folder, absed on the name of the project. E.G. `gitea`, `grav`.

# Additional files

Some additional files have been provided in the volume locations. Although the images will populate the majority of these files, a few of them are custom configurations set up to allow for certain functionality.

```data/swag/config/nginx/proxy-confs```

This overwrites the default config for SWAG and sets the Grav directory as the root of the website. It also enables the `gitea` subdomain. The rest of this folder will be populated by the creation of the images as part of the `up` command.

# Common issues
- If you are using Cloudflare or another proxying service as part of your DNS, the proxy will need to be disabled for initial setup, otherwise SWAG will fail to get security certificates