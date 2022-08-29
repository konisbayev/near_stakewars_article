# Stake Wars

#### The first thing we need to do is install a wallet:

https://wallet.shardnet.near.org/

After installation, you need to configure NEAR-CLIP (NEAR-CLI is a command line interface. All interaction with the NEAR blockchain takes place through it, the work is carried out through RPC:

_Important! For security reasons, it is not recommended to install NEAR-CLIP and node on the same device, as well as to store access keys on the same device on which the node is located._

First you should check if Linux has been updated to the latest version:

```sudo apt update && sudo apt upgrade -y```

![](https://miro.medium.com/max/1400/1*lteODR4ZE_VqWTm2QnswEg.png)

Then install the Node development tools.js and npm:

``` curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install build-essential nodejs
PATH="$PATH"
```
![](https://miro.medium.com/max/1400/1*vekszSbWS_KYF3_93sWFIQ.png)

![](https://miro.medium.com/max/1400/1*TFCm5Y9VWCQkHzdgxWyebw.png)

Next, check the Node versions.js and npm:

``` node -v && npm -v ```

![](https://miro.medium.com/max/1164/1*CrlLfhnxX0UM7KqzCu3-Mg.png)


![](https://miro.medium.com/max/1400/1*PuiM6XJqUxBtGS6Wcx3R6g.png)
![](https://miro.medium.com/max/1400/1*LyUHMyirYwUC5A6Wh6JJvA.png)

Then you should install the NEAR-CLI itself:

To work , we will need a NEAR CLI repository from Github . we take it from the following link: https://github.com/near/near-cli . Log in with root rights, if this is not possible, then use the sudo command to install NEAR-CLIP, then check that the near binary file is preserved /usr/local/bin

``` sudo npm install -g near-cli ```

![](https://miro.medium.com/max/1400/1*8wtvgHXR8YLTkGvjyHfCNw.png)

### Validator Statistics

After installing NEAR-CLIP, we need to run some tests. To do this, we need the commands that we will analyze below. We can use the same commands to view validator statistics. There are three reports that are commonly used to monitor the status of the validator:

### Environment
Environment is configured every time a new shell is launched, thanks to this setting, the correct network is selected

Networks available to us:
   - GuildNet
   - TestNet
  -  MainNet
   - Shardnet (Network for Stake Wars)

command: ``` export NEAR_ENV=shardnet ```

The command below can be executed to install a permanent Near testnet environment:
```
echo 'export NEAR_ENV=shardnet' >> ~/.bashrc
echo 'export NEAR_ENV=shardnet' >> ~/.bash_profile
source $HOME/.bash_profile
```
Let’s delve into the NEAR-CLI commands:
- Proposals
The offer informs that the validator wants to enter the validator network. In order for the proposal to be accepted, it must meet the minimum requirements.
command:
``` near proposals ```

- Validators Current
Calls a list that displays active validators in the current era, the number of blocks produced, the number of expected blocks and their speed. Using this command, we check for problems with the validator.
command: 
```near validators current```

- Validators Next
Shows validators whose offer was accepted one epoch ago, they will be included in the set of validators in the next epoch.
command: 
```near validators next```

### The next step is to install our node:

##### Important! Server specifications must meet the minimum requirements

| processor | 4-core processor with AVX support |
| RAM | 8 GB DDR4 |
| 500 GB SSD Solid-state drive|

#### *IMPORTANT:
Before starting, check that the computer has the correct CPU functions.

```
lscpu | grep -P '(?=.*avx )(?=.*sse4.2 )(?=.*cx16 )(?=.*popcnt )' > /dev/null \
  && echo "Supported" \
  || echo "Not supported" 
```
![](https://miro.medium.com/max/1400/1*dPlcGmZYrZs0xMFqlnNEDw.png)

Go to the settings of dev. tools:

```
sudo apt install -y git binutils-dev libcurl4-openssl-dev zlib1g-dev libdw-dev libiberty-dev cmake gcc g++ python docker.io protobuf-compiler libssl-dev pkg-config clang llvm cargo
```
![](https://miro.medium.com/max/1400/1*EmQG0TnJPHl8xvKl8tHKpw.png)

If you have problems installing python or docker.io on Ubuntu, the command should help:
```
sudo apt install python3
sudo apt install docker-ce
```

Now let’s analyze the installation in more detail:

   - Installing Python: 
   ``` sudo apt install python3-pip ```
    - Setting the configuration: 
    ``` USER_BASE_BIN=$(python3 -m site --user-base)/bin ```
   ``` export PATH="$USER_BASE_BIN:$PATH" ```
    - Installing Building 
   ``` env:sudo apt install clang build-essential make ```
    - Installing Rust & Cargo: 
   ``` curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh ```

After all manipulations, press “1” and “Enter”

To check the source of the environment, use: ```source $HOME/.cargo/env```

![](https://miro.medium.com/max/1400/1*81rgzkzIWDJZ65AaEQ2Xhg.png)
![](https://miro.medium.com/max/1400/1*KTFcJXi24uOUQ5EI3vs-Vw.png)
![](https://miro.medium.com/max/1400/1*izSzomfOqKk0FZ0faVXQpQ.png)

Now you need to clone the nearcore project from GitHub. For this:
Clone the repository nearcore :

```
git clone https://github.com/near/nearcore
cd nearcore
git fetch 
```
![](https://miro.medium.com/max/1400/1*wOfs7Iri6a87g36gG5IQ4g.png)

Check the commit: ``` git checkout <commit> ```

Compile the binary file nearcore:
``` cargo build -p neard --release --features shardnet ```

![](https://miro.medium.com/max/1400/1*8ejhYtAyZDbHZeNfXNR3FA.png)

The next step is to install the working directory: ``` ./target/release/neard --home ~/.near init --chain-id shardnet --download-genesis ```

![](https://miro.medium.com/max/1400/1*P1IxxUKrRgAKyvRZHF5Y4w.png)

This command will create a directory structure and generate it in the newly created network:
- config.json — A set of configurations that respond to the operation of the node. Config.json carries all the necessary information for the node to work in the network, Ways of interacting with peers, reaching consensus. Some options are configurable, but validators prefer to use the config file.json by default.
- genesis.json — A file with a large amount of data that the network started with during genesis. The file contains initial accounts, contracts, access keys and other records that display the initial state of the blockchain. The genesis file.json is essentially a snapshot of the network at a certain point in time. The contacts contain accounts, balances, active validators and other information about the network.
- node_key.json —An important file that stores the public and private key of the note. It also carries an optional parameter that is necessary to start the validator node (we don’t touch it here).account_id
- data/ — The folder in which the node will not record its state.

Replacing config.json
```
rm ~/.near/config.json
wget -O ~/.near/config.json https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/config.json
```
![](https://miro.medium.com/max/1400/1*P1IxxUKrRgAKyvRZHF5Y4w.png)

Next, I install the last snapshot, for this you need:
Installing the AWS command line interface: ``` sudo apt-get install awscli -y ```
 ploading a snapshot (genesis.json):
```
cd ~/.near
wget https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/genesis.json
```
Run the node with the following command:
```
cd ~/nearcore
./target/release/neard — home ~/.near run
```
![](https://miro.medium.com/max/1400/1*pe5wM2U6FPcNOG-T6-r1ag.png)


### Activate validator pull
Authorizing the wallet:

Install the full access key locally, this will allow you to sign transactions via NEAR-CLI

The command to install: near login To do it, you need to:

![](https://miro.medium.com/max/1400/1*bi5Sr22WqtajvnG_eSgF1A.png)

1. Copy the link to the browser
2. Grant access to NEAR CLI
3. After granting access, they will return to the console
4. Enter your wallet and press enter

The next step is ``` validator_key.json ```. To do this, you need the command: ``` cat ~/.near/validator_key.json ```

Launch the node: ``` target/release/neard run ```

But for this you need to do a few more manipulations: 
``` sudo vi /etc/systemd/system/neard.service ```
```
[Unit]
Description=NEARd Daemon Service[Service]
Type=simple
User=<USER>
#Group=near
WorkingDirectory=/home/<USER>/.near
ExecStart=/home/<USER>/nearcore/target/release/neard run
Restart=on-failure
RestartSec=30
KillSignal=SIGINT
TimeoutStopSec=45
KillMode=mixed[Install]
WantedBy=multi-user.target
```
![](https://miro.medium.com/max/1400/1*fdpi95JHzPfbOJQLkGH2KQ.png)
![](https://miro.medium.com/max/1400/1*DmIp2GYvcmU05I1d5CCNFQ.png)
The following command: sudo systemctl enable neard
Then ``` sudo systemctl start neard ```
![](https://miro.medium.com/max/1400/1*naDYIwGKTaWm08XMbjXC5w.png)

If an error appears, you need to restart the server:sudo systemctl reload neard
Looking at the logs: ``` journalctl -n 100 -f -u neard ```
To output logs in a beautiful font: ``` sudo apt install ccze ```
Color font of logs: journalctl ``` -n 100 -f -u neard | ccze -A ```

![](https://miro.medium.com/max/1400/1*V8Q7esBCu1RNXuyuFz6Wrw.png)

If in the validator status we see. “Validator”, this means that the pool is selected in the current set of validators.

### Editing a stacking pool:

Installing a stacking pool contract:

It is necessary to call the factory stacking pool, create a new stacking pool with the specified name and deploy it in the specified AccountId.
```
near call factory.shardnet.near create_staking_pool '{"staking_pool_id": "<pool name>", "owner_id": "<accountId>", "stake_public_key": "<public key>", "reward_fee_fraction": {"numerator": 5, "denominator": 100}, "code_hash":"DD428g9eqLL8fWUxv8QSpVFzyHi1Qd16P8ephYCTmMSZ"}' --accountId="<accountId>" --amount=30 --gas=300000000000000
```
![](https://miro.medium.com/max/1400/1*-c8TYRQUlqJmXNrbbgyJQw.png)

Then ping our pull: 
![]( https://miro.medium.com/max/1400/1*ibPe98f-vYuMoVKgmUGrTw.png)
Check what is written at the end, if “True”, then the pool has been created successfully.

Stake near into pull follow command:
``` near call <pool_id> deposit_and_stake --amount <amount> --accountId <accountId> --gas=300000000000000 ```

Creating monitoring and alerts:

We can set up an alert via email, as a rule, this greatly facilitates the work with transactions and their signing. To do this , you need:

    Files with logs that are stored in the ~/.nearup/logs directory, or in systemd. To do this, use the command: journalctl -n 100 -f -u neard | ccze -A
    RPC: sudo apt install curl jq

Common commands:
- Checking the node version: 
```
curl -s http://127.0.0.1:3030/status | jq .version
- Checking delegates and staking: near view <your pool>.factory.shardnet.near get_accounts '{"from_index": 0, "limit": 10}' --accountId <accountId>.shardnet.near 
```
- Checking the reason for the validator failure: 
```
curl -s -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' 127.0.0.1:3030 | jq -c '.result.prev_epoch_kickout[] | select(.account_id | contains ("<POOL_ID>"))' | jq .reason 
```
- Checking the produced blocks / Expected number of blocks: 
``` 
curl -r -s -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' 127.0.0.1:3030 | jq -c '.result.current_validators[] | select(.account_id | contains ("POOL_ID"))' 
```


