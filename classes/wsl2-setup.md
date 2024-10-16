### WSL(2) setup

1. Enable virtualization in UEFI (BIOS)  
2. Enable WSL and virtual pc features for Windows  
3. Upgrade WSL

```console
PS> wsl update
PS> wsl --list --online 
```
4. Install distro  
```console
PS> wsl –install Ubuntu-22.04  
```
5. Check version of WSL and ubuntu distro installed
```console
PS> wsl -l -v
```
6. Start your distro under WSL  
```console
PS> wsl
```

### Setup `lxc`/`lxd` 

We’ll use WSL2 as a platform for `lxc`/`lxd` container manager

Start shell in WSL2.  
```console
PS> bash.exe
```
Check privileges.  
```console
id  
sudo id  
```
To install we'll use different manager. Check `snap` is running.
```console
systemctl status snap 
```
What packages are there already?
```console
snap list
```
Install `lxd`
```console
sudo snap install lxd  
```
Initialize lxd ('no root file system' error would appear later if you miss it)  
Next step prepares settings for container volumes
```console
sudo lxd init --minimal
```
Start `lxd`
```console
sudo snap start lxd
```
Verify it's running
```console
sudo systemctl status snap.lxd.daemon
```
To avoid constantly prefix with `sudo` setup user group
```console
sudo usermod ubuntu -aG lxd
```
To take effect log out and log back in.
```console
groups
```
Notice that following commands are `lxc` not `lxd`\!\!\!  
Commands can be entered in the shell as argument, e.g. list installed containers
```console
lxc list
```
See containers images available
```console
lxc image list images: | less
```
But we focus on setting up lxd WebUI that would be available to host windows system. 
```console
sudo apt install net-tools 
ifconfig
```
Copy address. Set port number for `lxd` to which web service would be bound
```console
lxc config set core.https_address :8443
```
Use copied ip and port to open in browser. Ignore warning to go ahead. Follow instructions given in the page to generate certificate. Supply `.crt`-file to `lxd`
```console
lxc config trust add /mnt/c/Users/User/Downloads/lxd-ui.crt 
```
Import the other file in browser certificate manager.


