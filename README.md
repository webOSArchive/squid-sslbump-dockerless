(docker-less) squid-sslbump-rpi
======================
squid ssl proxy with icap without Docker on raspberry pi. Based on justinschw/docker-squid-sslbump-rpi, which was based on syakesaba/docker-sslbump-proxy. The Docker install didn't work on my Pi Zero, so here it is on the "bare metal."

Base Environment
======================
- ✅ Tested on: raspbian/stretch on Pi Zero and Zero W (it works, but it takes a *long* time to build!)
- ✅ Tested on: Ubuntu 18.04 on x64
- ❗Could not build on: Ubuntu 20.04
  - Requires `crypto` which can be had with `apt install libssl-dev`
  - Requires `libssl1.0-dev` which is no longer available. To fix:
      - add [this PPA](https://github.com/rvm/ubuntu_rvm): `sudo apt-add-repository -y ppa:rael-gc/rvm` 
      - `sudo apt install libssl1.0-dev`
  - Gave up and built on 18.04 and copied output folder to new machine 🤷‍♂️
- Instructions below require git (or you can download the files directly)
  - ```sudo apt-get update```
  - ```sudo apt-get install git```

Installation on Bare Metal
==========================
```sh
git clone https://github.com/codepoet80/squid-sslbump-rpi.git
cd squid-sslbump-rpi
chmod +x ./install.sh
sudo ./install.sh
```
- If you want, you can delete the ```squid-sslbump-rpi``` directory after install completes.

Installation via Docker
=======================
Community member Nomad84 has documented his approach to [Dockerizing this service here](https://github.com/h8pewou/legacy_webos).

Using the Proxy
======================
- Set your client's proxy server to the IP address of your Pi , and use port 3128 (for both HTTP and HTTPS)
- Export your fake root-cert from your Pi after installation and import it into your client web browsers.  
- (Root-cert path on the Pi after installation: ```/usr/local/squid/ssl/localCert.der```)
- OR, just access some HTTPS webpages from the client via the proxy and choose to "Trust Cert". 

Managing the Proxy
======================
- installed in: `/usr/local/squid`
- to see if its running, list processes: `ps -ef`
- to kill: `killall squid`
- to run in background: `/usr/local/squid/startsquid.sh &` 
  + the & makes it go background after its startup messages

Notes
======================
- This deliberately uses a root-cert with weak encryption to support old devices
- Make sure your is proxy safe.  
- To prevent unwanted use, firewalls or some squid-acls should be applied.  

License
======================
MIT License  
See: LICENSE

