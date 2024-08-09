# my_lava_validator_guide
In this simple guide, I will help you become a validator for the lava protocol project.
![lava_network-860x484](https://github.com/user-attachments/assets/2273257d-f96e-4188-93cd-3d1f9966c75b)



**How to Set Up a Lava Node and Become a Validator Without Docker**

Lava is a decentralized infrastructure protocol, and by running a Lava Node and becoming a validator, you can actively contribute to the network's stability while earning rewards. This guide will walk you through the steps to set up a Lava Node without using Docker.

### Prerequisites

Ensure you have the following:

- **Hardware Requirements**:
  - **CPU**: 4 cores (8 threads recommended)
  - **RAM**: 16 GB or more
  - **Storage**: SSD with at least 500 GB available
  - **Bandwidth**: Minimum 1 Gbps connection with at least 100 Mbps upload/download speed
- **Operating System**: Ubuntu 20.04 or later (Linux recommended)
- **Basic Command Line Knowledge**: Familiarity with terminal commands is essential.

### Step 1: Update and Prepare Your System

1. **Update Your System**: Run the following commands to ensure your system is up-to-date:

    ```bash
    sudo apt-get update && sudo apt-get upgrade -y
    ```

2. **Install Dependencies**: Install the necessary packages:

    ```bash
    sudo apt-get install build-essential git curl jq -y
    ```

3. **Install Go**: Lava is written in Go, so you’ll need to install it:

    ```bash
    wget https://golang.org/dl/go1.20.7.linux-amd64.tar.gz
    sudo tar -C /usr/local -xzf go1.20.7.linux-amd64.tar.gz
    echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc
    source ~/.bashrc
    ```

    Verify the installation:

    ```bash
    go version
    ```

### Step 2: Download and Build the Lava Node

1. **Clone the Lava Node Repository**:

    ```bash
    git clone https://github.com/lava-protocol/lava-node.git
    cd lava-node
    ```

2. **Checkout the Latest Stable Release**:

    ```bash
    git checkout <latest-release-tag>
    ```

    Replace `<latest-release-tag>` with the latest stable release tag from the repository.

3. **Build the Lava Node**:

    ```bash
    make install
    ```

    This will compile the node software and install it on your system.

4. **Verify the Installation**:

    After the build process completes, you can verify the installation by checking the version:

    ```bash
    lava version
    ```

### Step 3: Configure the Lava Node

1. **Initialize the Node**:

    Initialize the Lava node with your custom moniker (a unique name for your node):

    ```bash
    lava init <your-moniker> --chain-id=<chain-id>
    ```

    Replace `<your-moniker>` with your desired node name and `<chain-id>` with the appropriate chain ID for the Lava network.

2. **Download Genesis File**:

    Fetch the genesis file that corresponds to the Lava network:

    ```bash
    curl -s <genesis-file-url> > ~/.lava/config/genesis.json
    ```

    Replace `<genesis-file-url>` with the actual URL of the genesis file.

3. **Configure Peers**:

    Update your `config.toml` file to add persistent peers and configure other network settings:

    ```bash
    nano ~/.lava/config/config.toml
    ```

    In the `[p2p]` section, add peers to the `persistent_peers` parameter. You can find peers from the Lava network community.

4. **Set Minimum Gas Prices**:

    Configure minimum gas prices in `app.toml`:

    ```bash
    nano ~/.lava/config/app.toml
    ```

    Look for `minimum-gas-prices` and set it to a reasonable value, e.g., `0.025ulava`.

5. **Pruning and Other Configurations**:

    Adjust the pruning settings in `app.toml` to save disk space if necessary:

    ```bash
    pruning = "custom"
    pruning-keep-recent = "100"
    pruning-keep-every = "0"
    pruning-interval = "10"
    ```

### Step 4: Start the Lava Node

1. **Start the Node**:

    You can start your node with the following command:

    ```bash
    lava start
    ```

2. **Check Sync Status**:

    Monitor the node’s synchronization status by running:

    ```bash
    lava status 2>&1 | jq .SyncInfo
    ```

    The node will need to sync with the blockchain, which may take some time depending on your connection and the blockchain size.

### Step 5: Become a Validator

Once your node is fully synced, you can proceed with becoming a validator.

1. **Create a New Wallet or Import an Existing One**:

    To create a new wallet:

    ```bash
    lava keys add <wallet-name>
    ```

    To import an existing wallet:

    ```bash
    lava keys import <wallet-name> <key-file>
    ```

2. **Fund Your Wallet**:

    Ensure your wallet has enough funds to cover the staking amount and transaction fees. You can transfer tokens from an exchange or another wallet.

3. **Generate Validator Keys**:

    ```bash
    lava tendermint show-validator
    ```

4. **Submit the Validator Creation Transaction**:

    Use the following command to submit a validator creation transaction:

    ```bash
    lava tx staking create-validator \
      --amount=<amount_of_stake> \
      --pubkey=$(lava tendermint show-validator) \
      --moniker=<your-moniker> \
      --chain-id=<chain-id> \
      --commission-rate="0.10" \
      --commission-max-rate="0.20" \
      --commission-max-change-rate="0.01" \
      --min-self-delegation="1" \
      --from=<wallet-name> \
      --fees=5000ulava
    ```

    Replace placeholders with your information.

5. **Check Your Validator Status**:

    After submitting the transaction, you can check the status of your validator:

    ```bash
    lava query staking validator $(lava keys show <wallet-name> --bech val -a)
    ```

### Step 6: Regular Maintenance and Updates

- **Monitor Node Performance**: Regularly monitor your node's performance and logs to ensure everything is running smoothly.
  
    ```bash
    tail -f ~/.lava/log/lavad.log
    ```

- **Upgrade Node**: Keep your node up-to-date by pulling the latest changes from the repository and rebuilding the node:

    ```bash
    git pull
    make install
    lava unsafe-reset-all
    lava start
    ```

- **Security**: Regularly back up your validator key and keep your wallet secure.

### Conclusion

By setting up and running a Lava Node, and becoming a validator, you are actively contributing to the security and decentralization of the Lava network. Ensure you keep your node well-maintained and stay informed about network updates. Welcome to the Lava validator community!
