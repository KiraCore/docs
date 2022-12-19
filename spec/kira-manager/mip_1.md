# [⏎](README.md#Roadmap) MIP_1
> Setup Scripts

`Initial Setup Script` should be a simple, one line shell script which enables to download and start an `Essential Dependencies Setup Script` as well as verify its integrity by prompting user to acknowledge a known checksum.

`Essential Dependencies Setup Script` should be a simple shell script installing only absolutely essential dependencies & services that will enable KM to be installed and started. This script should assume that its execution occurs on the freshly deployed Ubuntu 21.04 instance.

During initial setup user should accept terms and conditions as well as be prompted to input 4 types of 24 seed word mnemonics (or choose to auto generate them):

1. Controller Account Seed (validator)
2. Faucet Account Seed
3. Signing Key Seed
4. Node Key Seed

User should also define his default SSH port, otherwise his machine will be completely locked out during firewall setup.

## Logic Flow
* User Inputs Initial Setup Script - one line of code
  * Essential Dependencies Setup Script download
  * Essential Dependencies Setup Script integrity check 
  * Essential Dependencies Setup Script start
    * Security
      * Prompt user to accept terms & conditions
      * Prompt user to create password (if not already setup)
        * Store sha256 of the password to enable secure communication
          * e.g. in the `/kira/secrets` directory
      * Prompt user to input or auto generate seed words & store them in the `secrets` directory
    * Pre-Setup Maintenance
      * Ensuring Environment is Updated 
        * apt update, etc...
      * Ensuring all services that might interfere with setup are gracefully stopped
        *  systemctl stop...
        *  docker containers halt, etc...
    * Essential Dependencies Installation (the less the better)
      * Golang
      * Kira Manager systemctl service setup

## Initial Setup Script Example

```
cd /tmp && read -p "Input branch name: " BRANCH && \
 wget https://raw.githubusercontent.com/KiraCore/kira-manager/$BRANCH/init.sh -O ./i.sh && \
 chmod 555 -v ./i.sh && H=$(sha256sum ./i.sh | awk '{ print $1 }') && read -p "Is '$H' a [V]alid SHA256 ?: "$'\n' -n 1 V && \
 [ "${V,,}" != "v" ] && echo "Hash was NOT accepted by the user" || ./i.sh "$BRANCH"
```
