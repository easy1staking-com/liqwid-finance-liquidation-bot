# liqwid-finance-liquidation-bot

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

The liquidation BOT is a standalone app distributed by the Liqwid Finance team as a Docker image.

You need to run as many container as the market you want to liquidate. So if for instance you have a bunch of ada and djed
you want to use to liquidate bad loans, you need two bots running.

At the time of writing, Liquidation BOTs require to talk to two service:
* [kupo](https://cardanosolutions.github.io/kupo/)
* [ogmios](https://ogmios.dev/)

While the Liqwid Finance team makes such services available, it is recommended for you to run your own instances, as it 
will greatly improve BOT performances and transaction building and give you a good chance be the first to liquidate a bad loan.
While very large loans can be processed by multiple transactions issued by different BOT operators, for medium and small loans 
it's race to the first that is able to submit a valid transaction.

Ideally, you will need to run the full set of services locally so to maximise your chances to be first. List of services is:
* Cardano Node
* Ogmios
* Kupo
* Number N of Bots depending on the markets you want to cover

In order to get as many people up to speed, I've prepared different charts that offer different type of complexity, so that
also the less experienced people can give it a go.

Charts and their docs, from most complex to simples:
1. [Full stack with external node](./FULL_STACK_EXTERNAL_NODE.md)
2. [Full stack with in memory kupo](./FULL_STACK_IN_MEMORY_KUPO.md)
3. [Full stack](./FULL_STACK.md)

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
