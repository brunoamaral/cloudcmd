# Cloud Commander v14.1.2 [![Build Status][BuildStatusIMGURL]][BuildStatusURL] [![Codacy][CodacyIMG]][CodacyURL] [![Gitter][GitterIMGURL]][GitterURL] [![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fcoderaiser%2Fcloudcmd.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2Fcoderaiser%2Fcloudcmd?ref=badge_shield)

### [Main][MainURL] [Blog][BlogURL] Live([Heroku][HerokuURL])

[NPM_INFO_IMG]:             https://nodei.co/npm/cloudcmd.png
[MainURL]:                  http://cloudcmd.io "Main"
[BlogURL]:                  http://blog.cloudcmd.io "Blog"
[HerokuURL]:                https://cloudcmd.herokuapp.com/ "Heroku"
[BuildStatusURL]:           https://travis-ci.org/coderaiser/cloudcmd  "Build Status"
[BuildStatusIMGURL]:        https://img.shields.io/travis/coderaiser/cloudcmd.svg?style=flat-squere&longCache=true

[BuildAppveyorURL]:         https://ci.appveyor.com/project/coderaiser/cloudcmd
[BuildAppveyorIMGURL]:      https://ci.appveyor.com/api/projects/status/tse6sc8dxrqxqehi?svg=true

[CodacyURL]:                https://www.codacy.com/app/coderaiser/cloudcmd
[CodacyIMG]:                https://api.codacy.com/project/badge/Grade/ddda78be780549ce8754f8d47a8c0e36

[GitterURL]:                https://gitter.im/cloudcmd/hello
[GitterIMGURL]:             https://img.shields.io/gitter/room/coderaiser/cloudcmd.js.svg

[DeployURL]:                https://heroku.com/deploy?template=https://github.com/coderaiser/cloudcmd "Deploy"
[DeployIMG]:                https://www.herokucdn.com/deploy/button.png

**Cloud Commander** a file manager for the web with console and editor.

![Cloud Commander](https://cloudcmd.io/img/logo/cloudcmd.png "Cloud Commander")

## Install

```
npm i cloudcmd -g
```
## Start

For starting just type in console:

```sh
cloudcmd
```

## How to use?

Open url `http://localhost:8000` in browser.

### View

You will see something similar to this.
![View](https://cloudcmd.io/img/screen/view.png "View")


## Deploy
`Cloud Commander` could be easily deployed to [Heroku][DeployURL].

[![Deploy][DeployIMG]][DeployURL]

## Using as Middleware

Cloud Commander could be used as middleware for `node.js` applications based on [socket.io](http://socket.io "Socket.IO") and [express](http://expressjs.com "Express"):

Init `package.json`:

```
npm init -y
```

Install dependencies:

```
npm i cloudcmd express socket.io -S
```

And create `index.js`:

```js
const http = require('http');
const cloudcmd = require('cloudcmd');
const io = require('socket.io');
const app = require('express')();

const port = 1337;
const prefix = '/';

const server = http.createServer(app);
const socket = io.listen(server, {
    path: `${prefix}/socket.io`
});

const config = {
    name: 'cloudcmd :)',
};

const filePicker = {
    data: {
        FilePicker: {
            key: 'key',
        }
    }
};

// override option from json/modules.json
const modules = {
    filePicker,
};

const {
    createConfigManager,
    configPath,
} = cloudcmd;

const configManager = createConfigManager({
    configPath,
}),

app.use(prefix, cloudcmd({
    socket,  // used by Config, Edit (optional) and Console (required)
    config,  // config data (optional)
    modules, // optional
    configManager, // optional
}));

server.listen(port);
```

Docker
---------------
`Cloud Commander` could be used as a [docker container](https://hub.docker.com/r/coderaiser/cloudcmd/ "Docker container") this way:

```sh
docker run -t --rm -v ~:/root -v /:/mnt/fs -w=/root -p 8000:8000 coderaiser/cloudcmd
```

Config would be read from home directory, hosts root file system would be mount to `/mnt/fs`,
`8000` port would be exposed to hosts port.

Also you could use [docker compose](https://docs.docker.com/compose/ "Docker Compose") with `docker-compose.yml`:

```yml
version: '2'
services:
  web:
    ports:
      - 8000:8000
    volumes:
      - ~:/root
      - /:/mnt/fs
    image: coderaiser/cloudcmd
```

When you create this file run:

```sh
docker-compose up
```

Get involved
---------------

There is a lot ways to be involved in `Cloud Commander` development:

- if you find a bug or got idea to share [create an issue](https://github.com/coderaiser/cloudcmd/issues/new "Create issue");
- if you fixed a bug, typo or implemented new feature [create pull request](https://github.com/coderaiser/cloudcmd/compare "Create pull request");
- if you know languages you can help with [site translations](https://github.com/coderaiser/cloudcmd/wiki "Cloud Commander community wiki");

## License

MIT
