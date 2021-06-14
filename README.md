# coinos-server

Coinos is a bitcoin wallet app that supports payments over the <a href="https://bitcoin.org">bitcoin</a>, <a href="https://blockstream.com/liquid/">liquid</a> and <a href="http://lightning.network/">lightning</a> networks. Try it out at <a href="https://coinos.io/">coinos.io</a>.

This repository contains the code for the backend API server which is implemented as a NodeJS application. The code for the frontend UI is tracked separately <a href="https://github.com/asoltys/coinos.io">here</a> (but is automatically installed & started via the Docker way outlined below). 

## Install/Run (the Docker way)

### Requirements 

* <a href="https://docs.docker.com/get-docker/
">docker</a> and <a href="https://docs.docker.com/compose/install/">docker-compose</a>

### Getting Started (WIP)

    git clone git@github.com:coinos/coinos-server.git
    cd coinos-server
    cp -rf sampleconfig ./config
    cp .env.sample .env
    # for local dev/experimenting, edit docker-compose.yml to comment-out the nginx and certbot services 
    docker-compose up

Note the last step currently downloads/installs ~7GB of data (ie- to /var/lib/docker; primarily for Liquid) 

After building & successful creation of all docker containers coinos will be available at http://localhost:8085

Todo: outline remaining steps to ensure proxy routes work (ie- /api and /ws)

----

### Install/Run (without Docker)

Note the Docker way to get setup is recommended as manual setup requirements/instructions shown below are somewhat deprecated. 


#### Requirements

* <a href="https://github.com/bitcoin/bitcoin">bitcoind</a> with zmq support
* <a href="https://github.com/ElementsProject/elements">elementsd</a> with zmq support
* two instances of <a href="https://github.com/lightningnetwork/lnd">lnd</a> (<a href="https://github.com/elementsproject/lightning">c-lightning</a> coming soon)
* a database that <a href="https://github.com/sequelize/sequelize">sequelize</a> can talk to

The bitcoind and elementsd nodes can be a pruned if you want to limit the amount of disk space used.

The reason for running two lightning nodes is so that one can create invoices while the other sends payments when two coinos users want to pay each other. 

#### Getting Started

    git clone git@github.com:coinos/coinos-server.git
    cd coinos-server
    cp -rf sampleconfig ./config
    cp .env.sample .env
    yarn
    yarn start


#### Database Setup

I've only tested with <a href="https://mariadb.org/">Maria</a>. Here's a [schema](https://github.com/asoltys/coinos-server/blob/master/db/schema.sql) to get you started.

    cat db/schema.sql | mysql -u root -p
