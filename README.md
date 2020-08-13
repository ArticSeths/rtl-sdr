# RTL-SDR
Turns your Realtek RTL2832 based DVB dongle into a SDR receiver

### First configuration
```
sudo apt-get update -y
sudo apt-get install cmake make libusb-1.0-0-dev -y

# Optional (for examples)
sudo apt-get install sox
sudo apt-get install netcat
```
### Installation and compile

```sh
git clone https://github.com/ArticSeths/rtl-sdr.git
cd rtl-sdr
mkdir build
cd build
cmake ../
make

# optional
sudo make install
```

### Optional cmake params
-- For install udev rules (to be able to use the dongle as a non-root user), use `-DINSTALL_UDEV_RULES=ON`

-- For building with kernel detach, use `-DDETACH_KERNEL_DRIVER=ON`
```
cmake ../ -DINSTALL_UDEV_RULES=ON -DDETACH_KERNEL_DRIVER=ON
```

### How to use (Examples)
If you didn't use a optional make install, you need to specified the route of compilation like this:
`/root/rtl-sdr/build/src/rtl_fm ...`

#### -- Play FM
You need install sox
```
rtl_fm -f 91.0e6 -M wbfm -s 256000 -r 22050 - | play -r 22050 -t s16 -L -c 1  -
```
Explained:
Record the FM frequency 91.0, with sample rate at 256k, resampling at 22050 and play it with play (sox)

#### -- Stream over TCP
You need install netcat
```
rtl_fm -f 91.0e6 -M wbfm -s 256000 -r 22050 - | sox -traw -r 22050 -es -b16 -c1 -V1 - -tmp3 - | socat -u - TCP-LISTEN:8080
```
Explained:
Record the FM frequency 91.0, with sample rate at 256k, resampling at 22050, format with sox to mp3 and set listener to port 8080 over TCP with socat (netcat)


### See more info
http://sdr.osmocom.org/trac/wiki/rtl-sdr
