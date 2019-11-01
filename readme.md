# Prisma: Helm Chart

A Helm Chart for easily installing Prisma with external mongoDB on a Kubernetes cluster and expose to a domain.

## Prerequisites

* Installed [helm CLI](https://docs.helm.sh/using_helm/#installing-helm)
* Kubernetes cluster

### Configuration

The chart comes with a default configuration you can find in the `prisma/values.yaml` file. 

All you have to change are these values inside `prisma/values.yaml`
```javascript
service:
  mongoUri: mongodb+srv://username:password@mongodb-url/admin
  prismaSecret: my-awesome-secret-change-me

hosts:
 - host: your.domain.com

tls:
 - secretName: prisma-change-me-tls
   hosts:
   - your.domain.com
```


### Pre Deployment

Dry run the install and check all values are passed correctly via:

```sh
helm install --dry-run --debug --name prisma --namespace prisma ./prisma/
```

### Deployment

After you change the values on `prisma/values.yaml` you can deploy the Prisma server via:

```sh
helm install --name prisma --namespace prisma  ./prisma/
```

Awesome, you have successfully installed a Prisma server via Helm! You can check if everything went smoothly by executing:

```
kubectl get pods,services,configmaps,ingress --namespace prisma
```

You should see that the Prisma pod is running, an internal load balancer, the respective config map has been created and the ingress that expose the Prisma server on the domain you supplied on the `values.yaml` file.

### Update

Change values inside prisma folder and then execute

```javascript
helm upgrade --install prisma ./prisma/
```

## Uninstalling the Prisma server

```sh
helm del --purge prisma
```