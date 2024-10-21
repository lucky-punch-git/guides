# Airchains installation
To set up an Airchains validator node, follow these steps. I’ve condensed the process from the available resources:

### 1. System Requirements
Before starting, ensure your system meets the recommended specifications:
- **OS**: Linux (Ubuntu 20.04 recommended)
- **RAM**: 16GB or higher
- **Storage**: SSD with at least 500GB of space
- **Processor**: 4 or more high-performance cores
- **Network**: Fast and reliable internet connection

### 2. Install Dependencies
You will need to install some basic dependencies like `wget`, `git`, and Go (v1.20+).
```bash
sudo apt-get update
sudo apt-get install wget git
```

### 3. Download and Install `junctiond`
This is the main binary for Airchains.
```bash
wget https://github.com/airchains-network/junction/releases/download/v0.1.0/junctiond
chmod +x junctiond
sudo mv junctiond /usr/local/bin/
```

### 4. Initialize the Node
Set a moniker (name) for your node.
```bash
junctiond init <moniker>
```

### 5. Configure the Genesis File
Download the latest genesis file for the Airchains testnet.
```bash
wget https://github.com/airchains-network/junction/releases/download/v0.1.0/genesis.json
cp genesis.json ~/.junction/config/genesis.json
```

### 6. Update Configuration
Modify the `config.toml` file to set persistent peers and ensure proper connectivity.
```bash
nano ~/.junction/config/config.toml
```
In the `persistent_peers` section, add:
```toml
persistent_peers = "de2e7251667dee5de5eed98e54a58749fadd23d8@34.22.237.85:26656"
```

### 7. Set Minimum Gas Prices
Edit the `app.toml` file to set the gas prices.
```bash
nano ~/.junction/config/app.toml
```
Add or modify the following line:
```toml
minimum-gas-prices = "0.00025amf"
```

### 8. Start the Node
Now you can start your node and allow it to sync with the blockchain.
```bash
junctiond start
```
You can check the sync status with:
```bash
junctiond status
```

### 9. Create a Validator Wallet
Create a new wallet for your validator.
```bash
junctiond keys add <validator-name>
```
Save the mnemonic securely.

### 10. Fund Your Account
You need to fund your account with at least 58 tokens. Testnet tokens can be obtained via the Airchains Discord faucet.

### 11. Create the Validator
Once your node is synced, you can create a validator. First, get the public key:
```bash
junctiond comet show-validator
```
Then, create the `validator.json` with the required details and use the following command to become a validator:
```bash
junctiond tx staking create-validator --from <keyname> --pubkey <pubkey> --chain-id junction --fees 500amf
```

For more detailed steps and troubleshooting, refer to the [official documentation](https://docs.airchains.io)】.
