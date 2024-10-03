# Day-20-Setting-Up-Your-Own-Mythic-C2-Instance

Are you ready to dive into the world of Command and Control (C2) frameworks? Mythic is one of the most powerful and customizable C2 platforms out there, designed with red teams and penetration testers in mind. In this post, I’ll guide you step-by-step through setting up your own Mythic C2 instance on a cloud server, and by the end, you’ll have a solid foundation to explore its potential.

Let’s get started!

## Step 1: Choose Your Cloud Provider (I’m Using Vultr)
First things first — head over to your cloud provider. For this guide, I’m using Vultr, but you can follow along with any cloud service that lets you spin up a virtual machine (VM).

Once you’re logged in, it’s time to deploy a new server. Click on the **Deploy** button at the top right corner and choose **Deploy New Server**.

### Server Specifications
For my setup, I’m using:
- **Cloud Compute (Shared CPU)**
- **Location**: London
- **OS**: Ubuntu 22.04 LTS
- **Specs**: At least 2 vCPUs and 4GB of RAM (this is crucial for running Mythic smoothly)

#### My Selected Configuration:
- **Name**: 80GB SSD
- **Cores**: 2 vCPUs
- **Memory**: 4GB RAM
- **Storage**: 80GB SSD

Don’t forget to uncheck Auto Backups and IPv6 — we won’t need them for this setup. You can also name the server whatever you like. Once everything looks good, click **Deploy Now** and wait for the server to spin up.

## Step 2: Connecting to Your Server
Once your server is up and running, the next step is to SSH into it. You’ll see your server’s IP address in Vultr’s dashboard.

For Windows users, open PowerShell and run:
```bash
ssh root@<your-server-ip>
```

You’ll be prompted for your password, which you can find in the Vultr console. Once you’re in, it’s time to get to work!

## Step 3: Update & Upgrade Your Server
Before diving into Mythic, let’s make sure your system is fully up-to-date. Run the following commands to update your repositories and upgrade the installed packages:
```bash
apt-get update && apt-get upgrade -y
```
Once that’s done, we can start setting up Mythic.

## Step 4: Install Docker Compose & Make
Mythic requires Docker to run, so the next step is to install Docker Compose and Make.

### Install Docker Compose:
```bash
apt install docker-compose
```

Install Make:
```bash
apt install make
```
That’s it! You’ve now got the basic components required to run Mythic.

## Step 5: Clone the Mythic Repository
Now, let’s clone Mythic from its GitHub repository. Run the following command:

```bash
git clone https://github.com/its-a-feature/Mythic
```
Once cloned, navigate into the Mythic directory:

```bash
cd Mythic
```
Now, it’s time to install the Mythic framework itself. Run:

```bash
./install_docker_ubuntu.sh
```
This will take a little while as it installs all the necessary components. Once it’s done, restart Docker and check that it’s running:

```bash
systemctl restart docker
systemctl status docker
```
Make sure Docker shows an “active” status before proceeding.

## Step 6: Start Mythic
Now that Docker is running, we can start Mythic using the `make` command:

```bash
make
```
This will set up everything in the background. Once that’s done, start the Mythic CLI by typing:

```bash
./mythic-cli start
```

Congratulations! Your Mythic C2 instance is now up and running. But wait, there’s one more thing we need to do before accessing the Mythic Web GUI.

## Step 7: Configure Your Firewall
Before you go any further, it’s crucial to set up a firewall. This ensures that only specific machines—like your own and your target—can communicate with the Mythic server. You don’t want the entire internet trying to poke around!

To do this, head back to Vultr and create a new Firewall Group. Add the IP addresses of the machines you trust—such as your Windows server, Linux box, and Mythic server.

Once that’s configured, you’re all set to access Mythic.

## Step 8: Access the Mythic Web GUI
Now for the fun part! Open your browser and navigate to the Mythic Web GUI using the IP address of your Mythic server:

```bash
https://<your-mythic-ip>:7443
```

You’ll be prompted for a username and password. The default username is `mythic_admin`. To get the password, SSH into your server and type:

```bash
ls -la
```

This will show hidden files, including `.env`. Open that file by running:

```bash
cat .env
```

Scroll down to find `MYTHIC_ADMIN_PASSWORD=`. Copy that password and paste it into the login page. Now you’re in!

## Mythic Overview
Once logged in, you’ll be greeted by the Mythic dashboard — a sleek, intuitive interface that’s ready to orchestrate your C2 operations. From here, you can manage agents, profiles, and tasks with ease.

But that’s a story for another day.

## What’s Next?
In my next post, we’ll dive deeper into Kali Linux, generate payloads, and use a C2 profile to create a Mythic agent that can establish communication with our Windows server. Remember, Mythic is powerful, but with great power comes great responsibility. Always practice in controlled environments and be mindful of what you’re doing.

We’ll walk through the entire process of creating your own agent and attacking the Windows server we set up. Stay tuned — things are about to get interesting!
