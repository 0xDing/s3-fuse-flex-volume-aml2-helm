# S3 FUSE Flex Volume Drivers - Amazon Linux 2 Helm Chart

This helm chart adds S3 FUSE flex volume drivers to your kubernetes cluster. Forked from [informatics-lab/s3-fuse-flex-volume](https://github.com/informatics-lab/s3-fuse-flex-volume) and add [Amazon Linux 2](https://aws.amazon.com/amazon-linux-2/) Support.

> **WARNING** This Chart only supports Amazon Linux 2, please use [informatics-lab/s3-fuse-flex-volume](https://github.com/informatics-lab/s3-fuse-flex-volume) for other distributions.

## Installation
```bash
git clone https://github.com/borisding1994/s3-fuse-flex-volume-aml2-helm
helm install --namespace kube-system --name s3-fuse-deployer ./s3-fuse-flex-volume-aml2-helm
```
This helm chart will create a `DaemonSet` which uses privileged containers to install the fuse dependancies and the flex drivers on the kubernetes nodes. You are then able to use the drivers in your pod definitions.

## Usage examples

### pysssix

Pysssix will mount "all" of S3 which is accessible to the authenticating user. A mount point is created which referrs to all of S3 and then you access objects at `/<mount>/<bucket>/<object>`.

With this driver you are limited to read only.

```yaml
volumes:
  - name: pysssix
    flexVolume:
      driver: "informaticslab/pysssix-flex-volume"
      options:
        # Optional
        subPath: "key/prefix"
containers:
  - name: mycontainer
    ...
    volumeMounts:
      - name: pysssix
        mountPath: /s3
```

### goofys

Goofys will only mount a specific bucket so you must provide the `bucket` option.

```yaml
volumes:
  - name: goofys-mybucket
    flexVolume:
      driver: "informaticslab/goofys-flex-volume"
      options:
        # Required
        bucket: "mybucket"
        # Optional
        dirMode: "0755"
        fileMode: "0644"
        subPath: "key/prefix"
containers:
  - name: mycontainer
    ...
    volumeMounts:
      - name: goofys-mybucket
        mountPath: /s3/mybucket
```

## License
CopyrightThis project is licensed under [GLWTPL](./LICENSE)