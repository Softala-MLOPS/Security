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
3. Once the new security group has been created, you can modify the rules by clicking "Manage Rules".
4. Create a new rule by clicking "Add Rule".
5. Choose SSH, HTTP and HTTPS as rules. Leave the rest as it is.

For more information on what these steps mean, please consult the [user guide made by CSC](https://docs.csc.fi/cloud/pouta/launch-vm-from-web-gui/#setting-up-ssh-keys).

### b) Key Pairs Setup
1. Navigate to Key Pairs: Compute -> Key Pairs.
2. Either import a SSH key pair you have made or create a new key pair by clicking "Create Key Pair".
3. Name your key pair - try to name it in lower case, no space, and no special characters. Key type is SSH Key.
**Important:** The key will be saved automatically to your computer once you click "Create Key Pair". This will be the only way to access the virtual machine, so don't lose it. File type should be ".pem" for Windows and Linux, and ".cer" for Mac.

For more information please consult the [user guide made by CSC](https://docs.csc.fi/cloud/pouta/launch-vm-from-web-gui/#setting-up-ssh-keys).

### c) Instances setup
1. Navigate to Instances: Compute -> Instances
2. Launch your Instance:
    - 2.1. Name your Instance whatever you want
    - 2.2. Flavour choose standard xxlarge as the minium
    - 2.3. Keep the number instance as 1
    - 2.4. Instance Boot Source, choose Boot from Image 
    - 2.5. Image name is "Ubuntu-24.04 (3.5GB)"
    - 2.6. In the "Access & Security" tab, check:
        - 2.6.1. if "Key Pair" is the key pair name you created previously
        - 2.6.2. Choose both of the security groups
    - 2.7. Launch your Instance by clicking "Launch"
3. Once your Instance is running (see if under Power State it states "Running"), associate your machine with a floating IP:
    - 3.1. Under "Actions" next to "Create Snapshot" press on the down arrow
    - 3.2. Choose "Associate Floating IP"
    - 3.4. Press on the "+" icon and choose Allocate IP, you don't need to change any settings here
    - Write your IP down somewhere but if you forgot to you can just check in your network tab under "Floating IPs"

Again, for more information please consult the [CSC user guide](https://docs.csc.fi/cloud/pouta/launch-vm-from-web-gui/#setting-up-ssh-keys).

## III) Connecting to cPouta machine from your local machine

### For Windows user

1. You should install [Window Subsystem for Linux (WSL)](https://learn.microsoft.com/en-us/windows/wsl/install).
2. Move the key you saved on your local computer in the earlier step (see b) Key Pairs Setup) to your Linux environment. Save it in the `home/<yourusername>/` directory.

### The rest of the setup will be universal!

1. Locate the key file you created earlier (see b) Key Pairs Setup) on your system, and navigate to it using the command line.
2. Change the file permissions with the following command: `chmod 400 <your_key_name>.pem` (or `chmod 400 <your_key_name>.cer` on Mac).
3. SSH into your Virtual Machine with the following command: `sudo ssh -L 8080:localhost:8080 <your_username>@<your_IP> -i <your_key_name>.pem`. In this case, **replace `<your_username>` with `ubuntu`** - reason can be found in [CSC user guide](https://docs.csc.fi/cloud/pouta/connecting-to-vm/).
4. Once inside, run: `sudo apt update` and `sudo apt upgrade`.
5. Install Docker LTS using the command: `sudo apt install docker.io`.
6. Verify that the installation was successful by running: `docker version`.
7. Clone the Github repository into the VM.
8. Navigate into the cloned repository using: `cd <repo_name>`.

9. Follow the rest of the tutorial in [Installation guide](https://github.com/Softala-MLOPS/oss-mlops-platform/blob/main/tools/CLI-tool/Installations%2C%20setups%20and%20usage.md#installations-setups-and-usage).
