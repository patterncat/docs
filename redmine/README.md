<!--

********************************************************************************

WARNING:

    DO NOT EDIT "redmine/README.md"

    IT IS AUTO-GENERATED

    (from the other files in "redmine/" combined with a set of templates)

********************************************************************************

-->

# Supported tags and respective `Dockerfile` links

-	[`3.1.7`, `3.1` (*3.1/Dockerfile*)](https://github.com/docker-library/redmine/blob/665a1f399082dc01543b36c9aecd0cf4c5ee214e/3.1/Dockerfile)
-	[`3.1.7-passenger`, `3.1-passenger` (*3.1/passenger/Dockerfile*)](https://github.com/docker-library/redmine/blob/06af4923eb88ca34909dc0fa4c2be77e6e80064d/3.1/passenger/Dockerfile)
-	[`3.2.6`, `3.2` (*3.2/Dockerfile*)](https://github.com/docker-library/redmine/blob/b994741065b7a297d030b7826c478655a10f26bd/3.2/Dockerfile)
-	[`3.2.6-passenger`, `3.2-passenger` (*3.2/passenger/Dockerfile*)](https://github.com/docker-library/redmine/blob/06af4923eb88ca34909dc0fa4c2be77e6e80064d/3.2/passenger/Dockerfile)
-	[`3.3.3`, `3.3`, `3`, `latest` (*3.3/Dockerfile*)](https://github.com/docker-library/redmine/blob/5453a92c4f8d18e59de9162c4030fb277bc72e8f/3.3/Dockerfile)
-	[`3.3.3-passenger`, `3.3-passenger`, `3-passenger`, `passenger` (*3.3/passenger/Dockerfile*)](https://github.com/docker-library/redmine/blob/06af4923eb88ca34909dc0fa4c2be77e6e80064d/3.3/passenger/Dockerfile)

# Quick reference

-	**Where to get help**:  
	[the Docker Community Forums](https://forums.docker.com/), [the Docker Community Slack](https://blog.docker.com/2016/11/introducing-docker-community-directory-docker-community-slack/), or [Stack Overflow](https://stackoverflow.com/search?tab=newest&q=docker)

-	**Where to file issues**:  
	[https://github.com/docker-library/redmine/issues](https://github.com/docker-library/redmine/issues)

-	**Maintained by**:  
	[the Docker Community](https://github.com/docker-library/redmine)

-	**Published image artifact details**:  
	[repo-info repo's `repos/redmine/` directory](https://github.com/docker-library/repo-info/blob/master/repos/redmine) ([history](https://github.com/docker-library/repo-info/commits/master/repos/redmine))  
	(image metadata, transfer size, etc)

-	**Image updates**:  
	[official-images PRs with label `library/redmine`](https://github.com/docker-library/official-images/pulls?q=label%3Alibrary%2Fredmine)  
	[official-images repo's `library/redmine` file](https://github.com/docker-library/official-images/blob/master/library/redmine) ([history](https://github.com/docker-library/official-images/commits/master/library/redmine))

-	**Source of this description**:  
	[docs repo's `redmine/` directory](https://github.com/docker-library/docs/tree/master/redmine) ([history](https://github.com/docker-library/docs/commits/master/redmine))

-	**Supported Docker versions**:  
	[the latest release](https://github.com/docker/docker/releases/latest) (down to 1.6 on a best-effort basis)

# What is Redmine?

Redmine is a free and open source, web-based project management and issue tracking tool. It allows users to manage multiple projects and associated subprojects. It features per project wikis and forums, time tracking, and flexible role based access control. It includes a calendar and Gantt charts to aid visual representation of projects and their deadlines. Redmine integrates with various version control systems and includes a repository browser and diff viewer.

> [wikipedia.org/wiki/Redmine](https://en.wikipedia.org/wiki/Redmine)

![logo](https://raw.githubusercontent.com/docker-library/docs/969091c4c590befe236a71d4a7bce5823fff020d/redmine/logo.png)

# How to use this image

## Run Redmine with SQLite3

This is the simplest setup; just run redmine.

```console
$ docker run -d --name some-redmine redmine
```

> not for multi-user production use ([redmine wiki](http://www.redmine.org/projects/redmine/wiki/RedmineInstall#Supported-database-back-ends))

## Run Redmine with a Database Container

Running Redmine with a database server is the recommened way.

1.	start a database container

	-	PostgreSQL

		```console
		$ docker run -d --name some-postgres -e POSTGRES_PASSWORD=secret -e POSTGRES_USER=redmine postgres
		```

	-	MySQL (replace `--link some-postgres:postgres` with `--link some-mysql:mysql` when running redmine)

		```console
		$ docker run -d --name some-mysql -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_DATABASE=redmine mysql
		```

2.	start redmine

	```console
	$ docker run -d --name some-redmine --link some-postgres:postgres redmine
	```

## ... via [`docker-compose`](https://github.com/docker/compose)

Example `docker-compose.yml` for `redmine`:

```yaml
version: '2'

services:

  redmine:
    image: redmine
    ports:
      - 8080:3000
    environment:
      REDMINE_DB_MYSQL: db
      REDMINE_DB_PASSWORD: example
    depends_on:
      - db
    restart: always

  db:
    image: mariadb
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: redmine
    restart: always
```

Run `docker-compose up`, wait for it to initialize completely, and visit `http://localhost:8080` or `http://host-ip:8080`.

## Alternative Web Server

The other tags in this repository, like those with `passenger`, use the same environment and `--links` as the default tags that use WEBrick (`rails s`) but instead give you the option of a different web and application server. `passenger` uses [Phusion Passenger](https://www.phusionpassenger.com/). [`tini`](https://github.com/krallin/tini) is used for reaping [zombies](https://en.wikipedia.org/wiki/Zombie_process).

## Accessing the Application

Currently, the default user and password from upstream is admin/admin ([logging into the application](https://www.redmine.org/projects/redmine/wiki/RedmineInstall#Step-10-Logging-into-the-application)).

## Where to Store Data

Important note: There are several ways to store data used by applications that run in Docker containers. We encourage users of the `redmine` images to familiarize themselves with the options available, including:

-	Let Docker manage the storage of your files [by writing the files to disk on the host system using its own internal volume management](https://docs.docker.com/engine/tutorials/dockervolumes/#adding-a-data-volume). This is the default and is easy and fairly transparent to the user. The downside is that the files may be hard to locate for tools and applications that run directly on the host system, i.e. outside containers.
-	Create a data directory on the host system (outside the container) and [mount this to a directory visible from inside the container](https://docs.docker.com/engine/tutorials/dockervolumes/#mount-a-host-directory-as-a-data-volume). This places the database files in a known location on the host system, and makes it easy for tools and applications on the host system to access the files. The downside is that the user needs to make sure that the directory exists, and that e.g. directory permissions and other security mechanisms on the host system are set up correctly.

The Docker documentation is a good starting point for understanding the different storage options and variations, and there are multiple blogs and forum postings that discuss and give advice in this area. We will simply show the basic procedure here for the latter option above:

1.	Create a data directory on a suitable volume on your host system, e.g. `/my/own/datadir`.
2.	Start your `redmine` container like this:

	```console
	$ docker run -d --name some-redmine -v /my/own/datadir:/usr/src/redmine/files --link some-postgres:postgres redmine
	```

The `-v /my/own/datadir:/usr/src/redmine/files` part of the command mounts the `/my/own/datadir` directory from the underlying host system as `/usr/src/redmine/files` inside the container, where Redmine will store uploaded files.

Note that users on host systems with SELinux enabled may see issues with this. The current workaround is to assign the relevant SELinux policy type to the new data directory so that the container will be allowed to access it:

```console
$ chcon -Rt svirt_sandbox_file_t /my/own/datadir
```

## Port Mapping

If you'd like to be able to access the instance from the host without the container's IP, standard port mappings can be used. Just add `-p 3000:3000` to the `docker run` arguments and then access either `http://localhost:3000` or `http://host-ip:3000` in a browser.

## Environment Variables

When you start the `redmine` image, you can adjust the configuration of the instance by passing one or more environment variables on the `docker run` command line.

### `REDMINE_NO_DB_MIGRATE`

This variable allows you to control if `rake db:migrate` is run on container start. Just set the variable to a non-empty string like `1` or `true` and the migrate script will not automatically run on container start.

`db:migrate` will also not run if you start your image with something other than the default `CMD`, like `bash`. See the current `docker-entrypoint.sh` in your image for details.

### `REDMINE_SECRET_KEY_BASE`

This variable is used to create an initial `config/secrets.yml` and set the `secret_key_base` value, which is "used by Rails to encode cookies storing session data thus preventing their tampering. Generating a new secret token invalidates all existing sessions after restart" ([session store](https://www.redmine.org/projects/redmine/wiki/RedmineInstall#Step-5-Session-store-secret-generation)). If you do not set this variable or provide a `secrets.yml` one will be generated using `rake generate_secret_token`.

# License

[Redmine](https://www.redmine.org/projects/redmine/wiki) is open source and released under the terms of the [GNU General Public License v2](https://www.gnu.org/licenses/old-licenses/gpl-2.0.html) (GPL).
