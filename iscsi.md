Following https://github.com/Microsoft/K8s-Storage-Plugins/blob/b2dfff4725/flexvolume/windows/sample_yamls/readme.md.

# Prerequisites

This example expects there to be a working iSCSI target to connect to. If there isn't one in place then it is possible to setup a software version on Linux by following these guides
- [Setup a iSCSI target on Fedora](https://www.server-world.info/en/note?os=Fedora_21&p=iscsi)
- [Install the iSCSI initiator on Fedora](https://www.server-world.info/en/note?os=Fedora_21&p=iscsi&f=2)


# Create a Windows workload

**Import Yaml** from **Default** _Project Page_:

- Create `Secret` to record the account for accessing iSCSI application, the type should be `microsoft.com/iscsi.cmd`

```
apiVersion: v1
kind: Secret
metadata:
  name: iscsi-secret
data:
  # base64 encode password, for example: username
  node.session.auth.password: c3Ryb25ncGFzc3dvcmQ=
  # base64 encode username, for example: strongpassword
  node.session.auth.username: dXNlcm5hbWU=
type: microsoft.com/iscsi.cmd

```

- Replace the `ISCSI_SERVER_HOST_IP` by the iSCSI workload host IP, Replace the `ISCSI_TARGET_IQN` by the iSCSI IQN
```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  labels:
    app: windows-iscsi-test
  name: windows-iscsi-test
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: windows-iscsi-test
      name: windows-iscsi-test
    spec:
      affinity:
        podAntiAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
          - labelSelector:
              matchExpressions:
              - key: app
                operator: In
                values:
                - windows-iscsi-test
            topologyKey: "kubernetes.io/hostname"
      nodeSelector:
        beta.kubernetes.io/os: windows
      containers:
      - name: windowsiscsitest
        image: mcr.microsoft.com/powershell:nanoserver-1809
        command:
        - pwsh.exe
        args:
        - -NoLogo
        - -NonInteractive
        - -Command
        - '&{$hs=hostname; echo $hs >> c:/mnt/hosts; do { echo "==========="; cat c:/mnt/hosts; echo "==========="; echo ""; start-sleep -s 10; } while($True)}'
        volumeMounts:
        - name: iscsi-volume
          mountPath: c:/mnt
      volumes:
      - name: iscsi-volume
        flexVolume:
          driver: "microsoft.com/iscsi.cmd"
          fsType: "NTFS"
          secretRef:
            name: "iscsi-secret"
          readOnly: false
          options:
            chapAuthDiscovery:  "false"
            chapAuthSession:  "true"
            targetPortal:  "ISCSI_SERVER_HOST_IP"
            iqn:  "ISCSI_TARGET_IQN"
            lun:  "0"
            authType:  "ONEWAYCHAP"
```

