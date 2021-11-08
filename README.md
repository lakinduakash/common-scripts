## New VM - Install zsh
```
sudo apt install zsh
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
sed -i '/ZSH_THEME=/c ZSH_THEME="crunch"' .zshrc
sudo usermod --shell $(which zsh) ${USER}
```

Extra theme
`sed -i '/ZSH_THEME=/c ZSH_THEME="geoffgarside"' .zshrc`


## Install docker

```

# Start docker installation

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88

sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose

sudo usermod -aG docker ${USER}
```
## Install nginx

```
sudo apt install nginx certbot python3-certbot-nginx
sudo certbot --nginx -d <domain>
```

### Sample config

```
server {

    listen 80;
    listen [::]:80;

    server_name chatbot.revokex.com;

    client_max_body_size 10M;
    
    #root /var/www/example.com;
    #index index.html;

    # We need to add specific headers so the websockets can be set up through the reverse proxy
    location /socket.io/ {
                proxy_pass http://localhost:3000/socket.io/;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection "Upgrade";
        }

    location / {
                #try_files $uri $uri/ =404;
                proxy_pass http://localhost:3000;
        }

}

```

## Tmux

Deattach from seesion -> `Ctrl + b d`

Attach session 0 -> `tmux attach-session -t 0`
