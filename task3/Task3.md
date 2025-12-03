# Arhitecture overview for monitoring

###### - Intall grafana , prometheus and loki on a machine.

###### - Install node exporter and promtail on the target machines.
- configure each machine to send long and metric 

For node exporter we export hardware metrics we don't need a configuration we just need to point prometheus to the machines with node exporter installed 
```sh 
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'node'
    static_configs:
      - targets:
          - '192.168.100.129:9100'
          - '192.168.100.158:9200'
          - '192.168.100.220:9100'
          - '192.168.100.221:9100'
          - '192.168.100.222:9100'
```


For promtail we conficure to send log file 
```
clients:
  - url: http://grafana-ip:3100/loki/api/v1/push
    tenant_id: luzilla
    external_labels:
      node: machien-ip

scrape_configs:
  - job_name: journal
    journal:
      path: /run/log/journal
      labels:
        job: journal
    relabel_configs:
      - source_labels: ['__journal__systemd_unit']
        target_label: 'unit'

  - job_name: varlogs
    static_configs:
      - targets:
          - localhost
        labels:
          job: varlogs
          __path__: /var/log/*.log
```