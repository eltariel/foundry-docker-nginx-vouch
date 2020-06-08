# FoundryVTT + docker + nginx + vouch-proxy + discord
## build containers
Edit foundry-vtt download url in Dockerfile
```
$ sudo docker -v build -t foundry-vtt .
```

## run container
```
$ sudo docker run -d --name=<name> -p <host-port>:30000 -v <fvtt data dir>:/home/foundry/data foundry-with-plutonium
```

# nginx Reverse Proxy
## Outline
## Site Template
## LetsEncrypt
```
$ sudo certbot certonly --webroot -d vouch.foundry.eltariel.com --email email.address@example.com -w /var/www/_letsencrypt -n --agree-tos --force-renewal
```

# OAuth2 via Dicsord
References: [nginx vouch setup][nginx-vouch], [using oidc with discord][discord-oidc]
```
$ sudo cp vouch-config.yml /etc/nginx/vouch/config.yml
$ sudo docker run -d --name vouch -v /etc/nginx/vouch:/config -p 9090:9090 voucher/vouch-proxy:alpine
```

# FoundryVTT login flow
## As written
```
GET /join
  -> form with dropdown of available users

Fill in details & submit:

POST /join multipart/form-data 
	"userid": "mlDlYBmmRo7vPZsJ"
	"password": ""
  -> JSON
	{
	  "response": "success" or "error",
	  "error": "something?",
	  "redirect": "/game"
	}

GET /game
```

## Inserting oauth2 mapping
landing page:
```
GET /
  -> Requires oauth2
  -> lists games available to you with allowed characters

Click a character name:
  -> if game server not running, launch it
  -> start at POST above
```

[nginx-vouch]: https://developer.okta.com/blog/2018/08/28/nginx-auth-request
[discord-oidc]: https://fusionauth.io/docs/v1/tech/identity-providers/openid-connect/discord
