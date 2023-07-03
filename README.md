# liqwid-finance-liquidation-bot

### DISCLAIMER

Misuse of code and examples contained in this project may lead to irrecoverable loss of funds or
disclosure of private keys with subsequent loss of funds. The author assumes no responsibility in case of loss of funds.

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
