# Ubuntu_developer_on_windows

## WSL2
WSL2 is essentially a high-performance virtual machine capable of running Linux on Windows. It can execute programs at nearly the same speed as compared to a native installation. It is necessary to install since the Framework is designed to run on Ubuntu 20.04.

Microsoft provides [detailed instructions](https://docs.microsoft.com/en-us/windows/wsl/install), but it boils down to launching Powershell as an admin.

Steps:
1. **Choose the ubuntu image and Install**

```
wsl --install -d Ubuntu-18.04
```
Note: you can check all online ubuntu images by ```wsl --list -o```.

2. **Configure your ubuntu vm**

- set your account and passcode for the ubuntu VM.
- update packages:
```
sudo apt update
sudo apt upgrades -y
```
- install gedit
```
sudo apt install gedit
```

3. **setup display for VM**

It is often useful or necessary to run graphical tools within WSL2, and the following steps will allow you to do that. WSL2 does not have direct access to the host machine's display and requires forwarding rendering instructions to the host using an X server.

First, download and install [VcXsrv](https://sourceforge.net/projects/vcxsrv/), which is an X server that will run on windows. Launch it, but on the "Extra Settings" page, deselect "Native opengl", and select "Disable access control". On the next page, you can click "Save configuration" to create a shortcut file that will launch it with these settings by default in the future. You will need to ensure the program is running (it will be visible in your tray) any time in the future you want to forward graphics from WSL2 to Windows.
> **You will need to re-launch the program, by double-clicking the saved configuration you just made, every time you restart your PC in the future.**
>

Second, append the following to your `bashrc` as before, so that WSL2 can find the X Server running on windows.
```bash
# Display settings for X forwarding
export DISPLAY=$(awk '/nameserver / {print $2; exit}' /etc/resolv.conf 2>/dev/null):0
```

You can test that everything functioned by installing and running a small tool that will display a pair of eyes on your screen.
```bash
sudo apt install x11-apps
xeyes
```


## Windows Terminal (optional)
This is simply a visual upgrade to the default terminal. Since the Windows Store is blocked, you can install it manually by finding the most recent [release](https://github.com/microsoft/terminal/releases), scrolling to "Assets", then downloading and installing the proper one for your operating system.

## VSCode
Visual Studio Code is a free IDE that integrates nicely with with the tools we've installed. Install it from the official [downloads](https://code.visualstudio.com/download) page, then launch it and install the following extensions (extensions are on the 5th icon down from the top, on the left sidebar):
- Remote - WSL (ms-vscode-remote.remote-containers)
- Remote - Containers (ms-vscode-remote.remote-wsl)
- git history

Add the following to your `bashrc`:
```bash
#VSCode display workaround
export WSL_DISPLAY=$DISPLAY
```
