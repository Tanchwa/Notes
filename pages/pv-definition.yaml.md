title:: pv-definition.yaml
```
apiVersion: v1
kind: PersistentVolume
metadata:
	name: pv-voll
spec:
	accessModes: #define how a volume is mounted on the host
    	- ReadOnlyMany #only one can be used
    	- ReadWriteOnce
        - ReadWriteMany
	capacity:
    	storage: 1Gi
    ~~hostPath:~~ #see Volume for why this should not be used on a prod env
    	~~path: /tmp/data~~
    awsElasticBLockStore: #better option 
    	volumeID: <volume-id>
        fsType: ext4
```

-
-