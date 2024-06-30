# ECS Imagebuilder
This repository contains the sample artifacts to use the imagebuilder to build worker node image for ECS.
## Prerequisite
We will need docker runtime.
## Files
Below files are in the artifacts directory which you will need to edit to match your environment
 - vsphere.json - describe the vSphere environment used to build this image
 - kubernetes.json - specify the K8s version, etc that need to be installed into this image
 - packer-node.json - specify the additional ansible role to run by editing the following field ```"ansible_user_vars": "node_custom_roles_post='ecs intelgpu'",```
 - Directory
	 - ecs - this is the ansible role that will enable sudo for diag user 
	 - intelgpu - this is the ansible role that will install Intel drivers

## Example how to create an image
 ```bash
vm@dc1-dev-jumpbox:~/git/ecs-imagebuilder$ docker run -it --rm --net=host \
--env PACKER_FLAGS=-force \
-v $PWD/artifacts/vsphere.json:/home/imagebuilder/packer/ova/vsphere.json \
-v $PWD/artifacts/kubernetes.json:/home/imagebuilder/packer/config/kubernetes.json \
-v $PWD/artifacts/packer-node.json:/home/imagebuilder/packer/ova/packer-node.json \
-v $PWD/artifacts/ecs:/home/imagebuilder/ansible/roles/ecs \
-v $PWD/output:/home/imagebuilder/output \
registry.k8s.io/scl-image-builder/cluster-node-image-builder-amd64:v0.1.29 \
build-node-ova-vsphere-ubuntu-2204-efi
```

