# INSTALL DROSERA NETWORK WITH CUSTOM TRAP
Contract : 0xF72B291c102e9277412963084076de71d5e66b52

Function : Auto Mint NFT and Auto Send To Trap Address

# System Requirements
Spec : 4 GB RAM, 2 Core, 20 GB SSD

# Tested 
Ubuntu 22

# Mining ETH Hoodi
https://hoodi-faucet.pk910.de/

# Setup
```
screen -R drosera
```
```
sudo apt-get update && sudo apt-get upgrade -y && sudo apt install curl ufw iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev  -y && sudo apt update -y && sudo apt upgrade -y && for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
```
```
sudo apt-get update && sudo apt-get install ca-certificates curl gnupg && sudo install -m 0755 -d /etc/apt/keyrings && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg && sudo chmod a+r /etc/apt/keyrings/docker.gpg
```
```
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
```
sudo apt update -y && sudo apt upgrade -y && sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin && sudo docker run hello-world
```
```
curl -L https://app.drosera.io/install | bash && source /root/.bashrc && droseraup
```
```
curl -L https://foundry.paradigm.xyz | bash && source /root/.bashrc && foundryup
```
```
curl -fsSL https://bun.sh/install | bash && mkdir my-drosera-trap && cd my-drosera-trap && forge init -t drosera-network/trap-foundry-template
```
```
apt install snapd
```
```
snap install bun-js
```
```
curl -fsSL https://bun.sh/install | bash &&  bun install && forge build
```
```
DROSERA_PRIVATE_KEY=YOURPRIVATEKEY drosera apply
```
```
drosera dryrun
```
```
nano drosera.toml
```
# Change "path & response_contract & response_function" with :
```
path = "out/Trap.sol/Trap.json"
response_contract = "0xF72B291c102e9277412963084076de71d5e66b52"
response_function = "randomMint(string)"
```
```
nano src/Trap.sol
```
# Paste This Code :
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import {ITrap} from "drosera-contracts/interfaces/ITrap.sol";

interface IEthereumHoodie {
    function isActive() external view returns (bool);
}

contract Trap is ITrap {
    address public constant RESPONSE_CONTRACT = 0xF72B291c102e9277412963084076de71d5e66b52;
    string constant discordName = "usernamediscord"; // 

    function collect() external view returns (bytes memory) {
        bool active = IEthereumHoodie(RESPONSE_CONTRACT).isActive();
        return abi.encode(active, discordName);
    }

    function shouldRespond(bytes[] calldata data) external pure returns (bool, bytes memory) {
        (bool active, string memory name) = abi.decode(data[0], (bool, string));
        if (!active || bytes(name).length == 0) {
            return (false, bytes(""));
        }
        return (true, abi.encode(name));
    }
}
```
```
curl -LO https://github.com/drosera-network/releases/releases/download/v1.18.0/drosera-operator-v1.18.0-x86_64-unknown-linux-gnu.tar.gz
```
```
tar -xvf drosera-operator-v1.18.0-x86_64-unknown-linux-gnu.tar.gz
```
```
sudo cp drosera-operator /usr/bin
```
```
source /root/.bashrc && droseraup
```
```
forge build
```
```
DROSERA_PRIVATE_KEY=YOURPRIVATEKEY drosera apply
```
```
drosera dryrun
```
```
drosera-operator register --eth-rpc-url https://rpc.hoodi.ethpandaops.io --eth-private-key PRIVATE_KEY --drosera-address 0x91cB447BaFc6e0EA0F4Fe056F5a9b1F14bb06e5D
```
```
drosera-operator optin --eth-rpc-url https://rpc.hoodi.ethpandaops.io --eth-private-key PRIVATE_KEY --trap-config-address TRAPS_ADDRESS
```
```
drosera bloomboost --private-key PRIVATE_KEY --trap-address TRAPS_ADDRESS --eth-amount 3
```
```
git clone https://github.com/0xmoei/Drosera-Network
```
```
cd && cd Drosera-Network
```
```
docker pull ghcr.io/drosera-network/drosera-operator:latest
```
```
nano docker-compose.yaml
```
# Delete all data, paste this :
```
version: '3'
services:
  drosera:
    image: ghcr.io/drosera-network/drosera-operator:latest
    container_name: drosera-node
    ports:
      - "31313:31313"
      - "31314:31314"
    volumes:
      - drosera_data:/data
    command: node --db-file-path /data/drosera.db --network-p2p-port 31313 --server-port 31314 --eth-rpc-url https://rpc.hoodi.ethpandaops.io --eth-backup-rpc-url https://ethereum-hoodi-rpc.publicnode.com/ --drosera-address 0x91cB447BaFc6e0EA0F4Fe056F5a9b1F14bb06e5D --eth-private-key ${ETH_PRIVATE_KEY} --listen-address 0.0.0.0 --network-external-p2p-address ${VPS_IP} --disable-dnr-confirmation true
    restart: always

volumes:
  drosera_data:
```
```
docker compose up -d && docker compose logs -f
```

