### Kong API Gateway
This documents covers these major topics
- [Setting up a Kong API Gateway backed by a 3 Node Cassandra
  configuration](production.md)
- [Recommended settings for running on t2.micro instances](production.md)
- [Configuring API routes in Kong](production.md)
- [Running Kong/Cassandra Instance via Docker](test.md)
- [Installing Kong/Cassandra via AWS CloudFormation template](test.md)

### Setting up Kong + 3 Node Cassandra configuration
- Create VPC with public + private subnet
  - Use the VPC Wizard
  - Select option 2 with both public and private subnets
  - Select an elastic IP for the NAT gateway
- Create an EC2 instance in the public subnet 
  - This will host the Kong Gateway
  - This can be a normal Amazon Linux t2.micro instance
  - This will also act as a bastion node to connect to the cassandra nodes
- Create 3 EC2 instances in the private subnet
  - This will host the Kong Gateway
  - This can be a normal Amazon Linux t2.micro instance
  - Open the following ports in the security group - 7000, 7001, 7199, 9042,
    9160
- Install Cassandra on all 3 instances
  - Connecting to instance on private subnet
    - Connect (via ssh) to the EC2 instance on the public subnet and from there connect to the instances on the private subnets through the private IP
    - https://aws.amazon.com/blogs/security/securely-connect-to-linux-instances-running-in-a-private-amazon-vpc/
  - Install java
    - It has been noticed that the oracle jre is more stable than the open-jdk
      java installation when running on t2.micro instances under memory
      pressure
    - Steps to install java
    ```
    cd ~
    wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" \
    "http://download.oracle.com/otn-pub/java/jdk/8u60-b27/jre-8u60-linux-x64.rpm"
    sudo yum localinstall jre-8u60-linux-x64.rpm

    # Check your installation with 
    java -version
    
    # Install JNA
    sudo yum install jna

    # Set JAVA_HOME
    export JAVA_HOME=/usr/java/jre1.8.0_60
    ```

    - Steps to install Cassandra
    ```
    # Download latest cassandra distribution
    curl -LO http://www-us.apache.org/dist/cassandra/3.10/apache-cassandra-3.10-bin.tar.gz
    tar -xvzf apache-cassandra-3.10-bin.tar.gz
    cd apache-cassandra-3.10
    ```

    - Optimize the Linux instance for running cassandra
      - https://docs.datastax.com/en/landing_page/doc/landing_page/recommendedSettings.html#recommendedSettings__jvm

  - Configure Cassandra Cluster
   - In all the cassandra installations modify the conf/cassandra.yaml file to
     reflect these settings
     ```
     cluster_name: 'Kong Cluster'
     seeds: "server_1_ip,server_3_ip,server_3_ip"
     listen_address: current_instance_ip
     rpc_address: current_instance_ip
     endpoint_snitch: GossipingPropertyFileSnitch
     ```

  - Run Cassandra
    - Run cassandra by executing the bin/cassandra file. Start the instances in
      the same order that you have specified in the seeds parameter

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
  --data 'upstream_url=http://<backend_ip>' \
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
  --data 'upstream_url=http://54.219.133.191:9090' \
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
  --data 'upstream_url=http://54.215.210.32:5000/starbucks' \
  --data 'uris=/example2/'

  curl -i -X PATCH \
  --url http://localhost:8001/apis/sunnyvale/ \
  --data 'name=sunnyvale' \
  --data 'upstream_url=ec2-54-67-122-67.us-west-1.compute.amazonaws.com' \
  --data 'uris=/'

  curl -i -X PATCH \
  --url http://localhost:8001/apis/palo-alto/ \
  --data 'name=palo-alto' \
  --data 'upstream_url=http://ec2-54-215-4-81.us-west-1.compute.amazonaws.com:5000/starbucks' \
  --data 'uris=/paloalto'

  http://54.183.210.6:9090/v3/starbucks/order/d3a7b9af-8ac3-4b45-bfb5-4d09fa3f400c
  

  curl -i -X PATCH \
  --url http://localhost:8001/apis/jsontest/ \
  --data 'name=jsontest' \
  --data 'upstream_url=http://54.215.197.21:9090/v3/starbucks' \
  --data 'uris=/jsontest'



  curl -i -X POST \
  --url http://localhost:8001/apis/ \
  --data 'name=elbtest' \
  --data 'upstream_url=http://ec2-54-70-172-67.us-west-2.compute.amazonaws.com:3000' \
  --data 'uris=/elbtest'
