### Adding Kong routes
  - Connect to Kong Instance and run the following commands to add different
    routes

### Sunnyvale Route
  ```
  curl -i -X POST \
  --url http://localhost:8001/apis/ \
  --data 'name=sunnyvale' \
  --data 'upstream_url=http://ec2-54-67-122-67.us-west-1.compute.amazonaws.com' \
  --data 'uris=/sunnyvale'
  ```

### Mountainview Route
  ```
  curl -i -X POST \
  --url http://localhost:8001/apis/ \
  --data 'name=mountainview' \
  --data 'upstream_url=http://54.219.133.191:9090' \
  --data 'uris=/mountainview'
  ```

### PaloAlto Route
  ```
  curl -i -X POST \
  --url http://localhost:8001/apis/ \
  --data 'name=palo-alto' \
  --data 'upstream_url=http://54.215.197.21:9090/v3/starbucks' \
  --data 'uris=/paloalto'
  ```

