# Seda protocol node installation
To set up a validator node for the SEDA protocol, here’s a detailed guide to help you get started.

### 1. **Install Prerequisites**
Ensure you have the necessary dependencies:

- Go (version 1.20+)
- Build essentials like `gcc`, `make`, `jq`, etc.

Install Go:
```bash
wget https://golang.org/dl/go1.20.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.20.linux-amd64.tar.gz
export PATH=$PATH:/usr/local/go/bin
```

### 2. **Install SEDA**
Clone the SEDA repository and build the binary:
```bash
git clone https://github.com/sedaprotocol/seda-core
cd seda-core
make install
```

Check if `sedad` is installed correctly:
```bash
sedad version
```

### 3. **Initialize Node**
Initialize your node by setting a moniker:
```bash
sedad init <your-moniker> --chain-id seda-testnet
```

### 4. **Set Up Configuration**
Download the configuration files and place them in the appropriate directory:
```bash
curl -s https://raw.githubusercontent.com/sedaprotocol/testnet/master/genesis.json > ~/.sedad/config/genesis.json
sed -i.bak 's/seeds = ""/seeds = "<seeds-from-github-page>"/' ~/.sedad/config/config.toml
```

### 5. **Start Your Node**
To start your node, use the following command:
```bash
sedad start
```

You can monitor your node’s synchronization status:
```bash
curl -s localhost:26657/status | jq .result.sync_info
```

### 6. **Create a Validator**
Once your node is fully synced (check `catching_up: false`), you can create a validator.

First, generate a validator key:
```bash
sedad keys add <validator-key>
```

Then, create the `validator.json` file with your validator information:
```json
{
  "pubkey": "$(sedad tendermint show-validator)",
  "amount": "100000000000000aseda", 
  "moniker": "<your-moniker>",
  "identity": "<optional-identity>",
  "website": "<optional-website>",
  "commission-rate": "0.1",
  "commission-max-rate": "0.2",
  "commission-max-change-rate": "0.01",
  "min-self-delegation": "1"
}
```

Create your validator by submitting the transaction:
```bash
sedad tx staking create-validator --amount="100000000000000aseda" --pubkey=$(sedad tendermint show-validator) --moniker="<your-moniker>" --chain-id seda-testnet --from <validator-key> --fees 5000aseda
```

### 7. **Configure for Upgrades (Optional)**
For automatic upgrades using Cosmovisor, install it:
```bash
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@latest
```

Add Cosmovisor to your environment:
```bash
echo "export DAEMON_NAME=sedad" >> ~/.profile
echo "export DAEMON_HOME=$HOME/.sedad" >> ~/.profile
source ~/.profile
```

Now, initialize Cosmovisor:
```bash
cosmovisor init sedad
```

### 8. **Advertise Your Validator**
To find your validator operator address:
```bash
sedad keys show <validator-key> --bech val -a
```
This is the address others can use to delegate tokens to your validator.

For more information, check out the [SEDA documentation](https://docs.seda.xyz).
