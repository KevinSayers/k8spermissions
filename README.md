# k8spermissions

Simple pods for testing user permissions in Kubernetes. Also a simple demo of writing to a persistent volume

* `userpod.yml` is meant to generate a file as a specified user not as root
* `pod.yml` generates a file as root
* `pv.yml` persistent volume for use with NFS 

Outstanding issues:
* runAsGroup still results in a file group ownership as root

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-user
spec:  # specification of the pod's contents
  restartPolicy: Never
  volumes: 
  - name: mystorage
    persistentVolumeClaim:
      claimName: nfs
  containers:
  - name: hello
    image: "ubuntu:14.04"
    command: ["/bin/bash"]
    args: ["-c","echo hello world again > /data/kevinowned/useroutput.txt"]
    volumeMounts:
    - mountPath: "/data"
      name: mystorage 
    securityContext:
      runAsUser: 5000
      runAsGroup: 5000
      allowPrivilegeEscalation: false
      
  securityContext:
    runAsUser: 5000
    fsGroup: 5000
```

