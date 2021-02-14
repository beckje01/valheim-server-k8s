# Valheim Server K8S Base Manifest


These are the base manifests to deploy [Valheim Server](https://www.valheimgame.com/) to k8s. Based on https://github.com/mbround18/valheim-docker


## Notes

Expected to used with Kustomizer. You can setup a local file and add ingress or other items needed.

## Example

Setting up a local deployment that also has a

kustomization.yaml
```
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization


namespace: valheim

commonLabels:
  app: valheim

resources:
  - github.com/beckje01/valheim-server-k8s/manifests?ref=v0.1
  - lb.yaml
```

lb.yaml
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: plantuml
  annotations:
    kubernetes.io/ingress.class: traefik
spec:
  rules:
  - host: plantuml.lab.example
    http:
      paths:
      - backend:
          serviceName: plantuml-server
          servicePort: 8080
```

Build:
```
kustomize build ./ | kubectl apply -f -
```