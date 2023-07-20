# Index

# Full Stack with external Node Setup

In this tutorial we will see how to run one or more Liqwid Finance bots along with all the required services leveraging
an already running Cardano Node. The great thing about this setup is that you can reuse a Cardano Node that is already
running on the same machine saving useful cpu and memory. This setup is highly performant.

The services you will run with this setup are:

1. Ogmios
2. Kupo (on disk db)
3. Your bot(s)

## Hardware Requirements

This setup is extremely lightweight and requires only a couple of cores and less than 8GB of RAM

| Service      | CPU | Memory | Disk   |
|--------------|-----|--------|--------|
| Ogmios       | 0.5 | 256 MB | N/A    |
| Kupo         | 0.5 | 4 GB   | 5 GB   |
| 1 x Bot      | 0.5 | 256 MB | N/A    |

## Managing the stack

The docker compose file you will work with is the `docker-compose-no-node.yaml`.

The fist step is to customise the `docker-compose-no-node.yaml` with the details of the bot(s) you want to run.
As this step is the same in each setup, I've created a dedicated page on how to do that: [How to configure a BOT](./CONFIGURING_A_BOT.md).

Now that you've configured the BOT(s), let's see how to further configure your stack and how to operate it

### Fine grained configuration

This stack requires to save the Kupo db on disk.

The default configuration will create a `kupo-data` as subfolder of the project folder.

You can change where these data are persisted by setting the following environmental variable:

* `KUPO_DATA_FOLDER` to configure where the Kupo db will be persisted

You can provide the environment variable either from the command line, or in your env file. My preference is in the env file.
This way you don't have to remember to pass such data every single time, but it's all in one place.

If for example you want to persist everything in a `/data` folder, you could create an env file as follows

```bash
WALLET_MNEMONIC=aaa bbb ... zzz

KUPO_DATA_FOLDER=/data/kupo-data
```

### Starting, stopping and upgrading your stack

Now that everything is ready, how can we quickly start, stop and upgrade our stack? Luckily `docker compose` makes this
easy enough.

**In order to start your stack** you can cd into the root of the project and issue:

```bash
docker compose --env-file _name-of-env-file_ -f docker-compose-no-node.yaml up -d
```

**Stopping the stack** is really easy, again, cd into the root of the project and issue:

```bash
docker compose --env-file _name-of-env-file_ -f docker-compose-no-node.yaml down
```

It is also possible to upgrade the stack on the fly by:

```bash
docker compose -f docker-compose-no-node.yaml build # Used to rebuild all locally built images
docker compose -f docker-compose-no-node.yaml pull # To pull latest version for all downloaded images
docker compose --env-file _name-of-env-file_ -f docker-compose-no-node.yaml up -d # That will restart container that have either a new image or different configuration
```

### Gotchas

Using an external cardano node could be tricky, the beauty of the all-inclusive recipes is that you can define dependencies,
and only start services after they're ready. With this recipe, docker compose is not able to understand the health status of the Cardano node
and will attempt to run Ogmios and Kupo immediately, regardless of whether the `node.socket` is actually available signalling
the Cardano Node to be ready.
