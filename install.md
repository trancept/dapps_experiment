# Install decentralized tools from scratch in a Linux server to go onLine

SSH to your server.
Keep it up-to-date :
```
 sudo apt-get update
 sudo apt-get upgrade
```
Install TMUX to be able to keep your command line running after you close connection. Not the cleanest way to do it but the easiest.
```
sudo apt-get install tmux git
```

### IPFS
Check last version here : https://dist.ipfs.io/#go-ipfs
```
sudo apt-get update
sudo apt-get install golang-go -y
wget https://dist.ipfs.io/go-ipfs/v0.4.14/go-ipfs_v0.4.14_linux-amd64.tar.gz -O go-ipfs.tar.gz
tar xvfz go-ipfs.tar.gz
sudo cp go-ipfs/ipfs /usr/local/bin/ipfs
ipfs init
tmux
ipfs daemon
```
=> Close the terminal and IPFS daemon will keep running

### Replicate IPFS

These commands will copy all the files existing on the source node to the destination node. So you could have many node running on the planet to be always online.

On the source node :
```
[hacker@source ~]$ ipfs refs local > all_the_ipfs_refs
```
Now, on the destination node
```
[hacker@dest ~]$ for ref in `cat all_the_ipfs_refs`; do ipfs block get $ref > /dev/null; done
```


## ETH testnet
```
  sudo apt-get install software-properties-common
  sudo add-apt-repository -y ppa:ethereum/ethereum
  sudo apt-get update
  sudo apt-get install ethereum
  tmux
  geth --testnet --fast --rpc --rpcapi eth,net,web3,personal --rpcport 9546
```


## NodeJS and Ganache-cli
```
  sudo apt-get install -y build-essential
  curl -sL https://deb.nodesource.com/setup_9.x | sudo -E bash -
  sudo apt-get install -y nodejs
  sudo npm install -g ganache-cli
```

## Install your project and run it



## HTTPS
If you want HTTPS, use Let's Encrypt certbot. But I won't cover it here.


## Reverse proxy with NGINX

It allow you to make many HTTP servers running locally on different port avaliable on the port 80 via multiple domain pointing to the same IP.
It is the easiest way I find to expose services like node and GETH without open too many port.
The drawback is that you have to create domains.

```
  sudo apt-get install nginx
  sudo  nano /etc/nginx/sites-enabled/default 
```
 
```javascript
server {
    listen 80;
    server_name geth.yourdomain.fr;
    location / {
        proxy_pass http://localhost:8545;
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
server {
    listen 80;
    server_name yourapp.yourdomain.fr;
    location / {
        proxy_pass http://localhost:3000;
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```
Run NginX
```
  sudo service nginx start
```

## Open port in firewall

- Open tcp:22 for SSH
- Open tcp:443 and tcp:80 for HTTP and HTTPS
- Open tcp:5001 and tcp:4001 For IPFS 

And that's it you good to go !



