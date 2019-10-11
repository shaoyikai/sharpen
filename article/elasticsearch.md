### 常用命令实例

> curl -X GET "http://localhost:9200/_cat/health?v" 
```
epoch      timestamp cluster       status node.total node.data shards pri relo init unassign pending_tasks max_task_wait_time active_shards_percent
1570676918 03:08:38  elasticsearch yellow          1         1      7   7    0    0        7             0                  -                 50.0%
```

> curl -X GET "http://localhost:9200/_cat/nodes?v" 
```
ip        heap.percent ram.percent cpu load_1m load_5m load_15m node.role master name
127.0.0.1           26          42   1    0.00    0.00     0.00 mdi       *      NOC_2_MASTER
```

> curl -X GET "http://localhost:9200/_cat/indices?v" 
```
health status index          uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   real_warn      v-dLLzkLT1q19DVrk1yJkw   1   1     484359            0       60mb           60mb
yellow open   zb_tms         mOYLh28pQyKRHDa5m-mZDA   1   1     450507            0       30mb           30mb
yellow open   zb_ticket      FNW0y4hzRFC0K6ujpJlyQA   1   1     571755            0       38mb           38mb
yellow open   zb_tms_unit    XKqMyL9TT120cQPdhTz3qg   1   1    3925090            0    272.4mb        272.4mb
yellow open   zb_ticket_unit mGEyVdjXQlGV9vrI8DzzfA   1   1    1781609            0    135.6mb        135.6mb
yellow open   real_normal    zwRSc3R3T4KpqJUi8yATYA   1   1    2850248            0    365.7mb        365.7mb
yellow open   real_earlywarn ex49W2QARwq_S5-2ezZaoA   1   1     139624            0       17mb           17mb
```

> select * from real_warn where device_model='SRX-R515-Dual' and cinema_code='36031401' and hall_code='03' and msg_type<>0 and (date(collect_time) between "2019-09-01" and "2019-09-25");
```
curl -X GET "http://localhost:9200/real_warn/_search" -H "Content-Type: application/json" -d '{
   "query" : {
        "bool" : {
            "must" : [
                { "match": { "device_model": "SRX-R515-Dual" }}, 
                { "match": {"cinema_code":   "36031401"}},
                { "match": {"hall_code":   "03"}}
            ],
            "must_not" :  { "match": {"@msg_type":   0}},
            "filter" : [
                { "range": {
                        "collect_time": {
                            "gte": "2019-09-01T00:00:00.000Z",
                            "lte": "2019-09-25T00:00:00.000Z",
                            "format":"strict_date_optional_time",
                            "time_zone":"GMT+8"
                        }
                    }
                }
            ]
        }
   },
   "size":1000
}'
```