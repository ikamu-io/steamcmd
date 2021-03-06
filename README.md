steamcmd in Docker
===

This repository provides everything you need to run steamcmd in Docker.

For convenience it contains the following server files:
* Just Cause 2:Multiplayer (no login required)
* Starbound (login required)

Why?
---
The main hurdle of running steamcmd in Docker is that steamcmd supports
multiple servers.
Compared to other steamcmd docker images, this one exposes a `volume` named
`config` as well as an `environment` variable to tell steamcmd which server file
to use. This means:

* One container can be used to run multiple servers
* Server scripts can be created externally and mounted into the container
* Container automatically updates game via config file and then starts server via sh file.

Run
---

To run the container with the default JC2:MP server file:

```bash
docker run -td -p 7777:7777 -p 27015:27015 -p 20560:20560 ghetto/steamcmd:latest
```

The `ghetto/steamcmd` container can also be run with custom config and sh files.  This allows
you to start container and have the update and server start process be completely automated.

```bash
docker run -td -p 7777:7777 -p 27015:27015 -p 20560:20560 -v /myscripts:/config -e SCRIPT=jc2.config -e INIT=start-jc2.sh ghetto/steamcmd:latest
```

The `ghetto/steamcmd` container can also be run with login credentials to download
games that require a login.

```bash
docker run -td -p 7777:7777 -p 27015:27015 -p 20560:20560 -v /myscripts:/config -e USERNAME=gaben -e PASSWORD=valve -e SCRIPT=starbound.config -e INIT=start-starbound.sh ghetto/steamcmd:latest
```

Volumes
---

* `/config` => Directory containing server scripts

Ports
---

* 7777 => game port
* 27015 => query port
* 20560 => steam

Environment Variables
---

* SCRIPT => jc2.config (default server file to use when launching steamcmd)
* INIT => start-jc2.sh (default bash script to start the JC2:MP server)
* USERNAME => anonymous (default username to use with login steamcmd command)
* PASSWORD => "" (default password to use with the login steamcmd command)

In the box
---
* **ghetto/steamcmd**

* The docker image with steamcmd. Built from the `accursoft/micro-debian`
  Debian micro image.
* Just Cause 2: Multiplayer server file example (no login)
* Starbound server file example (requires login)

Public Builds
---

https://registry.hub.docker.com/u/ghetto/steamcmd/


Build from Source
---

    docker build -t ghetto/steamcmd .

Notes
---

* Additional ports can be mapped as required by using the `-p` flag.
* See starbound.config/start-starbound.sh for how the system replaces credentials.

Todo
---

* Add an option to download config files from a GIT repo.
* Add config files and environment variables for games that require login info.
