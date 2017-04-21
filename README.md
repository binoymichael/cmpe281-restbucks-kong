### Steps to install the test Kong API Instance
- Launch AWS EC2 instance with Amazon Linux AMI 
- Install docker in the instance
  ```
  - sudo yum update -y
  - sudo yum install -y docker
  - sudo service docker start
  # Add ec2-user to docker group
  - sudo usermod -a -G docker ec2-user 
  - docker info
  ```
- Install cassandra and kong via docker
  ```
	docker run -d --name kong-database -p 9042:9042 cassandra:2.2
	docker run -d --name kong \
              --link kong-database:kong-database \
              -e "KONG_DATABASE=cassandra" \
              -e "KONG_CASSANDRA_CONTACT_POINTS=kong-database" \
              -e "KONG_PG_HOST=kong-database" \
              -p 8000:8000 \
              -p 8443:8443 \
              -p 8001:8001 \
              -p 7946:7946 \
              -p 7946:7946/udp \
              kong
  ```
- Check if kong apis are accessible
  ```
  curl http://localhost:8001/apis
  #=> {"data":[],"total":0}
  ```

- Configure routes by creating admin api objects
  
  curl -i -X POST \
  --url http://localhost:8001/apis/ \
  --data 'name=mountain-view' \
  --data 'upstream_url=http://www.mountainview.gov' \
  --data 'uris=/mountainview'

  curl -i -X POST \
  --url http://localhost:8001/apis/ \
  --data 'name=palo-alto' \
  --data 'upstream_url=http://www.cityofpaloalto.org' \
  --data 'uris=/paloalto'

  curl -i -X POST \
  --url http://localhost:8001/apis/ \
  --data 'name=jsontest' \
  --data 'upstream_url=http://ip.jsontest.com' \
  --data 'uris=/jsontest'
  

  "2abeff57-dab2-4fb9-87f7-3bcc8f11c014"
  
  curl -i \
  --url http://localhost:8001/apis/

  curl -i -X PATCH \
  --url http://localhost:8001/apis/mountain-view/ \
  --data 'name=mountain-view' \
  --data 'upstream_url=http://54.215.197.21:9090/v3/starbucks/' \
  --data 'uris=/mountainview'

  curl -i -X PATCH \
  --url http://localhost:8001/apis/jsontest/ \
  --data 'name=jsontest' \
  --data 'upstream_url=http://54.215.197.21:9090/v3/starbucks/orders' \
  --data 'uris=/jsontest'

  curl -i -X PATCH \
  --url http://localhost:8001/apis/jsontest/ \
  --data 'name=jsontest' \
  --data 'upstream_url=http://54.215.197.21:9090/v3/starbucks' \
  --data 'uris=/jsontest'


  curl -i -X PATCH \
  --url http://localhost:8001/apis/palo-alto/ \
  --data 'name=palo-alto' \
  --data 'upstream_url=http://54.183.245.6:5000/starbucks' \
  --data 'uris=/paloalto'

  http://54.183.210.6:9090/v3/starbucks/order/d3a7b9af-8ac3-4b45-bfb5-4d09fa3f400c
  

  curl -i -X PATCH \
  --url http://localhost:8001/apis/jsontest/ \
  --data 'name=jsontest' \
  --data 'upstream_url=http://54.215.197.21:9090/v3/starbucks' \
  --data 'uris=/jsontest'
