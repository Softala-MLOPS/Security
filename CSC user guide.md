# SET UP GUIDE FOR CSC USER:

## I) CSC setup

1. Set up your account in [CSC](https://my.csc.fi/welcome) using HAKA Login. (This might take up to 30 minutes, so don't worry if it doesn't instantly allow you in.)
2. Create a new project. You should see a button to create a new project either in your dashboard pane or in your project pane. (This might also take a while.)
3. Once you have your own project, you should be able to access it through your project pane.
4. When you are in your project, scroll down and there should be a tab called "Services".
5. Add service and choose cPouta as the service to run.
6. Login in to cPouta using haka. (This will take a while.)

## II) cPouta setup

### a) Security Group Setup
1. Once you are in cPouta, navigate to Security Groups: Network -> Security Groups.
2. Create a new security group (DO NOT MODIFY THE DEFAULT SECURITY GROUP IF YOU DON'T KNOW WHAT YOU ARE DOING).
3. Once the new security group has been created, you can modify the rules by pressing "Manage Rules".
4. Create a new rule by pressing "Add Rule".
5. Choose SSH, HTTP and HTTPS as rules. Leave the rest as it is.

For more information on what these steps mean, please consult the [user guide made by CSC](https://docs.csc.fi/cloud/pouta/launch-vm-from-web-gui/#setting-up-ssh-keys).

### b) Key Pairs Setup
1. Navigate to Key Pairs: Compute -> Key Pairs.
2. Either import a SSH key pair you have made or create a new key pair by pressing "Create Key Pair".
3. Name your key pair - try to name it in lower case, no space, and no special characters. Key type is SSH Key.
**Important:** The key will be saved automatically to your computer once you press "Create Key Pair". This will be the only way to access the virtual machine, so don't lose it. File type should be ".pem" for Windows and Linux, for Mac it will be ".cer"

For more information please consult the [user guide made by CSC](https://docs.csc.fi/cloud/pouta/launch-vm-from-web-gui/#setting-up-ssh-keys).

### c) Instances setup
1. Navigate to Instances: Compute -> Instances
2. Launch your instance:
    - 2.1. Name your Instance whatever you want
    - 2.2. Flavour choose standard xxlarge as the minium
    - 2.3. Keep the number instance as 1
    - 2.4. Instance Boot Source, choose Boot from Image 
    - 2.5. Image name is "Ubuntu-24.04 (3.5GB)"
    - 2.6. In the "Access & Security" tab check:
        - 2.6.1. if "Key Pair" is the key pair name that you had create previously
        - 2.6.2. Choose both of the security groups
    - 2.7. Launch your Instances by press "Launch"
3. Once your instances is running (see if under Power State it write "Running"), Associate your machine with a floating IP:
    - 3.1. Under "Actions" next to "Create Snapshot" press on the down arrow
    - 3.2. Choose "Associate Floating IP"
    - 3.4. Press on the "+" icon and choose Allocate IP, you don't need to change any settings here
    - Write your IP down somewhere but if you forgot to you can just check in your network tab under "Floating IPs"

Again for more information please consult the [CSC user guide](https://docs.csc.fi/cloud/pouta/launch-vm-from-web-gui/#setting-up-ssh-keys)

## III) Connecting to cPouta machine from your local machine

### For Window user

1. You should install [Window Subsystem for Linux (WSL)](https://learn.microsoft.com/en-us/windows/wsl/install)
2. Move your key pairs that you have save in your computer from the earlier step into your linux environment under `home/<yourusername>/`

The rest of the setup will then be universal!

1. Locate and navigate your key path in your system
2. Change the permission by using the command: `chmod 400 <your_key_name>.pem` or `chmod 400 <your_key_name>.cer` on mac
3. ssh into your Virtual machine using the command line by using the command: `sudo ssh -L 8080:localhost:8080 <your_username>@<your_IP> -i <your_key_name>.pem`
**<your_username> replace with `ubuntu` - reason with a detailed explanation can be found in [CSC user guide](https://docs.csc.fi/cloud/pouta/connecting-to-vm/)**
4. Once inside you should do `sudo apt update` and `sudo apt upgrade`
5. Then you should install docker LTS using the command `sudo apt install docker.io`
6. Check if you had install docker successfully by using `docker version`
7. Clone the Github repo into to the VM
8. go into the Github repo by using `cd`

**\---------This part is for the version of the project where there is error in the setup script\---------**

9. `cd` into scripts
10. Open `install_tools.sh` using either nano or vim
11. Replace the script with the script from this [link](https://github.com/makotosoul/oss-mlops-platform/blob/main/scripts/install_tools.sh)

**\---------End\---------**

12. Follow the rest of the tutorial in [Installation guide](https://github.com/Softala-MLOPS/oss-mlops-platform/blob/main/tools/CLI-tool/Installations%2C%20setups%20and%20usage.md#installations-setups-and-usage)
