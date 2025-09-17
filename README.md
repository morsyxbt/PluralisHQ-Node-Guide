# 🧩PluralisHQ Research Node 0-7.5B Guide - Using Docker

---

**Pluralis is building Protocol Learning : a decentralized peer-to-peer system for collaborative AI model training across consumer GPUs worldwide, enabling permissionless and fault tolerant pretraining of large language models without central orchestration**

---

### Hardware Requirements :

* **CPU**   : 8++
* **RAM**   : 32++ GB
* **Disk**  : 100 GB+ NVME GB SDD
* **GPU**   : 16GB Vram++

#### Rent a Server on Vast : Skip If You Can Run Locally

1. Go to [Vast.ai](https://cloud.vast.ai/billing/)
2. add balance crypto or card
3. rent a server - easy & lots of guides on CT already

---

### 1. Connect To Server : If Rented Server - Skip If Running Locally
```
ssh -p <port> root@<server_ip>
```

### 2. Update Server :
```
sudo apt update -y && sudo apt upgrade -y
```

### 3. Install Required Packages :
```
sudo apt install -y htop ca-certificates zlib1g-dev libncurses5-dev \
libgdbm-dev libnss3-dev tmux iptables curl nvme-cli git wget make jq \
libleveldb-dev build-essential pkg-config ncdu tar clang bsdmainutils \
lsb-release libssl-dev libreadline-dev libffi-dev gcc screen file unzip lz4
```

### 4. Install Docker :
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] \
https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io

docker version
```

### 5. Install Docker Compose :
```
VER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep tag_name | cut -d '"' -f 4)
curl -L "https://github.com/docker/compose/releases/download/$VER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

### 6. Enable Docker For Current User :
```
sudo groupadd docker
sudo usermod -aG docker $USER
```

- `Log out & back in, or run newgrp docker to apply immediately`

---

### 7. Clone Node0 Repo :
```
git clone https://github.com/PluralisResearch/node0
cd node0
```

### 8. Build Docker Image :
```
docker build . -t pluralis_node0
```

---

#### 9. Get HuggingFace Token :

- Create account: https://huggingface.co/
- Create token: New Access Token

### 10. Generate Start Script :

- Replace `<HF_TOKEN>` and `<email>` with your details
- Use Vast.ai external port for `--announce_port`
- If Running Locally replace `<external_port_from_vast>` with `49200`

```
python3 generate_script.py --use_docker \
  --host_port 49200 \
  --announce_port <external_port_from_vast> \
  --token <HF_TOKEN> \
  --email <your_email>
```

- **When it asks questions, type n to skip unnecessary prompts**
- **It will create start_server.sh.**

#### 11. Start Node :
```
./start_server.sh
```

#### 12. Check Logs :
```
tail -f run.out
```

#### 13. Download `private.key` :

- If you don't have direct file access, run :

```
python3 -m http.server 8080
```

- Then open in browser :
- http://localhost:8080/ → download private.key
- Stop server with CTRL+C

---

- **That's it** ✅
- **You’re now contributing to Node0 training swarm** 🚀

---
**Made with ❤️ by [Morsyxbt](https://x.com/morsyxbt)**
---
