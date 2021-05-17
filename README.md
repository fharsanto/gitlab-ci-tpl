# CI/CD Templates

env `APP_PREFIX` is prefix for application, sample value `SPK_`

env `DOCKER_AUTH_CONFIG` must be defined for credentials hub.spesolution.com, sample value:
```
{
	"auths": {
		"hub.spesolution.com": {
			"auth": "base64-credentials"
		}
	},
	"HttpHeaders": {
		"User-Agent": "Docker-Client/19.03.5 (linux)"
	}
}
```
env `DEPLOY_SSH_KEY` is private key SSH for specific environment (development, beta or production), sample value:
```
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAACFwAAAAdzc2gtcn
NhAAAAAwEAAQAAAgEAwiyUkW/
-----END OPENSSH PRIVATE KEY-----
```

env `DEPLOY_SSH_CONFIG` is ssh config for access the environment server, sample value:
```
Host gateway
HostName 8.19.87.20
Port 7334
User userName

Host serverName
HostName 192.168.50.36
Port 22
User userName
ProxyCommand ssh gateway -W %h:%p

Host *
StrictHostKeyChecking no
```

env `DEPLOY_SSH_SERVER` is name of environment server, see sample value of DEPLOY_SSH_CONFIG, sample value:
`serverName`

Sample usages

.gitlab-ci.yml
```
stages:
  - build
  - deploy

include:
  - project: 'feri/ci'
    file: '/docker.build.yml'
  - project: 'feri/ci'
    file: '/docker.deploy.yml'
```

Sample usages with extends
```
stages:
  - my-build

include:
  - project: 'feri/ci'
    file: '/docker.build.yml'

custom-build:
    extends: docker_build
    environment:
        name: custom-env
    stage: my-build
```