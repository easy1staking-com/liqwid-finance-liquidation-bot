# liqwid-finance-liquidation-bot

Hey this is Giovanni, SPO of the EASY1 Stake Pool and Serial Liquidator on the Liqwid Finance platform.
**If you like what you're seeing here, please consider supporting me delegating to my Stake Pool: EASY1.**

You can find out more about the EASY1 Stake Pool [here](https://easy1staking.com).

This project contains a bunch of docker-compose recipes that I've improved over the weeks that will allow you
to quickly get up to speed and run your own Liquidation BOT. Depending on your technical abilities, you can run more or less
complex setup and compete for a chance to liquidate bad loans on Liqwid Finance and earn a few bucks.

Before proceeding though, **PLEASE** carefully read the disclaimer below.

### DISCLAIMER

Misuse of code and examples contained in this repository may lead to **irrecoverable loss of funds** or
leakage of private keys with subsequent loss of funds. The author assumes no responsibility in case of loss of funds.

## Official Liqwid Finance Liquidation BOT page

The original github page of the Liqwid Liquidation Bot is available [here](https://github.com/Liqwid-Labs/liqwid-liquidation-bot)

## Before you start

It is necessary to clearly state who this project is for. 
In order to follow these tutorials and setup your own bot as well as customising these examples for your own bot, it is necessary
to be proficient with docker and containerised technologies. It's not the goal of this tutorial to teach docker, but rather
provide a set of recipes to get you up and running quickly with a Liqwid Finance bot.

Examples will go from a more basic configuration to a fairly complex setup where you will run your own mainnet cardano-node, ogmios, 
in-memory kupo and all the stuff you need to run a fast, reliable BOT and keep Liqwid Finance bad-debt free. And why not, earn a 
couple of bucks too.

If you're not comfortable with docker and co., close the window and go for a walk, don't come to me crying coz you lost 10k ada or 20k djed.

## Docker compose gotchas

Just a list of random article about docker gotchas, coz there's always something to learn, and also because docker-compose sucks hard.

`docker-compose up` does not rebuild images even if project's content has changed. Further info https://github.com/docker/compose/issues/1487

## About the Author

Hi! I am Giovanni Gargiulo and I am a Principal Software Engineer with 20 years of commercial experience. I'm a DevOps advocate and in
recent years I've worked on Machine Learning Projects. I'm a Cardano SPO since October 2020 and a full time Cardano dev since 2021.
