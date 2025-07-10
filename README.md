# ☁️ My First Azure VM Script (via Bash CLI)

This is a Bash script I wrote while following along with a **Microsoft Learn Azure Fundamentals lab** (AZ-900 level). It creates a simple virtual machine (VM) in Microsoft Azure using the Azure CLI.

Azure CLI (in Bash)

Cloud Shell (sandbox mode)

Ubuntu Linux VM

Basic Networking (open-port)

---

# 1. Create a resource group
echo "Creating resource group: $RESOURCE_GROUP"
az group create --name $RESOURCE_GROUP --location $LOCATION

1.2 > CREATE VM 
az vm create \
--resource-group $RESOURCE_GROUP \
--name $VM_NAME \
--image UbuntuLTS \
--admin-username azureuser \
--generate-ssh-keys

Web Server Setup 
The script below opens port 80 but doesn’t install a web server. For a complete “Welcome to Azure” page, you could add a step to SSH into the VM and install Apache/Nginx. For example:
ssh azureuser@$IP_ADDRESS "sudo apt update && sudo apt install -y apache2 && echo '<h1>Welcome to Azure</h1>' | sudo tee /var/www/html/index.html"

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
