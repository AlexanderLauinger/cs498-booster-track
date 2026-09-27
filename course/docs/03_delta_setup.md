# Getting Setup on the Delta Cluster

As members of the class project through NCSA, every student in this class has access to GPU hours on the Delta cluster as well as the Delta AI cluster. These clusters have different resources, but most of your assignments and projects can be trained and run on either. There may be times where queue times are faster on one machine than the other, so we recommend you set up on Delta as well.

This guide assumes you have already gone through `00_deltaai_setup.md`. The instructions for delta are mostly identical with a few key differences.

# Part A — Get access to Delta

### A.1 First SSH connection

From your laptop's terminal (macOS/Linux: built in; Windows: use Windows
Terminal + OpenSSH, or WSL):

```bash
ssh <your_ncsa_username>@login.delta.ncsa.illinois.edu
```

Your username is exactly the same as when you logged in on Delta AI. The hostname round-robins to four login nodes, dt-login01 … dt-login04. Your prompt shows which one you got. (ex. `[<your_ncsa_username>@dt-login0X ~]$`). This is the login node. Just like Delta AI, the GPU is accessed through Slurm.

Authentication is password + Duo push. The password is exactly the same as when you logged in on Delta AI.

Once this works. Add the following to your `.ssh/config` file **on your laptop**.

```
Host delta
    HostName login.delta.ncsa.illinois.edu
    User <your_ncsa_username>
    LocalForward 8080 localhost:8080

Host dt-login01 dt-login02 dt-login03 dt-login04
    HostName %h.delta.ncsa.illinois.edu
    User <your_ncsa_username>
    LocalForward 8080 localhost:8080
```

After that, `ssh delta` both logs you in and forwards the viewer port, and `ssh dt-login03` does the same to one specific login node. You need the second form whenever you use two terminals: delta round-robins over four nodes, and the forward reaches port 8080 only on the node that session landed on. Run `hostname` in the terminal where you plan to run `uv run play`, and connect to that node.

### A.2 Know where your files live

The file system on Delta is near identical to DeltaAI. In particular, the `/work/nvme/bign/<your_ncsa_username>` and `/work/hdd/bign/<your_ncsa_username>` folders are shared between Delta AI and Delta. This means that a file created on one cluster automatically exists on the other cluster. For this reason it's a good idea to put all important files and large or long-lived datasets in these folders. If, for whatever reason, you loose access or can't train on Delta or Delta AI, you can easily switch to the other cluster and your data will still be there.

However, your home folder, `$HOME` (`/u/<your_ncsa_username>`), is **NOT** shared between the two. For that reason, you will have to reclone your github repo and redo some of the setup tasks your first time. You will also likely want to create your own private github repository for your assigments in this class so that you can push work you do on one cluster and pull it into another easily. If you'd like some directions on how to do that, see `04_github_setup.md`. NOTE: If you push your work to your own repository, it must be a **private** repository. Pushing your solutions to assignments to a public repository is a violation of academic integrity.

# Part B — Install the course environment (on the login node)

This setup only needs to happen once, the first time you use Delta. Most of it is identical to the setup for Delta AI.

### B.1 Clone the repo

```bash
git clone https://github.com/parasollab/cs498-booster-track.git ~/(your_repo_name)
cd ~/(your_repo_name)
```



### B.2 Install uv

`uv` is the Python package/environment manager the course standardizes on:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Then restart your shell, and confirm `uv --version` prints something.

### B.3 Sync the environment

```bash
uv sync
```

### B.4 Verify

```bash
uv run python scripts/check_login_env.py
```

# Part C — Running a training job with `sbatch`

Some configurations required for delta are slightly different than delta AI. For that reason, the `train.sbatch` script has been renamed as the `train_delta.sbatch` and the `train_deltaai.sbatch` scripts. Use the script which corresponts to what cluster you are on. Otherwise, the command to run it is the same. For example, on the cartpole assignment, you would run:

```bash
sbatch scripts/train_<delta or deltaai>.sbatch Course-Cartpole-Swingup --env.scene.num-envs 4096
```
