# OG-Storage-Node
# 🛑 0G Storage Node Guide 🛑

---

## 💻 Device/System Minimum Requirements

| Component   | Minimum Requirement |
|-------------|---------------------|
| **RAM**     | 32 GB                |
| **Storage** | 500 GB              |
| **CPU**     | 8 Core              |

---

## 🛑 At First

1. **Add 0G-Galileo-Testnet chain** from here:  
   🔗 [https://docs.0g.ai/run-a-node/testnet-information](https://docs.0g.ai/run-a-node/testnet-information)

2. **Take faucet** from:  
   💧 [https://faucet.0g.ai/](https://faucet.0g.ai/)

---

## 🛑 Install All Dependencies

```bash
sudo apt-get update
```

```bash
sudo apt-get upgrade -y
```

```bash
sudo apt install curl iptables build-essential git wget lz4 jq make cmake gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev screen ufw -y
```

---

## 🛑 Install rustup

```bash
curl https://sh.rustup.rs -sSf | sh
```

```bash
source $HOME/.cargo/env
```

### • Check Version

```bash
rustc --version
```

---

## 🛑 Install Go

```bash
wget https://go.dev/dl/go1.24.3.linux-amd64.tar.gz && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf go1.24.3.linux-amd64.tar.gz && \
rm go1.24.3.linux-amd64.tar.gz && \
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc && \
source ~/.bashrc
```

### • Check Version

```bash
go version
```

---

## 🛑 Clone the Repository

```bash
git clone https://github.com/0glabs/0g-storage-node.git
```

```bash
cd 0g-storage-node && git checkout v1.0.0 && git submodule update --init
```

---

### • Build Easy Mode

> Build in release mode

```bash
cargo build --release
```

---

## 🛑 Set Configurations

```bash
rm -rf $HOME/0g-storage-node/run/config.toml
```

```bash
curl -o $HOME/0g-storage-node/run/config.toml https://raw.githubusercontent.com/Mayankgg01/0G-Storage-Node-Guide/main/config.toml
```

> 📝 **IMPORTANT:**  
> Add your wallet’s **Private Key** in `config.toml`  
> ⚠️ **Do not include** `0x` before the private key.

---

### • Open and go to `miner_key` and add your private key:

```bash
nano $HOME/0g-storage-node/run/config.toml
```

---

### • Change The RPC

1. Get RPC from here:  
   🔗 [https://www.astrostake.xyz/0g-status](https://www.astrostake.xyz/0g-status)

2. Choose any RPC and edit it in the `config.toml` file.

---

## 🛑 Create a Systemd Service File

```bash
sudo tee /etc/systemd/system/zgs.service > /dev/null <<EOF
[Unit]
Description=ZGS Node
After=network.target

[Service]
User=$USER
WorkingDirectory=$HOME/0g-storage-node/run
ExecStart=$HOME/0g-storage-node/target/release/zgs_node --config $HOME/0g-storage-node/run/config.toml
Restart=on-failure
RestartSec=10
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

---

### • Reload

```bash
sudo systemctl daemon-reload
```

### • Enable

```bash
sudo systemctl enable zgs
```

### • Start Service

```bash
sudo systemctl start zgs
```

---

## 🛑 Check Logs

```bash
sudo systemctl status zgs
```

---

### • Full Logs

```bash
tail -f ~/0g-storage-node/run/log/zgs.log.$(TZ=UTC date +%Y-%m-%d)
```

---

### • Check block & Sync process - Match to the latest block on explorer

```bash
while true; do
  response=$(curl -s -X POST http://localhost:5678 -H "Content-Type: application/json" -d '{"jsonrpc":"2.0","method":"zgs_getStatus","params":[],"id":1}');
  logSyncHeight=$(echo $response | jq '.result.logSyncHeight');
  connectedPeers=$(echo $response | jq '.result.connectedPeers');
  echo -e "logSyncHeight: [32m$logSyncHeight[0m, connectedPeers: [34m$connectedPeers[0m";
  sleep 5;
done
```

---

## 🛑 If You Want to Delete 0G Node From VPS

```bash
sudo systemctl stop zgs
```

```bash
sudo systemctl disable zgs
```

```bash
sudo rm /etc/systemd/system/zgs.service
```

```bash
rm -rf $HOME/0g-storage-node
```

---

## 🛑 Check Here

- **Explorer** (View your transactions - Paste your address):  
  🔗 [https://chainscan-galileo.bangcode.id/](https://chainscan-galileo.bangcode.id/) OR [https://chainscan-galileo.0g.ai/](https://chainscan-galileo.0g.ai/)

- **View Miner Details** (Add your wallet address at the end of the link):  
  🔗 [https://storagescan-galileo.0g.ai/miner/](https://storagescan-galileo.0g.ai/miner/)

---

❤️ **Join TG for more Updates:**  
🔗 [https://t.me/CryptoRikhav](https://t.me/CryptoRikhav)
