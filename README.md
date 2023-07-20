# Table of Contents

- [Liqwid Finance BOT - Master Class](#liqwid-finance-bot-master-class)
    + [DISCLAIMER](#disclaimer)
    * [Official Liqwid Finance Liquidation BOT page](#official-liqwid-finance-liquidation-bot-page)
    * [Before you start](#before-you-start)
    * [Installing Docker](#installing-docker)
- [Running Liquidation BOTS](#running-liquidation-bots)
    * [BOTs' Wallets](#bots-wallets)
        + [Wallets, tips and tricks](#wallets-tips-and-tricks)
    * [Guide to run Liquidation BOTs](#guide-to-run-liquidation-bots)
    * [Docker compose gotchas](#docker-compose-gotchas)
    * [About the Author](#about-the-author)

# Liqwid Finance BOT - Master Class

Hey this is Giovanni, SPO of the EASY1 Stake Pool and Serial Liquidator on the Liqwid Finance platform.

This project contains a bunch of docker-compose recipes that I'm improving over the weeks and that will allow you
to quickly get up to speed and run your own Liquidation BOT.

Depending on your technical abilities, you can run more or less
complex setup and compete for a chance to liquidate bad loans on [Liqwid Finance](https://liqwid.finance) and earn a few bucks.

Before proceeding though, **PLEASE** carefully read the disclaimer below.

**If you like what you're seeing here, please consider supporting me by delegating to my Stake Pool: EASY1.
OR buy me a coffee by sending a few ada/djed to the $cryptojoe101 adahandle**

You can find out more about the EASY1 Stake Pool [here](https://easy1staking.com).

### DISCLAIMER

Misuse of code and examples contained in this repository may lead to **irrecoverable loss of funds** or
leakage of private keys with subsequent loss of funds. The author assumes no responsibility in case of loss of funds.

## Official Liqwid Finance Liquidation BOT page

First things first. 

[Liqwid Finance](https://liqwid.finance) is a decentralised lending and borrowing platform running on Cardano.

The team has handed over to the community the important task to liquidate bad debts and keep the platform financially healthy.
In order to ensure that bad loans are promptly liquidated, the team has provided the community with a BOT that peforms the liquidation and
with incentives for community members to run the BOT.

Yep you heard right. You can get paid to liquidate bad debts on the Liqwid Finance platform.

The original github page and documentation of the Liqwid Liquidation Bot is available [here](https://github.com/Liqwid-Labs/liqwid-liquidation-bot)

## Before you start

It is necessary to clearly state who this project is for. 

In order to follow these tutorials, and setup your own bot as well as customising these examples for your own bot, it is necessary
to be proficient with docker and containerised technologies. It's not the goal of this tutorial to teach docker, but rather
provide a set of recipes to get you up and running quickly with a Liqwid Finance bot.

Examples will go from a more basic configuration to a fairly complex setup where you will run your own mainnet cardano-node, ogmios, 
kupo and all the stuff you need to run a fast, reliable BOT and keep Liqwid Finance bad-debt free. And why not, earn a 
few bucks too.

If you're not comfortable with unix and or docker, close the window and go for a walk, don't come to me crying coz you lost a bunch of ada or djed.

## Installing Docker

The process of installing docker, closely depends on the Operative System you're using and even on linux there are several 
different ways of installing it.

As I run my BOTs on either Docker or Kubernetes, I like to use Ubuntu Linux as underlying OS for all my projects as it's very popular,
simple to use, and there are hundreds of tutorials and articles to solve most of the issues you might encounter. 
So, if you're starting from scratch on old hardware or renting a VPS, I would recommend ubuntu because you'll be able to follow the instructions
contained in this project all the way from installing and configuring components to running your BOTs. 

It's not the purpose of this tutorial to explain how to install and run docker, but in order to get you up to speed, you can 
find how to install docker on ubuntu linux in this quick tutorial [Installing Docker on Ubuntu](./INSTALLING_DOCKER.md).

# Running Liquidation BOTS

The liquidation BOT is a standalone app distributed by the Liqwid Finance team as a Docker image. A running container
will only be able to perform liquidations for a specific market. At the moment of writing there are four different markets:
Ada, DJED, IUSD, SHEN. 

You will need to run as many containers as the number of markets you want your bots to operate in. If for example you have
a spare bunch of ada and djed you want to use to liquidate bad loans, you need to run two bots, one for each market.

At the time of writing, Liquidation BOTs require to talk to two service:
* [kupo](https://cardanosolutions.github.io/kupo/)
* [ogmios](https://ogmios.dev/)

While the Liqwid Finance team makes such services available, it is recommended for you to run your own instances, as it 
will greatly improve BOT performances in fetching bad debts data and transaction building and give you a good chance be the 
first to liquidate a bad loan. While very large loans can be processed by multiple transactions issued by different BOT operators, 
for medium and small loans it's race to the first that is able to submit a valid transaction.

Ideally, you will need to run the full set of services locally so to maximise your chances to be first. List of services is:
* Cardano Node
* Ogmios
* Kupo
* Number N of Bots depending on the markets you want to cover

Before jumping into the actual guides, let's quickly speak about the wallets the BOTs will use.

## BOTs' Wallets

In order for your BOTs to execute liquidation transactions, it is necessary that the BOTs has enough ADA and, if you're 
operating a BOT for a non-ADA market, enough token for the market your BOT is operating into.

At the time of writing, only loans worth more than 100$ can be liquidated. This also means that you need at least 100$ worth
of the token of the market your BOT is operating, plus a few ADA to pay for collateral and transaction fees. A good rule of thumb 
would be to have between 20 and 30 ada spare plus the token you want to use to perform liquidations.

I would recommend to create new dedicated wallet for your BOTs. Ideally you should have one wallet for BOT (in order to 
avoid concurrency issue), but you could reuse the same wallet for markets that are usually not very coupled.
As an example I have a single wallet for the ADA and the IUSD market. DJED and IUSD should be for example kept on two 
different wallets as IUSD and DJED loans that have ADA as collateral, could become unhealthy at the same time and create
concurrency issue.

### Wallets, tips and tricks

BOTs can be configured to use either a wallet payment Signature KEY (aka the skey) or the wallet mnemonic. I strongly 
recommend the mnemonic, because you can restore such wallet in your favorite Cardano wallet like Nami, Eternl or Yoroi.

Should your BOT perform a liquidation, you also want be able to quickly and easily move funds around, as you might want to 
transfer or convert your earnings so to restore the wallet's balance. I want to remind you that if you liquidate an ADA loan 
backed by DJED, your BOT will repay the ADA and get the DJED. You will need to convert the DJED back into ADA to materialise
your earnings. This needs to be done kinda quickly or you might end up with a loss. Being able to access your funds quickly is 
of paramount importance. 

Don't be scared. My bots have performed several liquidations and I've never ended up with a loss. But in order to achieve this,
I had to setup a few things. And today I'm sharing them with you.

1. Setup your BOTs with a 24 words mnemonic
2. Restore the mnemonic in Eternl and set as the name of the wallet, the market it operates into. Eg. "Liqwid - Djed"
3. In the Eternl set the "Single Address Mode" to true, so that your BOT and Eternl will use exactly the same address, or
the BOT might not be able to access all the wallet funds 
4. (Optional) If you have telegram and want to be notified every time a BOT wallet sends or receive funds (or performs a
liquidation) install [Thoth-Bot](https://github.com/DevStakePool/thoth-bot) a Telegram plugin developed by Alessio, SPO of DEV Pool
5. Make yourself familiar on how to use MinSwap, SundaeSwap or your favourite Dex from the Eternl app on your phone.
6. If you operate a BOT for the Ada market, don't forget to stake your Ada! Obviously on the EASY1 Stake Pool. Right? RIGHT?
This way your Ada are both generating rewards and processing liquidations.

**NOTE:** It is not possible to use Ada on Hardware Wallets.

## Guide to run Liquidation BOTs

In order to get as many people up to speed, I've prepared different charts that offer different type of complexity, so that
also the less experienced people can give it a go.

Charts and their docs, from most complex to simples:
1. [Full stack with external node](./FULL_STACK_EXTERNAL_NODE.md)
2. [Full stack with in memory kupo](./FULL_STACK_IN_MEMORY_KUPO.md)
3. [Full stack](./FULL_STACK.md)
4. BOTs only (Work In Progress)

## Docker compose gotchas

Just a list of random article about docker gotchas, coz there's always something to learn, and also because docker-compose sucks hard.

`docker-compose up` does not rebuild images even if project's content has changed. Further info https://github.com/docker/compose/issues/1487

## About the Author

Hi! I am Giovanni Gargiulo! I am a Principal Software Engineer with 20 years of commercial experience. I'm a DevOps advocate and in
recent years I've worked on Machine Learning Projects. I'm a Cardano SPO since October 2020 and a full time Cardano dev since 2021.

You can find me:
* on twitter: https://twitter.com/CryptoJoe101
* linkedin https://www.linkedin.com/in/giovannigargiulo/
* discord: giovannieasy1
