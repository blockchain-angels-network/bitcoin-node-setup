
# Bitcoin Node setup

## Faucet bitcoin testnet3
[<https://coinfaucet.eu/en/btc-testnet/>](https://coinfaucet.eu/en/btc-testnet/)

## General
The easy way to run bitcoin fullnode is running bitcoin application( bitcoind in console, bitcoinQT in GUI). There are 3 running modes of Bitcoin
* `Bitcoin Mainnet` : running bitcoin fullnode with real environment-real bitcoin blockchain network.The bitcoin on this network is worthy.
* `Bitcoin Testnet` : For testing purpose, people create bitcoin testnet( the current is bitcoin `testnet3`).The behaviour of your applications in this network is nearly the same with mainnet. Usually, the application should be tested on testnet before launching to mainnet. Testnet coins are separate and distinct from actual bitcoins and worthless, but you have to ask them the free coins for testing through faucets [<https://coinfaucet.eu/en/btc-testnet/>](https://coinfaucet.eu/en/btc-testnet/)

* `Bitcoin Regtest` in this mode people fully control the bitcoin network, people can generate block without doing hardwork to mine new block. This mode is usefull for local testing because you can have your own alot of bitcoin to test application as a reward through mining newblock.( the difficulty of this system is so easy). Testnet coins are separate and distinct from actual bitcoins and Testnet coin and also worthless


## Bitcoin regtest node

### For Linux

* Download 

```Sh

mkdir -p ~/BitcoinApp
cd ~/BitcoinApp
wget https://bitcoin.org/bin/bitcoin-core-0.16.3/bitcoin-0.16.3-x86_64-linux-gnu.tar.gz
tar xvfz bitcoin-0.16.3-x86_64-linux-gnu.tar.gz
# the bitcoin execution binaries are extracted in ~/BitcoinApp/bitcoin-0.16.3/bin
```

* Optional, if you want to directly run bitcoind without their path information, run this cmd

```Sh 
sudo cp -r ./bitcoin-0.16.3/ /usr/local
```

* Create the configuration file `~/BitcoinApp/bitcoin.conf`

```Sh
#bitcoin.conf file
regtest=1
#testnet=1
#the p2p proto port
port=9003
# remote http req user
rpcuser=user
# remote http req pass
rpcpassword=pass
# remote http req port listen
rpcport=9004
# remote http req  listen on all interfaces
rpcbind=0.0.0.0
# remote http req accept all ip
rpcallowip=::/0
server=1

discover=0
txindex=1
rpcssl=0
# To add more node here using addnode=<ip or link>:port
#addnode=55.12.51.12:9002
#addnode=127.0.0.1:18003

```

* Start bitcoind as application

```Sh
cd ~/BitcoinApp
mkdir -p chaindata
#$PWD/bitcoin-0.16.3/bin/bitcoind -conf=<path to bitcoin.conf> -datadir=<folder path to sore blockchain data> -debug=1 -printtoconsole 
$PWD/bitcoin-0.16.3/bin/bitcoind -conf="$PWD/bitcoin.conf" -datadir="$PWD/chaindata" -debug=1 -printtoconsole
```
* If you want to start bitcoind as system service, add ` -daemon` on the bitcoind cmd below

```Sh
cd ~/BitcoinApp
mkdir -p chaindata
$PWD/bitcoin-0.16.3/bin/bitcoind -conf="$PWD/bitcoin.conf" -datadir="$PWD/chaindata" -debug=1 -printtoconsole -daemon
```

* Start cli to interact with running bitcoind above in another terminal session and run `bitcoin-cli -conf="path to bitcoin.conf" cmd_name [cmd paras]`. More detais about the cli cmds can be found in [Original Bitcoin client/API calls list - Bitcoin Wiki](https://en.bitcoin.it/wiki/Original_Bitcoin_client/API_calls_list).Here are the examples to run some cmds.


```Sh
cd ~/BitcoinApp

#to get blockchain info
$PWD/bitcoin-0.16.3/bin/bitcoin-cli -conf="$PWD/bitcoin.conf" getblockchaininfo

#to get network info
$PWD/bitcoin-0.16.3/bin/bitcoin-cli -conf="$PWD/bitcoin.conf" getnetworkinfo

#to check how many peers connect to this node
$PWD/bitcoin-0.16.3/bin/bitcoin-cli -conf="$PWD/bitcoin.conf" getpeerinfo

#to check how many blocks on blockchain
$PWD/bitcoin-0.16.3/bin/bitcoin-cli -conf="$PWD/bitcoin.conf" getblockcount

#to create new addr
$PWD/bitcoin-0.16.3/bin/bitcoin-cli -conf="$PWD/bitcoin.conf" getnewaddress

#to mine new block ( this cmd 'only available in regtest mode'.)

$PWD/bitcoin-0.16.3/bin/bitcoin-cli -conf="$PWD/bitcoin.conf" generate <number of blocks to generate>

#or mine new blocks and give reward coin (block reward) to specific address  

$PWD/bitcoin-0.16.3/bin/bitcoin-cli -conf="$PWD/bitcoin.conf" generatetoaddress <number of blocks> "<addr to recv the reward>"

```

### For MacOS

Bitcoin support bitcoinQT application to run on Macos. This app include the bitcoin fullnode( like bitcond) and also support GUI to let user interract with bitcoin blockchain
* Download
```Sh
mkdir -p ~/BitcoinApp
cd ~/BitcoinApp
wget https://bitcoin.org/bin/bitcoin-core-0.16.3/bitcoin-0.16.3-osx.dmg

```
* Install binaries
Open bitcoin-0.16.3-osx.dmg but do not drag and drop `Bitcoin core` icon to `Applications` as normal
Right click on `Bitcoin core` icon to show off context menu --> copy then paste it into `~/BitcoinApp`

* Create bitcoin config file `~/BitcoinApp/bitcoin.conf` the same with the Linux's one
```Sh
#bitcoin.conf file
regtest=1
#testnet=1
#the p2p proto port
port=9003
# remote http req user
rpcuser=user
# remote http req pass
rpcpassword=pass
# remote http req port listen
rpcport=9004
# remote http req  listen on all interfaces
rpcbind=0.0.0.0
# remote http req accept all ip
rpcallowip=::/0
server=1

discover=0
txindex=1
rpcssl=0
# To add more node here using addnode=<ip or link>:port
#addnode=55.12.51.12:9002
#addnode=127.0.0.1:18003
```
* Run bitcoinQT app

```Sh
cd ~/BitcoinApp
mkdir -p blockchain
./Bitcoin-Qt.app/Contents/MacOS/Bitcoin-Qt -conf="${PWD}/bitcoin.conf" -datadir="${PWD}/blockchain" -debug=1 -printtoconsole
```
* To run CLI console: after Bitcoin core GUI start, access menu `Help-->Debug Window--> Console` to show off console window and run bitcoin cli cmd. You can run some commands in inputbox to test from this window
*  More detais in [Original Bitcoin client/API calls list - Bitcoin Wiki](https://en.bitcoin.it/wiki/Original_Bitcoin_client/API_calls_list)





