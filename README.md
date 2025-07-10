# ☁️ My First Azure VM Script (via Bash CLI)

This is a Bash script I wrote while following along with a **Microsoft Learn Azure Fundamentals lab** (AZ-900 level). It creates a simple virtual machine (VM) in Microsoft Azure using the Azure CLI.

Azure CLI (in Bash)

Cloud Shell (sandbox mode)

Ubuntu Linux VM

Basic Networking (open-port)

---

#!/bin/bash

# Variables
RESOURCE_GROUP="learn-rg-$RANDOM"
LOCATION="eastus"
VM_NAME="myFirstVM"

# 1. Create a resource group
echo "Creating resource group: $RESOURCE_GROUP"
az group create --name $RESOURCE_GROUP --location $LOCATION

# 2. Create the VM
echo "Creating virtual machine: $VM_NAME"
az vm create \
  --resource-group $RESOURCE_GROUP \
  --name $VM_NAME \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys

# 3. Open port 80 to allow web traffic
echo "Opening port 80"
az vm open-port --port 80 --resource-group $RESOURCE_GROUP --name $VM_NAME

# 4. Get the public IP address
IP_ADDRESS=$(az vm list-ip-addresses --resource-group $RESOURCE_GROUP --name $VM_NAME --query "[0].virtualMachine.network.publicIpAddresses[0].ipAddress" --output tsv)

echo "VM created successfully!"
echo "Public IP address: $IP_ADDRESS"
echo "You can now SSH into it or host a web page at http://$IP_ADDRESS"

## 🧠 What This Script Does

1. Creates a new **Resource Group**
2. Deploys a basic **Ubuntu Linux VM**
3. Opens **port 80** so it can serve a web page (like a "Welcome to Azure" page)
4. Pulls the **public IP address** of the VM so you can access it in your browser

Once it finishes, you’ll see the IP, and you can run:
```bash
curl http://<you
Real Things I Learned While Doing This

I wanted to make a note here for anyone else trying to follow along (or myself, if I ever forget!):

🔄 1. Switch to Bash — Not PowerShell

Microsoft Learn lets you use either Bash or PowerShell in Cloud Shell — but most tutorials are written in Bash, and I initially got stuck because I was trying to run Bash-style variable assignments like this:

IPADDRESS=$(az vm list-ip-addresses ...)

…inside PS which doesn’t work.
Double-check your resource group name

Another thing that confused me up was the exact resource group name. In the Microsoft Learn sandbox, it auto-generates names like:
learn-0e97d60f-9a09-404c-852b-30ad4d50580e
So if you accidentally mistype it (even a single letter), the script fails silently or gives weird errors like:
"URL has no host"

I added a random suffix to my resource group name ($RANDOM) in this script just to make it easier for testing.

Notes 📝 - 
This script was tested inside the Microsoft Learn sandbox — you can do it for free using their guided labs.

Don’t forget the sandbox times out after about an hour!

👋 About Me

I’m working toward earning my Microsoft Azure AZ-900 certification and looking to break into tech — especially roles related to IT support, cloud, or infrastructure. This is one of my first hands-on cloud projects.

Thanks for checking it out!
