# Setup Tailscale on Windows and Mac

This is divided on MacOS, Windows, and Linux, specifically Artix Linux using runit.

> [!NOTE]
> In the case of MacOS and Linux we will be using the Open Source version of Tailscale.

1. [Tailscale on MacOS](#setup-tailscale-on-mac)
2. [Tailscale on Windows](#setup-tailscale-on-windows)
3. [Tailscale on Linux](#setup-tailscale-on-linux)

## Setup Tailscale on Mac

Before downloading Tailscale, first make sure you have Golang installed on your Mac, you can install it [here](https://go.dev/dl/) or if you have Homebrew on your Mac you can run the following.

```bash
brew install go
```

After it has been installed run `go version` and you should see the version of Go that you have downloaded.

After that, run the following command to install the installation of Tailscale on your computer

```bash
go install tailscale.com/cmd/tailscale{,d}@main
```

With this Tailscale should be installed on your Mac, now run the daemon.

```bash
sudo $HOME/go/bin/tailscaled install-system-daemon
```

With this command Tailscale should start automatically after login, to confirm that the model worked properly, you can go to `/Library/LaunchDaemons` and you should see a `com.tailscale.tailscaled.plist` file in there

Finally, before finishing on Mac, you will have to run the following command to authenticate your Tailscale and be able to communicate between Windows and Mac.

```bash
~/go/bin/tailscale up
```

This command will show you a link that you need to follow to log in, after you do this Tailscale will run after you log in without the need to authenticate again.

If you are not using your Mac as the main server you can try checking the connection with your server IP (if already setup)

```bash
curl http://<YOUR_SERVER_IP>:11434 # The port 11434 will communicate with Ollama directly
```

## Setup Tailscale on Windows

The installation on Windows is more simple, simply go to the [Tailscale website](https://tailscale.com/download) and install the application, it will also prompt to authenticate, make sure that you use the same account, after this is done you'll be ready to go.

If you want to run Tailscale on start up on your computer you can press Win+R and enter `shell:startup`, this will open a folder on Windows Explorer, here you can either drop or create a shortcut that points to the `.exe` file of Tailscale.

After you have set up Tailscale on both computers you should be able to see both devices active and the Tailscale IP Address for each one of them.

To make sure our computers can communicate between each other run the following command

On Windows.

```powershell
curl http://<YOUR_SERVER_IP>:11434
```

You should receive `Ollama is running` on your shell, this means Windows is communicating with your Mac, and more importantly, Ollama is receiving requests.

On Mac you can ping the IP of your Windows computer and should all the responses.

## Setup Tailscale on Linux

> [!WARNING]
> This section is for the Artix Linux distro, specifically to the Artix Linux version running the runit init system.

To install Tailscale you can use Pacman.

```bash
pacman -S tailscale
```

After this you will need to create the runit service to start Tailscale with your computer.

```bash
sudo mkdir -p /etc/runit/sv/tailscaled
sudo nano /etc/runit/sv/tailscaled/run
```

In side of the run file add the following.

```sh
#!/bin/sh
exec /usr/sbin/tailscaled 2>&1
```

And make the run file executable.

```bash
sudo chmod +x /etc/runitt/sv/tailscaled/run
```

Start the service and verify it is running.

```bash
sudo ln -s /etc/runit/sv/tailscaled /run/runit/service/

# Verify that the service is running
sudo sv tailscaled
```

Finally authenticate to your Tailscale account.

```bash
sudo tailscale up # You will get an URL to authenticate

# Check the communication with your server after setting up Tailscale
curl http://<YOUR_SERVER_IP>:11434 
```