# General ADA setup
<!-- Author : Siddarth Gottumukkula -->

## Docs : https://hpc.iiit.ac.in/wiki/index.php?title=Ada_User_Guide

## Accessing account

To get access to ADA account, contact advisor and send a mail to hpc@iiit.ac.in
Once the account is setup, proceed to the terminal and enter the following command.

`ssh <username>@ada.iiit.ac.in`

Enter the password when prompted to.

Use `pestat -G` to check out the free nodes and resources.

In the `Partition` column, we can see the split between the servers. All the nodes marked u22 are open to use for accounts not associated with labs that have specific clusters. 

From node 43, all the nodes have 2080 Ti GPUs while all the nodes before have 1080 Ti GPUs

## Connecting to a gnode

To run in the debug mode (will automatically disconnect access after 6 hours, will talk about creating longer jobs in a later section), use the following command. 

- `sinteractive` sets the mode
- `-p` is used to set the partition. Enter the cluster partition after this.
- `-c` is used to set the number of CPU cores
- `-g` is used to set the number of GPUs. The CPU cores-to-GPUs ratio is recommended to be kept above 9
- `-A` is used to set the account type. Enter 'research' is its a research account.
- `-w` is used to select the gnode. Following this flag, enter the gnode name.

For example, the gnode050 u22 cluster with 36 CPU cores and 4 GPUs from a research account is to be accessed using the command below.

`sinteractive -p u22 -c 36 -g 4 -A research -w gnode050`

- Note that based on QOS(quality of service) of the Ada account, the number of GPUs and CPUs we can take up are limited.
    - A Low QOS account is limited to 1 GPU and 10 CPUs. It will throw an error if we try to claim more resources.

## Virtual environment

### Initialization

Once you enter, install `miniconda` to setup a virtual environment. Use the following resource : https://www.anaconda.com/docs/getting-started/miniconda/install#linux-terminal-installer

PS. If conda is not recognizd even after installation, manually initialize shell (commands in the same link above).

### Linking to VScode

- Open a new window in VScode and click on the `Open a Remote Window` option on the bottom-left corner.
- Click on the `Connect to host` option.
- If no gnode is in the list, click on the `Configure SSH Hosts` option and select the path. For example, `C:\Users\<name>\.ssh\config`.
- Add the following lines to the file, enter the required content and save it.

```
Host ada.iiit.ac.in
    HostName ada.iiit.ac.in
    User <username>

Host gnode<num>
    HostName gnode<num>
    LocalForward 6000 localhost:22
    ProxyJump <username>@ada.iiit.ac.in
    User <username>
```

- Now the desired gnode ahould be visible in the `Connect to host` option.
    - If another gnode is occupied later, just change `gnode<num>` in the config file seen earlier.
- You will be prompted to enter your password multiple times when ever you try to connect and open. Kindly do so.
- Click on `Select a folder` and now click on `OK` beside the search bar to select the home directory.  
- Here, you can create a folder and start working and executing code.

## Few general pointers

- All the libraries must be installed in this virtual environment. Check the requirements of the program and do so.
- Ensure that PyTorch is installed along with CUDA using `conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia` (Or select the desired CUDA version). And of course, set the device to 'cuda' in the code to utilize the GPU's power.
- Use `nvidia-smi` to check status of the GPUs.
- Use `exit` to cleanly exit the session. Can also directly close the terminal to exit the session.
- Be mindful of the time limit and storage limit (one of the storage spaces is cleared after 7 days).

## GUI to operate Ada

- We can use Termius app to setup the host as `ada.iiit.ac.in` and access the system.
    - This is not a free app, has a free trial for ~2 weeks. Get used to the CLI for the long run.
- Enter username and password for the Ada account in the settings of the Ada server.
- The main advantage is the ease of loading and accessing files directly without going into sinteractive mode.

## Scheduling batches for large tasks (#TODO)

- We need to use `sbatch` to schedule the workload. For this, we must write a bash script that initializes and runs the files. A sample bash script is given below.

```
#SBATCH ...
...
...
```

- Use the command `sbatch <bash_file_name>.sh` 

## Memory Usage Command in Linux
https://chatgpt.com/share/688a6899-06f0-8005-aea7-fcab78d27963