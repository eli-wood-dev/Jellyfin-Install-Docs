# Jellyfin

## Initial setup

### Update Pi

Run the following commands to update the pi

- `sudo apt update`
- `sudo apt upgrade -y`

### Get the Jellyfin packages

Run the following commands to get the Jellyfin packages

- `sudo apt install apt-transport-https lsb-release`
- `curl https://repo.jellyfin.org/debian/jellyfin_team.gpg.key | gpg --dearmor | sudo tee /usr/share/keyrings/jellyfin-archive-keyring.gpg >/dev/null`
- `echo "deb [signed-by=/usr/share/keyrings/jellyfin-archive-keyring.gpg arch=$( dpkg --print-architecture )] https://repo.jellyfin.org/debian $( lsb_release -c -s ) main" | sudo tee /etc/apt/sources.list.d/jellyfin.list`

### Install Jellyfin

Run the following commands to install Jellyfin

- `sudo apt update`
- `sudo apt install jellyfin`

Wait for the download to finish then continue

### Allowing traffic

If ufw is being used then port 8096 should be opened

`sudo ufw allow 8096`

`sudo ufw enable`

Before enabling ufw ensure other critical ports are opened. (22 for SSH, 80 for HTTP, and 443 for HTTPS)

### Test that the site is available

Visit `<hostname>.local:8096` to access the site

If the hostname is not known you can find the ip address with `ifconfig` then visit `<ip>:8096`

### Configure Jellyfin

Once you access the site it will bring you through a configuration process

- Create a user that you will use to access Jellyfin
- Select `Add Media Library`
- Choose the content type of the media library, e.g. Movies, Music, Books
- Choose the name for your media library
- Press the `+`bButton next to the `Folders` tag
- Select the filepath to your media (this should be on an external media like a usb hard drive)
 
> you can also use network shares as the path for your media

- Select `OK`
- Select `Next`
- Choose your Language and Country
- Ensure `Allow remote connections to this server` is checked
- Select `Finish`
- Log into the account you created earlier

You are now in the Jellyfin dashboard

### Enabling Hardware Acceleration (Optional)

Running Jellyfin is very demanding for a Raspberry Pi, especially if multiple devices try to access it

Start by allowing Jellyfin to use the built in GPU

- `sudo usermod -aG video jellyfin`
- `sudo nano /boot/firmware/config.txt`
- Go to the bottom of the file and add this line `gpu_mem=<value>` where value is 320 if you are using a pi 4 or 256 if ou are using a pi 3
- Save the file
- Restart the pi with `sudo reboot now`
- Verify that the memory was updated with `vcgencmd get_mem gpu`

Next you need to enable hardware acceleration

- In the Jellyfin web interface select the person icon in the top right then click on `Dashboard`
- Select `Playback` then `Hardware acceleration`
- Select `Video4Linux2 (V4L2)` (This is the only one that works on the Raspberry Pi)
- Scroll to the bottom and select `Save`

## Next Steps

### Adding new media libraries

- click on the hamburger in the upper right
- select `Dashboard`
- under `Server`, select `Libraries` and click `Add media library`
- Choose the content type of the media library, e.g. Movies, Music, Books
- Choose the name for your media library
- Press the `+` button next to the `Folders` tag
- Select the filepath to your media (this should be on an external media like a usb hard drive)
  
> you can also use network shares as the path for your media

- Select `OK`

### Adding content to media libraries

In order to add content to an existing library you only need to add the media files to the specified folder from earlier

### Accessing the server remotely

If you wish to access the servere remotely you will need some way to enter your network externally, either through port forwarding or a vpn

You can also put Jellyfin behind a [reverse proxy](https://jellyfin.org/docs/general/networking/#running-jellyfin-behind-a-reverse-proxy)

There are two ways of accessing the media, using the website or using the app

### Jellyfin app

- Download the Jellyfin app from the [Jellyfin Github page](https://github.com/jellyfin/jellyfin-media-player/releases)
- Enter the host name or ip of the pi followed by the port, e.g. `192.168.1.100:8096`
- Sign into the user account you used in the setup

### Jellyfin on a browser

- Navigate to the hostname or ip of the pi followed by the port number, e.g. `192.168.1.100:8096`
- Log into the user you used previously

### Creating a new user

- click on the hamburger in the upper right
- select `Dashboard`
- under `Server`, select `Users`
- Press the `+` button
- Enter the `name` and `password` of the new user
- Under` Libraries` Choose what media libraries the user can access, or select `Enable access to all libraries`
- Click `Save`
