# Index

- [Full Stack Setup](#full-stack-setup)
    * [Hardware Requirements](#hardware-requirements)
    * [Managing the stack](#managing-the-stack)
        + [Fine grained configuration](#fine-grained-configuration)
        + [Starting, stopping and upgrading your stack](#starting-stopping-and-upgrading-your-stack)

# Full Stack Setup

In this tutorial we will see how to run one or more Liqwid Finance bots along with all the required services. The great
thing about this setup is that you won't have any external dependencies, making your BOT really fast in fetching
critical
data.

The services you will run with this setup are:

1. Cardano Node
2. Ogmios
3. Kupo (on disk db)
4. Your bot(s)

## Hardware Requirements

This setup is mostly memory intensive. Here below a rough estimate of the hardware requirements

| Service      | CPU | Memory | Disk   |
|--------------|-----|--------|--------|
| Cardano Node | 1   | 16 GB  | 150 GB |
| Ogmios       | 0.5 | 256 MB | N/A    |
| Kupo         | 0.5 | 4 GB   | 5 GB   |
| 1 x Bot      | 0.5 | 256 MB | N/A    |

As you can see 4 cores and 24 GB should do the trick. From a DISK PoV 200 GB will be perfect. 
Remember that if you are running more than one BOT, you need to account for another half core and 256 MB of ram.

## Managing the stack

The docker compose file you will work with is the `docker-compose-full.yaml`.

The fist step is to customise the `docker-compose-full.yaml` with the details of the bot(s) you want to run. 
As this step is the same in each setup, I've created a dedicated page on how to do that: [How to configure a BOT](./CONFIGURING_A_BOT.md).

Now that you've configured the BOT(s), let's see how to further configure your stack and how to operate it

### Fine grained configuration

This stack requires to save data on disk. Both the Cardano Node and Kupo will respectively store the blockchain and the 
utxo data on disk.

The default configuration will create two folders: `cardano-node-data` and `kupo-data` as subfolder of the project folder.

You can change where these data are persisted by setting the following environmental variables:

* `CARDANO_NODE_DATA_FOLDER` to configure where the blockchain is persisted
* `KUPO_DATA_FOLDER` to configure where the Kupo db will be persisted

You can provide these environment variables either from the command line, or in your env file. My preference is in the env file.
This way you don't have to remember to pass such data every single time, but it's all in one place.

If for example you want to persist everything in a `/data` folder, you could create an env file as follows

```bash
WALLET_MNEMONIC=aaa bbb ... zzz

CARDANO_NODE_DATA_FOLDER=/data/cardano-node-data
KUPO_DATA_FOLDER=/data/kupo-data
```

### Starting, stopping and upgrading your stack

Now that everything is ready, how can we quickly start, stop and upgrade our stack? Luckily `docker compose` makes this
easy enough.

**In order to start your stack** you can cd into the root of the project and issue:

```bash
docker compose --env-file _name-of-env-file_ -f docker-compose-full.yaml up -d
```

Bear in mind the first time you do this, it could take up to several minutes to complete. This happens because the `init` container
is downloading the blockchain from S3, rather than letting the node to sync naturally. This will greatly reduce the time you'll get up to speed.

**Stopping the stack** is really easy, again, cd into the root of the project and issue:

```bash
docker compose --env-file _name-of-env-file_ -f docker-compose-full.yaml down
```

It is also possible to upgrade the stack on the fly by:

```bash
docker compose -f docker-compose-full.yaml build # Used to rebuild all locally built images
docker compose -f docker-compose-full.yaml pull # To pull latest version for all downloaded images
docker compose --env-file _name-of-env-file_ -f docker-compose-full.yaml up -d # That will restart container that have either a new image or different configuration
```