# Dev Readme

## Overview
- backstage is a bit unusual in that you have to build the app yourself
- it's also a ball ache to install app on MacOS due to.. well being MacOS
- creating Backstage app runs locally with SQLite database and demo content

### Create Backstage App
- [docs](https://backstage.io/docs/getting-started/)

```bash
# prerequisites (all distros)
curl -v
cmake --version
docker -v
gcc -v
git -v
make -v
pkg-config --version
yarn -v

# macOS typically uses Clang as default C++ compiler
# make sure version supports C++20 (Clang 10 or later)
clang --version

# macos
xcode-select --install

# install latest version of node and npm
nvm install node &&
npm install -g npm@latest

# install node-gyp
# (node native addon build tool)
npm install -g node-gyp@latest

# install isolated-vm
# (isolated js environments for node)
npm install -g isolated-vm@latest

# had to brew update and rerun prev three commands a couple times for
# isolated-vm to install properly

# backstage Github releases
https://github.com/backstage/backstage/releases

# check latest npm version
npm show @backstage/create-app version

# show last 15 npm versions
npm show @backstage/create-app versions --json | tail -n 15

# create backstage app
npx @backstage/create-app@latest

# create backstage app (specifying fixed version)
npx @backstage/create-app@0.5.21

# proceed (y) and use default name (backstage)
```

## Run App Locally (SQLite)
```bash
# launch dev server
cd backstage && yarn dev

# open app
http://localhost:3000
```

## Configure App For Database (PostgreSQL)
```bash
# install postgres client
yarn --cwd packages/backend add pg

# amend app config
# backstage/app-config.yaml
backend:
  database:
    # config options: https://node-postgres.com/apis/client
    client: pg
    connection:
      host: ${POSTGRES_HOST}
      port: ${POSTGRES_PORT}
      user: ${POSTGRES_USER}
      password: ${POSTGRES_PASSWORD}

# will now need to export envars when running locally
# but this configures everything nicely for app to be containerized

# export envars when running locally
EXPORT POSTGRES_HOST=...
EXPORT POSTGRES_PORT=...
EXPORT POSTGRES_USER=...
EXPORT POSTGRES_PASSWORD=...
```