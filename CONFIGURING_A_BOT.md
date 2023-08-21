# Index

- [Configuring a BOT](#configuring-a-bot)
    * [One stack, one bot for the ADA Market](#one-stack-one-bot-for-the-ada-market)
    * [One stack, two BOTs, Ada and Djed](#one-stack-two-bots-ada-and-djed)
        + [NOTE](#note)

# Configuring a BOT

In this paragraph we will learn how to configure the actual BOT(s). The goal of this project is to simplify as much
as possible the configuration and management of both the BOTs and the Cardano services required to run the BOTs. 

For this reason I've provided convenient default values wherever possible.

The desired outcome would be for you, the end user, to just

1. copy and paste the bot section for any of the provided recipes, as many times as the number of bots you want to run
2. assign a name to such bots
3. simply rename and configure the `WALLET_MNEMONIC_ADA` environment variable for the specific bot. And job done!

The best way of explaining how to configure one or more bots, is via examples

## One stack, one bot for the ADA Market

Let's start with a simple example where we're running any of the provided recipes, with just one BOT operating the ADA Market.

```yaml
  ada-bot:
    container_name: ada-bot
    image: ghcr.io/liqwid-labs/liqwid-liquidation-bot:main
    restart: always
    environment:
      - WALLET_MNEMONIC=${WALLET_MNEMONIC_ADA:-}
      - MARKET=Ada
      - HEARTBEAT_ADDR=127.0.0.1
      - HEARTBEAT_PORT=2002
      - VERBOSE=${VERBOSE:-false}
      - LOGGER=${LOGGER:-NoColourLogger}

      - OGMIOS_HOST=${OGMIOS_HOST:-ogmios}
      - OGMIOS_PORT=${OGMIOS_PORT:-1337}
      - OGMIOS_SECURE=${OGMIOS_SECURE:-false}
      - OGMIOS_PATH=${OGMIOS_PATH:-}

      - KUPO_HOST=${KUPO_HOST:-kupo}
      - KUPO_PORT=${KUPO_PORT:-1442}
      - KUPO_SECURE=${KUPO_SECURE:-false}
      - KUPO_PATH=${KUPO_PATH:-}

      - INTERVAL=${INTERVAL:-5}
      - BUFFER=${BUFFER:-20000000}
      - CHECK_PROFIT=${CHECK_PROFIT:-true}
      - PROFIT_AMOUNT=${PROFIT_AMOUNT:-5}
      - ENABLE_REDEEMS=${ENABLE_REDEEMS:-true}
      - LIQWID_CONFIG=liqwid-mainnet-config.json
```

Things to notice are:
* Service Name: in this case is `ada-bot` (first line of the snippet above), this must be unique, and it would make sense 
to give it a name that would remind what his goal is. If it was operating on the djed market I would recommend to call it `djed-bot` and so on.
If you had two ada bot, I would recommend to specify the difference, maybe a large wallet: `ada-bot-large`.
* Container Name: I would recommend this to match the service name as it is the name displayed when getting the list of running
containers and can obtained with `docker ps`
* Wallet Mnemonic: is the name of the environment variable contained in the env file and passed to docker compose. 
While the name of the env var  does not require to be unique, it's recommended to have one mnemonic per bot, hence they must have different name
* Buffer: How much of the token (in this case Ada) you want to leave in the wallet and not use to repay the loan. This can't be zero for Ada market
as otherwise you won't be able to pay following transactions. 20 Ada is a good enough value. Don't forget that the values is in lovelaces.
For other markets, it can be zero.

That's it. This is the bare minimum that you need to configure properly in order to run a bot. 

Let's now see an example of an env var file that can be passed to the docker compose.

Let's call the file `ada-only-env`, and set its content to:

```bash
WALLET_MNEMONIC_ADA=never gonna give you up ... never gonna let you down 
```

The env file name needs to be used and passed to the docker compose file. Eg.

`docker compose --env-file ada-only-env -f docker-compose-full.yaml up -d`

An env var file as simple as this one would do the job for this specific example.

## One stack, two BOTs, Ada and Djed

Let's complicate things a bit. One stack, two bots: ada and djed. As before, also in this case, this example can be applied to
any of the docker-compose files.

```yaml
  ada-bot:
    container_name: ada-bot
    image: ghcr.io/liqwid-labs/liqwid-liquidation-bot:main
    restart: always
    environment:
      - WALLET_MNEMONIC=${WALLET_MNEMONIC_ADA:-}
      - MARKET=Ada
      - HEARTBEAT_ADDR=127.0.0.1
      - HEARTBEAT_PORT=2002
      - VERBOSE=${VERBOSE:-false}
      - LOGGER=${LOGGER:-NoColourLogger}

      - OGMIOS_HOST=${OGMIOS_HOST:-ogmios}
      - OGMIOS_PORT=${OGMIOS_PORT:-1337}
      - OGMIOS_SECURE=${OGMIOS_SECURE:-false}
      - OGMIOS_PATH=${OGMIOS_PATH:-}

      - KUPO_HOST=${KUPO_HOST:-kupo}
      - KUPO_PORT=${KUPO_PORT:-1442}
      - KUPO_SECURE=${KUPO_SECURE:-false}
      - KUPO_PATH=${KUPO_PATH:-}

      - INTERVAL=${INTERVAL:-5}
      - BUFFER=${BUFFER:-20000000}
      - CHECK_PROFIT=${CHECK_PROFIT:-true}
      - PROFIT_AMOUNT=${PROFIT_AMOUNT:-5}
      - ENABLE_REDEEMS=${ENABLE_REDEEMS:-true}
      - LIQWID_CONFIG=liqwid-mainnet-config.json  
  djed-bot:
    container_name: djed-bot
    image: ghcr.io/liqwid-labs/liqwid-liquidation-bot:main
    restart: always
    environment:
      - WALLET_MNEMONIC=${WALLET_MNEMONIC_DJED:-}
      - MARKET=DJED
      - HEARTBEAT_ADDR=127.0.0.1
      - HEARTBEAT_PORT=2002
      - VERBOSE=${VERBOSE:-false}
      - LOGGER=${LOGGER:-NoColourLogger}

      - OGMIOS_HOST=${OGMIOS_HOST:-ogmios}
      - OGMIOS_PORT=${OGMIOS_PORT:-1337}
      - OGMIOS_SECURE=${OGMIOS_SECURE:-false}
      - OGMIOS_PATH=${OGMIOS_PATH:-}

      - KUPO_HOST=${KUPO_HOST:-kupo}
      - KUPO_PORT=${KUPO_PORT:-1442}
      - KUPO_SECURE=${KUPO_SECURE:-false}
      - KUPO_PATH=${KUPO_PATH:-}

      - INTERVAL=${INTERVAL:-5}
      - BUFFER=${BUFFER:-0}
      - CHECK_PROFIT=${CHECK_PROFIT:-true}
      - PROFIT_AMOUNT=${PROFIT_AMOUNT:-5}
      - ENABLE_REDEEMS=${ENABLE_REDEEMS:-true}
      - LIQWID_CONFIG=liqwid-mainnet-config.json
```

And the env file maybe called `ada-djed-env` 

```bash
WALLET_MNEMONIC_ADA=never gonna give you up ... never gonna let you down 
WALLET_MNEMONIC_DJED=will walk one hundred miles ... more 
```

Again, the env file name needs to be used and passed to the docker compose file. Eg.

`docker compose --env-file ada-djed-env -f docker-compose-full.yaml up -d`

### NOTE

For more details about all the BOT parameters, it is recommended to check the documentation available at 
official [Liqwid Finance BOT Project](https://github.com/Liqwid-Labs/liqwid-liquidation-bot)
