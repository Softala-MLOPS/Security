# SET UP GUIDE FOR CSC USER:

## I) CSC setup

1. Set up your account in [CSC](https://my.csc.fi/welcome) using HAKA Login. (This might take up to 30 minutes, so don't worry if it doesn't instantly allow you in.)
2. Create a new project. Navigate to Projects pane and click "New project". (Creating a new project might also take a while.)
3. Once you have your own project, you should be able to access it through your project pane.
4. In your project, scroll down to the "Services" section.
5. Click "Add services" and select cPouta as the service to run.
6. Login to cPouta using Haka. (This will take a while.)

## II) cPouta setup

### a) Security Group Setup
1. Once inside cPouta, navigate to Security Groups: "Network" -> "Security Groups".
2. Create a new security group. **Warning: As a good practice, do not modify the default security group. Create a new one instead.**
3. After creating a new security group, you can modify its rules by clicking "Manage Rules".
4. Create a new rule by clicking "Add Rule".
5. Choose SSH, HTTP and HTTPS as rules. Leave the rest as it is.

For more information on what these steps mean, please consult the [user guide made by CSC](https://docs.csc.fi/cloud/pouta/launch-vm-from-web-gui/#setting-up-ssh-keys).

### b) Key Pairs Setup
1. Navigate to Key Pairs: "Compute" -> "Key Pairs".
2. Either import an existing SSH key pair or create a new one by clicking "Create Key Pair".
3. Name your key pair - try to name it in lower case, no space, and no special characters. Key type is SSH Key.
**Important:** The private key will be saved automatically to your computer once you click "Create Key Pair". This is the only way to access your virtual machine, so make sure not to lose it. File type should be ".pem" for Windows and Linux, and ".cer" for Mac.

For more information please consult the [user guide made by CSC](https://docs.csc.fi/cloud/pouta/launch-vm-from-web-gui/#setting-up-ssh-keys).

### c) Instances setup
1. Navigate to Instances: "Compute" -> "Instances".
2. Launch your Instance:
    - "Details":
        - 2.1. Name your instance whatever you like.
        - 2.2. Keep "Count" as 1.
    - "Source":
        - 2.3. Under "Select Boot Source" select "Image".
        - 2.4. Select image named "Ubuntu-24.04 (3.50 GB)".
    - "Flavor":
        - 2.5. Select standard.xxlarge at minium.
    - "Security Groups":
        - 2.6. Select both the default security group and the one you created earlier.
    - "Key Pair":
        - 2.7. Select key pair you created earlier.
    - 2.8. Click "Launch Instance" to launch.
3. Once your Instance is running (see if under Power State it states "Running"), associate your machine with a floating IP:
    - 3.1. Under "Actions" next to "Create Snapshot", click on the down arrow.
    - 3.2. Select "Associate Floating IP".
    - 3.3. Click on the "+" icon and "Allocate IP". No need to change any settings here.
    - 3.4. Click "Associate".
    - Write down your IP address somewhere. If you forget it, you can find it in the “Floating IPs” section under "Network" tab.

Again, for more information please consult the [CSC user guide](https://docs.csc.fi/cloud/pouta/launch-vm-from-web-gui/#setting-up-ssh-keys).

## III) Connecting to cPouta machine from your local machine

### For Windows user

1. You should install [Window Subsystem for Linux (WSL)](https://learn.microsoft.com/en-us/windows/wsl/install).
2. Move the key you saved on your local computer in the earlier step (see b) Key Pairs Setup) to your Linux environment. Save it in the `home/<yourusername>/` directory.

### The rest of the setup will be universal!

1. Using the command line, navigate to the folder where your key file (see b) Key Pairs Setup) is stored.
2. Change the file permissions with the following command: `chmod 400 <your_key_name>.pem` (or `chmod 400 <your_key_name>.cer` on Mac).
3. SSH into your Virtual Machine with the following command: `sudo ssh -L 8080:localhost:8080 <your_username>@<your_floating_IP> -i <your_key_name>.pem`. In this case, **replace `<your_username>` with `ubuntu`** - reason can be found in [CSC user guide](https://docs.csc.fi/cloud/pouta/connecting-to-vm/).
4. Once inside, run: `sudo apt update` and `sudo apt upgrade`.
5. Install Docker LTS using the command: `sudo apt install docker.io`.
6. Verify that the installation was successful by running: `docker version`.
7. Clone the Github repository into the VM.
8. Navigate into the cloned repository using: `cd <repo_name>`.

9. Follow the rest of the tutorial in [Installation guide](https://github.com/Softala-MLOPS/oss-mlops-platform/blob/main/tools/CLI-tool/Installations%2C%20setups%20and%20usage.md#installations-setups-and-usage).
