# MQTT-SSL-DOCKER
Mosquitto docker container with ssl enabled, after providing certs

## Clone this repository
```
git clone https://github.com/NikoHeikki/MQTT-SSL-DOCKER.git
```

## Create mosquitto file containing username and hashed password for authentication
```
mosquitto_passwd -c passwd sammy
```

## Certificates

### Create CA self-signed certificates using script 
```
./generate-self-signed-ca-certs.sh
```
### You can also use fully qualified certificates, get certificate from Let’s Encrypt(https://letsencrypt.org/) using Certbot(https://certbot.eff.org/)
Install Cerbot and install certs by following instructions and you should end up with certificate file and key. Requires to open port 80 for Let’s Encrypt to verify the domain name and do http challenge.


## Run docker-compose
```
docker-compose up -d
```

## Send message to broker and listen messages
If you used self signed certificates, you need to specify the ca.crt in mosquitto commands, this means if you have mqtt broker running in different machine/server, you need to download this ca.crt to your own device, in order for the ssl connection to work

Publish message:
```
mosquitto_pub -h <server name> -t "/foobar333/1/req" -m "testing"  -p 8883 --cafile ./ca.crt -u "<username>" -P "<password>"
```

Subscripe and listen to topic
```
mosquitto_sub -h <server name> -t "/foobar333/1/req" -v -u "<username>" -P "<password>" --cafile ./ca.crt 
```

