## Openshift Login and Configuration

| Command                                                                      | Description                                                             |
| ---------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| oc login https://192.168.99.100:8443 -u developer -p developer               | login with a user                                                       |
| oc login -u system:admin                                                     | login as system admin                                                   |
| oc whoami                                                                    | User Information                                                        |
| oc config view                                                               | View your configuration                                                 |
| oc config set-context `oc config current-context` --namespace=<project_name> | Update the current context to have users login to the desired namespace |

## Basic Commands

| Command                                                                                                   | Description                                                            |
| --------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| oc new-app https://github.com/name/project --template=<template>                                          | Use specific template                                                  |
| oc new-app --name=html-dev nginx:1.10~https://github.com/joe-speedboat/openshift.html.devops.git#mybranch | New app from a different branch                                        |
| oc create -f myobject.yaml -n <myproject>                                                                 | Create objects from a file:                                            |
| oc apply -f myobject.yaml -n <myproject>                                                                  | Create or merge objects from file                                      |
| oc patch svc mysvc --type merge --patch '{"spec":{"ports":[{"port": 8080, "targetPort": 5000 }]}}'        | Update existing object                                                 |
| watch oc get pods                                                                                         | Monitor Pod status                                                     |
| oc get pods --show-labels                                                                                 | show labels                                                            |
| oc get pods -o wide                                                                                       | Gather information on a project's pod deployment with node information |
| oc get pods --show-all=false                                                                              | Hide inactive Pods                                                     |
| oc get all,secret,configmap                                                                               | Display all resources                                                  |
| oc get -n openshift-console route console                                                                 | Get the Openshift Console Address                                      |
| POD=\$(oc get pods -l app=myapp -o name) \| oc rsh -n \$POD                                               | Get the Pod name from the Selector and rsh in it                       |
| oc exec $POD \$COMMAND                                                                                    | exec single command in pod                                             |
| oc rsync myrunning-pod-2:/tmp/LogginData_20180717220510.json .                                            | Copy file from myrunning-pod-2 path in the current location            |
| oc explain dc                                                                                             | Read resource schema doc                                               |

## Image Streams

| Command                                                                                                               | Description                               |
| --------------------------------------------------------------------------------------------------------------------- | ----------------------------------------- |
| oc get is -n openshift                                                                                                | List available IS for openshift project   |
| oc import-image --from=registry.access.redhat.com/jboss-amq-6/amq62-openshift -n openshift jboss-amq-62:1.3 --confirm | Import an image from an external registry |
| oc new-app --list                                                                                                     | List available IS and templates           |

## Create app from a Project with Dockerfile

```
oc new-build --binary --name=mywildfly -l app=mywildfly
oc patch bc/mywildfly -p '{"spec":{"strategy":{"dockerStrategy":{"dockerfilePath":"Dockerfile"}}}}'
oc start-build mywildfly --from-dir=. --follow
oc new-app --image-stream=mywildfly
oc expose svc/mywildfly
```

## Nodes

| Command                                                                                                                                 | Description                                    |
| --------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| oc get nodes                                                                                                                            | Get Nodes lits                                 |
| oc get pods -o wide                                                                                                                     | Check on which Node your Pods are running      |
| oc patch dc myapp -p '{"spec":{"template":{"spec":{"nodeSelector":{"kubernetes.io/hostname": "ip-10-0-0-74.acme.compute.internal"}}}}}' | Schedule an application to run on another Node |
| oc adm manage-node node1.local --list-pods                                                                                              | List all pods which are running on a Node      |
| oc label node node1.local mylabel=myvalue                                                                                               | Add a label to a Node                          |
| oc label node node1.local mylabel-                                                                                                      | Remove a label from a Node                     |

## Build

| Command                                          | Description                       |
| ------------------------------------------------ | --------------------------------- |
| oc start-build ruby-ex                           | Manual build from source          |
| oc cancel-build <build_name>                     | Stop a build that is in progress  |
| oc set env bc/my-build-name BUILD_LOGLEVEL=[1-5] | Changing the log level of a build |

## Deployment

| Command                                                                                           | Description                                             |
| ------------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| oc rollout latest ruby-ex                                                                         | Manual deployment                                       |
| oc rollout pause dc \$DEPLOYMENT                                                                  | Pause automatic deployment rollout                      |
| oc rollout resume dc \$DEPLOYMENT                                                                 | Resume automatic deployment rollout                     |
| oc set resources deployment nginx --limits=cpu=200m,memory=512Mi --requests=cpu=100m,memory=256Mi | Define resource requests and limits in DeploymentConfig |
| oc set probe dc/nginx --readiness --get-url=http://:8080/healthz --initial-delay-seconds=10       | Define readinessProve in DeploymentConfig               |
| oc set probe dc/nginx --liveness --get-url=http://:8080/healthz --initial-delay-seconds=10        | Define livenessProve in DeploymentConfig                |
| oc autoscale dc \$DC_NAME --max=4 --cpu-percent=10                                                | Define Horizontal Pod Autoscaler (hpa)                  |

## Routes

| Command                                                     | Description                   |
| ----------------------------------------------------------- | ----------------------------- |
| oc expose service ruby-ex                                   | Create route                  |
| oc get route my-route -o jsonpath --template="{.spec.host}" | Read the Route Host attribute |

## Services

| Command                                                 | Description                                                                                      |
| ------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| oc idle ruby-ex                                         | Make a service idle. When the service is next accessed will automatically boot up the pods again |
| oc get rook-ceph-mon-a --template='{{.spec.clusterIP}}' | Read a Service IP                                                                                |

## Clean up resources

| Command                                                            | Description                                                                                                                                                 |
| ------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| oc delete all --all                                                | Delete all resources                                                                                                                                        |
| oc delete -l app=ruby-ex                                           | Delete resources for one specific app                                                                                                                       |
| oc delete all -l app=ruby-ex                                       | Delete resources for one specific app                                                                                                                       |
| oc adm prune images --keep-tag-revisions=3 --keep-younger-than=60m | CleanUp old docker images on nodes. Keeping up to three tag revisions 1, and keeping resources (images, image streams and pods) younger than sixty minutes: |
| oc adm prune images --prune-over-size-limit                        | Pruning every image that exceeds defined limits                                                                                                             |

## Troubleshooting

| Command                                              | Description                                      |
| ---------------------------------------------------- | ------------------------------------------------ |
| oc status                                            | Check status of current project                  |
| oc get events --sort-by='{.lastTimestamp}'           | Get events for a project                         |
| oc logs myrunning-pod-2-fdthn                        | get the logs of the myrunning-pod-2-fdthn pod    |
| oc logs -f myrunning-pod-2-fdthn                     | follow the logs of the myrunning-pod-2-fdthn pod |
| oc logs myrunning-pod-2-fdthn --tail=50              | tail the logs of the myrunning-pod-2-fdthn pod   |
| oc logs docker-registry-n-{xxxxx} -n default \| less | Check the integrated Docker registry logs:       |
| oc adm diagnostics                                   | run cluster diagnostics                          |

## Security

| Command                                                                                                 | Description                                 |
| ------------------------------------------------------------------------------------------------------- | ------------------------------------------- |
| oc create secret generic oia-secret --from-literal=username=myuser --from-literal=password=mypassword   | Create a secret from the CLI and            |
| oc set volumes dc/myapp --add --name=secret-volume --mount-path=/opt/app-root/ --secret-name=oia-secret | mount it as a volume to a deployment config |
