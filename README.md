# gblstack
A dockerized development stack for GeoBlacklight.

Adapted from Northwestern University Library's [devstack project](https://github.com/nulib/devstack).

## Setup

```bash
$ git clone https://github.com/geoblacklight/gblstack.git
$ cd gblstack

# On MacOS
$ echo "export PATH=\$PATH:$(pwd)/bin" >> ~/.bash_profile
$ source ~/.bash_profile

# On Linux
$ echo "export PATH=\$PATH:$(pwd)/bin" >> ~/.profile
$ source ~/.profile
```

## Use

The `gblstack` command is a thin wrapper around [`docker-compose`](https://docs.docker.com/compose/reference/), and accepts
all of the same subcommands, parameters, and arguments, with a few notable enhancements:

* The `update` command will update the gblstack command and configs in place.
* `docker-compose` commands that accept service names as arguments can also accept application names
  (`web`, `solr`, `db`) that will be expanded to the list of services those
  applications depend on. For example, `gblstack up web` will bring up all services required
  to run the blacklight web server.

### Cheat Sheet

* `gblstack -t [rest-of-command]` will run `gblstack` in test mode. That means:
  * All services run on test ports instead of dev ports (e.g., `solr` on `8985` instead of `8983`)
  * The `COMPOSE_PROJECT_NAME` will have `_test` appended (e.g., the default container names will be 
    `gblstack_test_*` instead of `gblstack_*`)
  * The stack will clean itself up (delete all volumes and persistent data) when it spins down.
* `gblstack up [-d] [SERVICE|APPLICATION...]`: Bring up all requested services (default: all services).
  `-d` will run everything in the background.
* `gblstack logs [-f] [SERVICE|APPLICATION...]`: Show the container logs for the specified services (default:
  all running services). `-f` behaves the way it does for the `tail` command.
* `gblstack down [-v]`: Bring down the stack. Adding `-v` will destroy the stack's persistent data volumes as
  well, resulting in a clean slate on the next `up`.
* `gblstack ps`: View the current state of running containers
* `gblstack update`: Upgrade to the latest revision of `gblstack`

### Tips and Tricks

* Setting the `COMPOSE_PROJECT_NAME` environment variable will change the prefix `docker-compose` uses when
  creating containers and volumes, allowing you to create a temporary stack without interfering with your
  main stack. This can be useful for one-off debugging.
* It’s a good idea to run `docker system prune` once in a while to get rid of stopped containers, dangling
  images, and unused networks & volumes. If you run it while the stack is up, it won’t remove your current
  set of data volumes (because they'll be in use).
