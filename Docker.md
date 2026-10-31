# دستورات سرور داکری:

## مشاهده آی پی‌ها و میزان ریسورس سرورهای کلاستر داکر:
```bash
for node in $(docker node ls --format '{{.Hostname}}'); do
  docker node inspect "$node" --format '{{.Description.Hostname}}|{{if and .ManagerStatus .ManagerStatus.Addr}}{{.ManagerStatus.Addr}}{{else}}{{.Status.Addr}}{{end}}|{{.Description.Resources.NanoCPUs}}|{{.Description.Resources.MemoryBytes}}'
done | awk -F'|' '{
  sub(/:[0-9]+$/, "", $2)
  printf "%-18s %-16s CPU: %-4.0f  RAM: %.0f GB\n", $1, $2, $3/1e9, $4/1024/1024/1024
}'
```
---
## تغییر پسورد الستیک:
```bash
docker exec -it infra_elasticsearch bin/elasticsearch-reset-password -u kibana_system -i --url http://localhost:9200
```
---
<br>
## لود کردن یکجا همه ایمیج‌ها:
```bash
for f in *.tar; do echo "Loading $f..."; docker load -i "$f"; done
```
---
