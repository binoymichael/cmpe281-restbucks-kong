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
  --data 'name=sunnyvale' \
  --data 'upstream_url=http://sunnyvale.ca.gov' \
  --data 'uris=/sunnyvale'
  
  
  
