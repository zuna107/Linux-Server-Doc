
# Linux Server Documentation - Linux Armbian [B860H](https://github.com/topics/b860h)-[HG680P](https://github.com/topics/hg680p)

This documentation is used for Linux Server: `Armbian` - `Amlogic` - `Casa Os`
which I use for `Node.js` servers.

This Armbian type is used for a Set Top Box (STB) that has an [Amlogic](https://github.com/topics/amlogic) type cipset.

## [Install Armbian](https://www.mediafire.com/file/ic0kgvrr30tqlrh/Armbian_20.10.img.xz/file)

- Prepare an SD Card with at least 8GB storage and a Card reader.
- Burn the Armbian.img file with `balenaEtcher` or `Rufus` software and navigate to the SD Card.
- Open the "boot SD card" and enter the `extlinux` folder and open the file named `extlinux.conf` with notepad. then remove the **#** in `rk-3399` and add **#** in `aml s9xxx`.
- Next in the "boot" folder change `u-boot-s905-s912` to `u-boot.ext`.
- then put the `u-boot-hg680p` file into boot and rename it to **uboot.bin**, you can get the **uboot** file in this Repository.

**Important Notes:**
1. The main requirement is that the Set Top Box (STB) is already **unlocked (Root)** and **[Open Wrt](https://github.com/topics/openwrt)** is installed on the system.
2. For STB `b860h` must use **hg680p** bootloader.

- Remove the SD card and insert it into the STB, then plug your network LAN port into the STB, wait for a while and look for the connected device on the router.
- Search for the STB device's `IP Address` on the router's configuration page or you can search for it using the `Advanced IP Scanner` software, usually the device will be named **Arm-64**.

## Set up Linux

- Type the IP Address in the `Putty` software.
Default Username: root
Password : 1234
(You will set a new username and password and etc.)
- Update the Linux repository:
```
sudo apt update
```

## Install Casa Os
```
curl -fsSL https://get.casaos.io | sudo bash
```
CasaOS fully supports ZimaBoard, Intel NUC, and Raspberry Pi. Also, more computers and development boards and fully compatible with Ubuntu, Debian, Raspberry Pi OS, and CentOS with one-liner installation.

#### Hardware Compatibility
- amd64 / x86-64
- arm64
- armv7
#### System Compatibility
Official Support

- Debian 12 (✅ Tested, Recommended)
- Ubuntu Server 20.04 (✅ Tested)
- Raspberry Pi OS (✅ Tested)
 

## Node JS Server Setup

- Upload the application (bot/app, etc) into the `DATA/name_folder` by excluding the `node_modules` folder when uploading your project.
- Open Casa Os terminal > change directory to your application folder (cd.. > cd DATA > cd folder_application).
- Install `node_modules` with the command:
```
npm install
```

#### [PM2 Setup](https://pm2.keymetrics.io/)
- Open Casa Os terminal and install Pm2:
```
npm install pm2 -g
```
- Create a file named `ecosystem.config.js` in your application directory folder, and fill it in:
```js
module.exports = {
  apps: [
    {
      name: 'your_application_name',
      script: './src/index.js', //customize index.js location
      instances: 1,
      autorestart: true,
      watch: true,
      watch_delay: 1000,
      max_memory_restart: '1G', //restart when memory usage reaches 1GB
      env: {
        NODE_ENV: 'development',
      },
      env_production: {
        NODE_ENV: 'production',
      },
    },
  ],
};


```
- Run the application with the command:
```
pm2 start ecosystem.config.js
```
