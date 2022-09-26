---
sidebar_position: 4
---

# Run a validator node

## Using AWS Image
A Newrl node can be instantly spawned using the pre-built Newrl Image on AWS. It's currently available in Ohio and N.Virginia.
At the launch time, the only configuration that needs to be specified is the security group to allow incoming connections on ports `8456` for mainnet and `8420` for testnet.
![AWSNewrl](/img/aws_newrl_image.png)

### Add Node wallet to chain
Once the node is live, it's generated wallet public key can be obtained from the URL `/get-node-info`. The node's wallet need to be added on the chain with the help of a custodian or as a linked wallet from [Newrl wallet app](wallet.newrl.net).
Example: If node's public IP is 54.91.131.211, the url will be http://54.91.131.211:8420/get-node-info.

## From source code
### Prerequisites 
1. A computer accessible outside with port open for incoming.
    1. To start a node on AWS, launch an EC2 instance from instructions [here](https://docs.aws.amazon.com/efs/latest/ug/gs-step-one-create-ec2-resources.html). Make sure to make port 8456 open to public when configuring the security group. 
    2. To start a node on Digital ocean droplet, instructions [here](https://docs.digitalocean.com/products/droplets/quickstart/)
3. Git installed. Steps [here](https://git-scm.com/downloads)
4. Python3.7+ installed. This is preinstalled on most common linux distributions. On ubuntu the steps will be `sudo apt update && sudo apt install python3.10-venv`
6. Pip and python3 venv installed. Installation steps available [here](https://pip.pypa.io/en/stable/installation/)

### Installation

```bash
git clone https://github.com/newrlfoundation/newrl.git
scripts/install.sh
```

### Start the node
```bash
screen -S newrl
scripts/start.sh
```
Screen session is used to let the node run in background when terminal is closed. 

Note: The port 8456 should be publicly accessible on the instance. On AWS, this can be done by opening inbound port in the security group. For local installations, the router need to be configured to bypass symmetric NAT for the port 8456.