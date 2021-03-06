## Features
* Create an STA (Station) at any channel.
* Choose one of the following encryptions: WPA, WPA2, WPA/WPA2, Open, WEP.
* Add: **WPA3** (SAE/Enhanced Open)
* Choose one of the foloowing WPA pairwise: TKIP, CCMP, CCMP/TKIP
* Choose one of the foloowing WPA group: TKIP, CCMP, CCMP/TKIP
* Connect to hidden your SSID.


## Dependencies
### General
* bash (to run this script)
* util-linux (for getopt)
* procps or procps-ng
* wpa_supplicant
* dhclient
* iw
* iwconfig (you only need this if 'iw' can not recognize your adapter)
* haveged (optional)


## Installation
### Generic
    git clone https://github.com/verifsec/create_sta
    cd create_sta
    make install

## Examples
### No passphrase (open network):
    create_sta wlan0 MyAccessPoint

### No passphrase but encrypted (Enhanced Open)
    create_sta --enhanced_open -x /path/to/wpa_supplicant/ wlan0 MyAccessPoint
    (Note: You have better use wpa_supplicant 2.7 <=. See "Build wpa_supplicant for WPA3".)

### WEP key:
    create_sta --wep wlan0 MyAccessPoint MyPassPhrase

### WPA + WPA2 passphrase:
    create_sta -w 1+2 --pair CCMP+TKIP --group CCMP+TKIP wlan0 MyAccessPoint MyPassPhrase

### WPA-TKIP:
    create_sta -w 1 --pair TKIP --group TKIP wlan0 MyAccessPoint MyPassPhrase
    (Note: Realtek drivers usually have problems with WPA1, forcing WPA2.)

### WPA-CCMP:
    create_sta -w 1 --pair CCMP --group CCMP wlan0 MyAccessPoint MyPassPhrase

### WPA-CCMP/TKIP:
    create_sta -w 1 --pair CCMP --group TKIP wlan0 MyAccessPoint MyPassPhrase

### WPA2-TKIP:
    create_sta -w 2 --pair TKIP --group TKIP wlan0 MyAccessPoint MyPassPhrase

### WPA2-CCMP:
    create_sta -w 2 --pair CCMP --group CCMP wlan0 MyAccessPoint MyPassPhrase

### WPA2-CCMP/TKIP:
    create_sta -w 2 --pair CCMP --group TKIP wlan0 MyAccessPoint MyPassPhrase

### WPA3 (SAE Dragonfly):
    create_sta --sae -x /path/to/wpa_supplicant/ wlan0 MyAccessPoint MyPassPhrase
    (Note: You have better use wpa_supplicant 2.7 <=. See "Build wpa_supplicant for WPA3".)

### Use wpa_supplicant.conf:
    create_sta -c /path/to/wpa_supplicant.conf wlan0 MyAccessPoint

### Use DHCP Client:
    create_sta --dhcp -w 1+2 --pair CCMP+TKIP --group CCMP+TKIP wlan0 MyAccessPoint MyPassPhrase

### SSID Stealth:
    create_sta --hidden -w 1+2 --pair CCMP+TKIP --group CCMP+TKIP wlan0 MyAccessPoint MyPassPhrase

### Use WPA Pre-Shared-Key (instead of WPA Passphrase):
    create_sta --psk -w 1+2 --pair CCMP+TKIP --group CCMP+TKIP wlan0 MyAccessPoint MyPreSharedKey

## Build wpa_supplicant for WPA3
### wpa_supplicant-2.7
    apt install pkg-config libnl-3-dev libssl-dev libnl-genl-3-dev
    wget https://w1.fi/releases/wpa_supplicant-2.7.tar.gz
    tar xvzf ./wpa_supplicant-2.7.tar.gz
    cd ./wpa_supplicant-2.7/wpa_supplicant/
    echo -ne "\nCONFIG_OWE=y\nCONFIG_IEEE80211W=y\nCONFIG_DPP=y\nCONFIG_SAE=y\nCONFIG_SUITEB=y" >> ./defconfig
    cp ./defconfig .config
    make -j 2

### WPA3 Dragonfly
    create_sta --sae -x ./ --dhcp wlan0 MyAccessPoint MyPassPhrase

## License
FreeBSD
