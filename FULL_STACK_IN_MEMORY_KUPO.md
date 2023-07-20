# Index

# Full Stack Setup with in-memory Kupo

In this tutorial we will see how to run one or more Liqwid Finance bots along with all the required services. The great
thing about this setup is that you won't have any external dependencies, making your BOT really fast in fetching
critical data. 

The peculiarity of this setup is that Kupo will load in-memory. This has pros and cons.

**Pros**
* Once Kupo has synced with tip, it will be super super fast

**Cons**
* Every time Kupo restarts, it takes between 2 and 3 hours to sync with tip. You won't be able to liquidate anything during this time
because relevant utxos won't be in the Kupo database.

The services you will run with this setup are:

1. Cardano Node
2. Ogmios
3. Kupo (in memory)
4. Your bot(s)

## Hardware Requirements

This setup is mostly memory intensive. Here below a rough estimate of the hardware requirements

| Service      | CPU | Memory | Disk   |
|--------------|-----|--------|--------|
| Cardano Node | 1   | 16 GB  | 150 GB |
| Ogmios       | 0.5 | 256 MB | N/A    |
| Kupo         | 0.5 | 5 GB   | N/A    |
| 1 x Bot      | 0.5 | 256 MB | N/A    |

As you can see 4 cores and 24 GB should do the trick. From a DISK PoV 200 GB will be perfect.
Remember that if you are running more than one BOT, you need to account for another half core and 256 MB of ram.

## Managing the stack

The docker compose file you will work with is the `docker-compose-full-in-memory-kupo.yaml`.

The fist step is to customise the `docker-compose-full-in-memory-kupo.yaml` with the details of the bot(s) you want to run.
As this step is the same in each setup, I've created a dedicated page on how to do that: [How to configure a BOT](./CONFIGURING_A_BOT.md).

Now that you've configured the BOT(s), let's see how to further configure your stack and how to operate it

### Fine grained configuration

This stack requires to save the cardano blockchain on disk.

The default configuration will create the `cardano-node-data` folder as subfolder of the project folder.

You can change where these data are persisted by setting the following environmental variable:

* `CARDANO_NODE_DATA_FOLDER` to configure where the blockchain is persisted

You can provide the environment variable, either from the command line, or in your env file. My preference is in the env file.
This way you don't have to remember to pass such data every single time, but it's all in one place.

If for example you want to persist everything in a `/data` folder, you could create an env file as follows

```bash
WALLET_MNEMONIC=aaa bbb ... zzz

CARDANO_NODE_DATA_FOLDER=/data/cardano-node-data
```

### Starting, stopping and upgrading your stack

Now that everything is ready, how can we quickly start, stop and upgrade our stack? Luckily `docker compose` makes this
easy enough.

**In order to start your stack** you can cd into the root of the project and issue:

```bash
docker compose --env-file _name-of-env-file_ -f docker-compose-full-in-memory-kupo.yaml up -d
```

Bear in mind the first time you do this, it could take up to several minutes to complete. This happens because the `init` container
is downloading the blockchain from S3, rather than letting the node to sync naturally. This will greatly reduce the time you'll get up to speed.

**Stopping the stack** is really easy, again, cd into the root of the project and issue:

```bash
docker compose --env-file _name-of-env-file_ -f docker-compose-full-in-memory-kupo.yaml down
```

It is also possible to upgrade the stack on the fly by:

```bash
docker compose -f docker-compose-full-in-memory-kupo.yaml build # Used to rebuild all locally built images
docker compose -f docker-compose-full-in-memory-kupo.yaml pull # To pull latest version for all downloaded images
docker compose --env-file _name-of-env-file_ -f docker-compose-full-in-memory-kupo.yaml up -d # That will restart container that have either a new image or different configuration
```

Differently from the other setups, **I would recommend** to learn how to master the _upgrade on the fly_, as you want to restart 
Kupo as few times as possible as it will required 2 to 3 hours to sync to tip and during this time you won't be able to
perform any liquidations.
