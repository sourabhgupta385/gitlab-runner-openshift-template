### Description

This repository contains template for deploying GitLab runner on OpenShift.

This template creates a GitLab Multi-Runner manager in your OpenShift project.

It defaults to executing GitLab CI jobs using the multi-runner's kubernetes
executor.

By deploying the multi-runner in OpenShift, it is able to automatically detect
and use the OpenShift kubernetes config to create new pods for CI job execution.

#### Service Account User

The `runner-template.yaml` file creates a service user for the runner
application. The service user's name is `gitlab-runner-user`.

This user requires the ability to run as privileged container in order to support
dind builds. In order to run the Runner application. In OpenShift this means an
administrator needs to add the user to the `privileged` security context.

They can do this by running:

```
$ oc adm policy add-scc-to-user privileged system:serviceaccount:<project-name>:gitlab-runner-user
```

The Runner application deployment will fail to successfully start until this has
been done.


### Setup

1. Add the policy as described in previous section.

2. Log in to OpenShift cluster

```
$ oc login <server-url> --token=<token>
```

3. Install the template

```
$ oc process -f runner-template.yml -p GITLAB_URL=<gitlab-url> -p REGISTRATION_TOKEN=<registration-token> | oc create -f -
```
