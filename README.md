# cwf-deployment

### Install Docker, Kubernetes


### Pre-requisites

To install the Carrier Wifi Gateway, there are three required files that are deployment-specific. These are described below:

1. rootCA.pem - This file should match the rootCA.pem of the Orchestrator that the Carrier Wifi Gateway will connect to.

2. control_proxy.yml - This file is used to configure the magmad and control_proxy services to point toward the appropriate Orchestrator. A sample configuration is provided below. The bootstrap_address, bootstrap_port, controller_address, and controller_port are the parameters that will likely need to be modified.

```bash
# nghttpx config will be generated here and used
nghttpx_config_location: /var/tmp/nghttpx.conf

# Location for certs
rootca_cert: /var/opt/magma/certs/rootCA.pem
gateway_cert: /var/opt/magma/certs/gateway.crt
gateway_key: /var/opt/magma/certs/gateway.key

# Listening port of the proxy for local services. The port would be closed
# for the rest of the world.
local_port: 8443

# Cloud address for reaching out to the cloud.
cloud_address: controller.magma.test
cloud_port: 443

bootstrap_address: bootstrapper-controller.magma.test
bootstrap_port: 443

# Option to use nghttpx for proxying. If disabled, the individual
# services would establish the TLS connections themselves.
proxy_cloud_connections: True

# Allows http_proxy usage if the environment variable is present
allow_http_proxy: True
```

3. .env - This file provides any deployment specific environment variables used in the docker-compose.yml of the Carrier Wifi Gateway. A sample configuration is provided below:

```bash
COMPOSE_PROJECT_NAME=cwf
DOCKER_REGISTRY=<registry>
DOCKER_USERNAME=<username>
DOCKER_PASSWORD=<password>
IMAGE_VERSION=latest
GIT_HASH=master

BUILD_CONTEXT=https://github.com/facebookincubator/magma.git#master

ROOTCA_PATH=/var/opt/magma/certs/rootCA.pem
CONTROL_PROXY_PATH=/etc/magma/control_proxy.yml
CONFIGS_TEMPLATES_PATH=/etc/magma/templates

CERTS_VOLUME=/var/opt/magma/certs
CONFIGS_OVERRIDE_VOLUME=/var/opt/magma/configs
CONFIGS_DEFAULT_VOLUME=/etc/magma
```


### Installation
The installation is done using the install_gateway.sh script located at magma/orc8r/tools/docker. To install, copy that file and the three files described above into a directory on the install host. Then

```bash
$ sudo ./install_gateway.sh cwag
```
After this completes, you should see: Installed successfully!!
