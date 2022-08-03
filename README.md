[![FINOS - Archived](https://cdn.jsdelivr.net/gh/finos/contrib-toolbox@master/images/badge-archived.svg)](https://community.finos.org/docs/governance/Software-Projects/stages/archived)


**NOTE!** This project is archived due to lack of activity; you can still consume this software, although not advised, as it's not been maintained since November 2021. If you're interested to restore activity on this repository, please email [help@finos.org](mailto:help@finos.org)

# Legend Integration Helm

This intergration is a helm chart to deploy Legend Studio to Kubernetes.

## Installation

Kubernetes CNCF Cluster <=1.16 (K3s/RKE/RKE2):

1. Clone Helm Chart Repo
```
git clone --depth 1 https://github.com/finos/legend-integration-helm.git
```

2. Gitlab OAuth config setup

If you don't already have a Gitlab OAuth application, first, navigate to `User Settings > Applications` and create an application with the following parameters:

- Name: Legend Demo
- Redirect URI:
```
http://{HOST IP}.sslip.io/callback
http://{HOST IP}.sslip.io/api/auth/callback
http://{HOST IP}.sslip.io/api/pac4j/login/callback
http://{HOST IP}.sslip.io/studio/log.in/callback
```
- Enable the "Confidential" check box
- Enable these scopes: openid, profile, api
- Finally, "Save Application"

3. Install helm chart
```
helm install legend ./legend/installers/helm/ --set env.LEGEND_HOST="{HOST IP}.sslip.io" \
                                              --set env.GITLAB_OAUTH_CLIENT_ID=""   \
                                              --set env.GITLAB_OAUTH_SECRET=""      \
                                              --namespace legend                    \
                                              --create-namespace
```
3. Check pods are in running/ready state
```
kubectl get pods -n legend
```

4. Browse to http://{HOST IP}.sslip.io/studio

## SSL Enable
Same steps as 2. but with https callback url:
- Redirect URI:
```
https://{DNS Hostname}/callback
https://{DNS Hostname}/api/auth/callback
https://{DNS Hostname}/api/pac4j/login/callback
https://{DNS Hostname}/studio/log.in/callback
```

Create Ingress TLS Secret
```
kubectl create secret tls tls-legend --cert=fullchain.pem  --key=key.pem
```

Install helm chart
```
helm install legend ./legend/installers/helm/ --set env.LEGEND_HOST="{DNS Hostname}" \
                                              --set env.GITLAB_OAUTH_CLIENT_ID="" \
                                              --set env.GITLAB_OAUTH_SECRET=""    \
                                              --set env.HTTP_MODE="https"         \
                                              --set env.TLS_SECRET="tls-legend"   \
                                              --namespace legend                  \
                                              --create-namespace
```

Browse to `https://{DNS Hostname}/studio`

## Contributing

To learn about contributing to Legend, see the [CONTRIBUTING.md](CONTRIBUTING.md) file or the ["contribute to Legend"](https://legend.finos.org/docs/getting-started/contribute-to-legend) section of the Legend documentation site.

## License

Copyright 2021 Suse

Distributed under the [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0).

SPDX-License-Identifier: [Apache-2.0](https://spdx.org/licenses/Apache-2.0)
