## **Installing the node**

### **Step 1**

**Download ready-made binaries from the official github** *(this link is relevant for MAIN.1.0, if you are reading this guide, but there is already a new version is available, try changing the number 1.0 in the link to the number of the current version)*:

```bash
Linux:

wget https://github.com/massalabs/massa/releases/download/MAIN.2.1/massa_MAIN.2.1_release_linux.tar.gz

Linux Arm64:

wget https://github.com/massalabs/massa/releases/download/MAIN.2.1/massa_MAIN.2.1_release_linux_arm64.tar.gz
```

Unpack the downloaded archive:

```bash
Linux:

tar zxvf massa_MAIN.2.1_release_linux.tar.gz

Linux Arm64:

tar zxvf massa_MAIN.2.1_release_linux_arm64.tar.gz
```

You won’t believe it, but that’s it! You have installed the node.

### **Step 2**

Before starting the node, we will create a config file with the ip-address of your server into it — for the node to be routable (replace the **IP_ADDRESS** with the IPv4/IPv6 address of your server), then copy and paste whole the block:

```bash
sudo tee <<EOF >/dev/null $HOME/massa/massa-node/config/config.toml
[protocol]
routable_ip = "IP_ADDRESS"
EOF
```

### **Step 3**

Create a **service file** for the node to run in the background and does not stop after closing the terminal.
Please note — replace the word **YOUR_PASSWORD** with your own password, the same password that you used when creating a wallet!

(Copy the block below in Notepad, replace the word YOUR_PASSWORD with your password and paste it entirely on the command line, not line by line.)

```bash
printf "[Unit]
Description=Massa Node
After=network-online.target
[Service]
User=$USER
WorkingDirectory=$HOME/massa/massa-node
ExecStart=$HOME/massa/massa-node/massa-node -p YOUR_PASSWORD
Restart=on-failure
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target" > /etc/systemd/system/massad.service
```

Next, run the node using the service file (3 commands):

```bash
sudo systemctl daemon-reload

sudo systemctl enable massad

sudo systemctl restart massad
```

Checking that the node is working properly:

```bash
sudo journalctl -f -n 100 -u massad
```

### **Step 4**

Now let’s move on to setting up the client / wallet.
Launching the client for the first time. It will require, like the node, to come up with a password (I advise you to create the same as on the node, although most likely this is not entirely correct).

```bash
cd $HOME/massa/massa-client/

./massa-client
```

Next we need to restore the wallet (Your wallet secret key instead of Secretkey):

```bash
wallet_add_secret_keys Secretkey
```

Copy the **wallet address** and add it to the stake:

```bash
node_start_staking WALLET_ADDRESS
```

Checking that the node is ok:

```bash
cd /$HOME/massa/massa-client/ && ./massa-client -p YOUR_PASSWORD wallet_info
```

### **It remains only to buy a roll for our coins.
We go to the client:**

```bash
cd /$HOME/massa/massa-client/ && ./massa-client -p YOUR_PASSWORD
```

and in the client we buy a roll using the command:

```bash
buy_rolls YOUR_WALLET_ADDRESS Number_of_rolls Commission

# In the above case, the command looks like this:

buy_rolls A1zpSerGtbwjiAEK3yueeTAFTgFzrFtJ3p33c4ACu9ytDFQTAjU 1 0
```
