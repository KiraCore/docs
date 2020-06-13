# [⏎](README.md) Workstation Setup

**WARNING: Because many users might have access to this repository or apply changes to the code that might compromise your environment, it is mandatory for everyone to utilise VM as your working environment at all times. Your host should never, under any circumstances execute any scripts even if written by you alone due to potential malicious behavior of dependencies. If despite those warnings you decide to launch `infra` on your host machine then we take no responsibilities for potential damages of your hardware or files**

> _NOTE: To maintain consistency among all working environments we will utilise `Ubuntu 20.04 LTS` as a mandatory OS for your workstation_

For the purpose of setting up development environment we will 

1. Install [VMWare Workstation 15.5+](https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html)
   
2. Download and Install [Ubuntu 20.04 LTS](https://releases.ubuntu.com/20.04/) with VMWare
   * Recommended VM Setup
     * CPU: 4 virtualized cores
     * RAM: 8 GB (4GB minimum)
     * Disk: 64 GB (SSD or NVMe)

   > NOTE: Your username should only contain alphanumeric characters and no spaces
3. Boot your machine, and ensure latest updates are applied using `Software Updater`

   ![picture 1](https://i.imgur.com/7SX2g7yl.png)

   > NOTE: It is recommended that after applying all updates you should shut down your machine, locate your VM folder, e.g. `C:\Users\<user>\Documents\Virtual Machines\<name>` and zip its content for backup purposes.

4. Start your VM & Open terminal to execute following command that will launch a setup script

    > _NOTE: You will be prompted to input branch names you are working with as well as email address where you will receive notifications, to simplify the setup you can press [ENTER] key every time you are prompted to used the default (recommended) settings_

    ```
    sudo -s

    cd /tmp && rm -f ./init.sh && wget https://raw.githubusercontent.com/KiraCore/infra/master/workstation/init.sh -O ./init.sh && chmod 777 ./init.sh && ./init.sh
    ```

5. Go though installation setup of your working environment

   This step (running for the first time) might take up to 90 minutes to install all dependencies, depending on your network connection. If you get disconnected or something goes wrong during the process you can return to step 4

   > _NOTE: If you want to stay 100% safe, you should create a new gmail account to receive notifications from Kira's virtual environment_

   i. (OPTIONAL) Define email where you want to receive build notifications

   ii. (OPTIONAL) [Enable SMTP](https://www.youtube.com/watch?v=D-NYmDWiFjU) and [less secure apps](https://web.archive.org/save/https://hotter.io/docs/email-accounts/secure-app-gmail/) in your gmail account, then provide your login and password as SMTP credentials

   iii. (OPTIONAL) In your github go to [Account Settings](https://github.com/settings/profile) -> [SSH and PGP keys](https://github.com/settings/keys) -> [New SSH Key](`https://github.com/settings/ssh/new`) and add new ssh key using provided to you by the `KIRA-MANAGER` PUBLIC ssh key (or create new one and provide PRIVATE ssh key to the `KIRA-MANAGER`). If you do not provide SSH key or do not add new public ssh key to your github account you will not be able to access many useful features offered by the  **KIRA GIT MANAGER**

   ![picture 5](https://i.imgur.com/5MUhRWK.png)  

   
   > _NOTE: If you want to stay 100% safe, you should create a new github account, and request access to `sekai` and all other repositories you want to interact with though `KIRA GIT MANAGER`_

6. Allow launching of KIRA-MANAGER

   Right-click on the `KIRA-MANAGER` desktop icon and select `Allow Launching` option from the menu as demonstrated by the picture below

    ![picture 1](https://i.imgur.com/4EKLdEhl.png)

   By double clicking on the icon you will be prompted to enter password and will be presented with a management console.

7. See what is going on with your new network !
   
   **KIRA NETWORK MANAGER** enables you to restart your environment and pull latest changes from github, every time you make change to repositories you are working with you should select option `[R]` to re-build everything (If you only work with `sekai` that should take less then 3 minutes to complete!). If you ever need to update your default settings such as SSH key or notifications email address you can select option `[I]` and go through setup once again.

   ![picture 1](https://i.imgur.com/iyuqqsz.png)

   **KIRA CONTAINER MANAGER** allows you to inspect any running container, such as validator based of your `sekai` repository. You can also manage the container by stopping, starting, pausing or unpausing it. If something goes wrong and your validator fails all you have to do to find out what happened is click `[L]` and inspect logs in the pre-installed visual studio code editor. Longs will contain all information's necessary do debug. To refresh logs you have to run `[L]` option again, you can do it bot with container in running or failed state.

   ![picture 3](https://i.imgur.com/LFKtIAm.png) 

   **KIRA GIT MANAGER** allows you to edit, push, pull, resolve conflicts, change branches and even merge all your code changes with other branches without having to leave your virtual machine. Clicking `[V]` option will open a visual studio code editor and allow you deep dive into the code in minutes.

   ![picture 4](https://i.imgur.com/OKGDSMH.png)  