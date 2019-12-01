---
layout: post
title: Updating an image used within k8s deployment
---

There are times when we update an image used in a deployment and we want our k8s deployment to use the latest image. We can achieve this in two ways, doing a rolling restart of the deployment or by specifying the deployment to run an image with a specific tag. Below the imperative commands that allow us to achieve this:

```bash
# Rolling update - the k8s deployment will update images to latest version
`kubectl rollout restart deployment/client-deployment` 

# Specify a k8s deployment to run an image with a specific tag
kubectl set image deployment/[deployment-name] [container-name]=[image-name:tag-name]` 
```

### References:
- Rolling update of deployment: https://github.com/kubernetes/kubernetes/issues/13488
- k8s updating a deployment: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#updating-a-deployment 
