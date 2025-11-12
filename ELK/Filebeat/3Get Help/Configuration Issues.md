## Journald input not working
#filebeat #k8s #daemonset #journald #logs 
When trying to configure journald input module to get information about service units from a VM, it can happen that it does not work if you are using k8s to deploy a filebeat deamonset.
Filebeat config example:
```yaml
filebeat:
  inputs:
    - type: journald
      id: services
      include_matches.match:
        - _SYSTEMD_UNIT=kubelet.service
        - _SYSTEMD_UNIT=containerd.service
        - _SYSTEMD_UNIT=node-problem-detector.service
      parsers:
      - syslog:
        log_errors: true
        add_error_key: true
      index: journal-aks-test
```
This might not work due to some folders missing in daemonset configuration. For this to work you MUST have the following volumes mapped:
```yaml
volumes:
  - configMap:
	  defaultMode: 416
	  name: filebeat-config
	name: config
  - hostPath:
	  path: /var/log
	  type: ""
	name: varlog
  - hostPath:
	  path: /run/log
	  type: ""
	name: runlog
  - hostPath:
	  path: /var/lib/filebeat-data
	  type: DirectoryOrCreate
	name: data
  - hostPath:
	  path: /run/systemd
	  type: ""
	name: runsystemd
  - hostPath:
	  path: /etc/machine-id
	  type: ""
	name: machine-id
```
Normally the ones that are missing are runsystemd, machine-id and runlog. Be sure to configure those in mountVolumes:
```yaml
volumeMounts:
- mountPath: /etc/filebeat.yml
  name: config
  readOnly: true
  subPath: filebeat.yml
- mountPath: /usr/share/filebeat/data
  name: data
- mountPath: /run/log
  name: runlog
  readOnly: true
- mountPath: /var/log
  name: varlog
  readOnly: true
- mountPath: /run/systemd
  name: runsystemd
  readOnly: true
- mountPath: /etc/machine-id
  name: machine-id
  readOnly: true
```

