# Common queries for troubleshooting, get information and management.

## Navigate from Kibana to Management -> Dev Tools

### Get information 
```
GET /_cat/indices

GET /_index_template/

GET _data_stream/

GET _ingest/pipeline/

GET _security/user/

```

### Delete indice
```
delete zeek-eve-2026.03.08
```

### Delete data stream
```
delete _data_stream/logs-wazuh.alerts-default
```

### Geo Point issues

**Check mappings**
```
get /_mapping/field/destination.geo.location?pretty
```
**Create template for geo-mapping (pfBlockerNG example)**
```
GET _component_template/pfblockerng-mappings
 {
   "template": {
     "mappings": {
       "properties": {
         "@timestamp":     { "type": "date" },
         "source": { "properties": {
           "ip":           { "type": "ip" },
           "port":         { "type": "integer" },
           "geo": { "properties": {
             "location":   { "type": "geo_point" },
             "country_iso_code": { "type": "keyword" },
             "city_name":  { "type": "keyword" }
           }},
           "as": { "properties": {
             "number":     { "type": "long" },
             "organization": { "properties": {
               "name":     { "type": "keyword" }
             }}
           }}
         }},
         "destination": { "properties": {
           "ip":           { "type": "ip" },
           "port":         { "type": "integer" },
           "domain":       { "type": "keyword" },
           "geo": { "properties": {
             "location":   { "type": "geo_point" },
             "country_iso_code": { "type": "keyword" },
             "city_name":  { "type": "keyword" }
           }}
         }},
         "network": { "properties": {
           "transport":    { "type": "keyword" },
           "type":         { "type": "keyword" },
           "direction":    { "type": "keyword" }
         }},
         "event": { "properties": {
           "action":       { "type": "keyword" },
           "category":     { "type": "keyword" },
           "type":         { "type": "keyword" },
           "kind":         { "type": "keyword" },
           "module":       { "type": "keyword" },
           "dataset":      { "type": "keyword" }
         }},
         "observer": { "properties": {
           "product":      { "type": "keyword" },
           "type":         { "type": "keyword" },
           "vendor":       { "type": "keyword" },
           "interface": { "properties": {
             "name":       { "type": "keyword" }
           }}
         }},
         "pfblocker": { "properties": {
           "type":         { "type": "keyword" },
           "group":        { "type": "keyword" },
           "feed":         { "type": "keyword" },
           "category":     { "type": "keyword" },
           "col_count":    { "type": "integer" }
         }},
         "tags":           { "type": "keyword" }
       }
     }
   }
 }

```

**Put to component template to index**
```
 PUT _index_template/pfblockerng
 {
   "index_patterns": ["logs-pfblockerng.*-*"],
   "data_stream": {},
   "composed_of": ["pfblockerng-mappings"],
   "priority": 200
 }
```

### Rollover Data
```
POST logs-pfsense.pfblockerng-default/_rollover
```


### Create user - for write/create
```
PUT _security/role/ingest_writer" 
{
  "cluster": ["monitor", "manage_pipeline"],
  "indices": [
    {
      "names": ["logs-*",zeek-*],
      "privileges": ["create_index","create","write","auto_configure"]
    }
  ]
}
```


