https://us-west-1.console.aws.amazon.com/cloudformation/home?region=us-west-1#/stacks/new?stackName=kong-elb-cassandra-hvm&templateURL=https:%2F%2Fs3.amazonaws.com%2Fkong-cf-templates%2Flatest%2Fkong-elb-cassandra-new-vpc-optional-hvm.template

Create VPC with public + private subnet
Create a bastion node in public subnet
Create 3 nodes in private subnet


https://aws.amazon.com/blogs/security/securely-connect-to-linux-instances-running-in-a-private-amazon-vpc/

Install cassandra on an instance
  Install java
  - sudo yum install java-1.8.0-openjdk-devel
  sudo /usr/sbin/alternatives --config java

https://docs.datastax.com/en/cassandra/3.0/cassandra/install/installRHEL.html


http://datascale.io/how-to-create-a-cassandra-cluster-in-aws/
http://datascale.io/how-to-create-a-cassandra-cluster-in-aws-part-2/
https://www.digitalocean.com/community/tutorials/how-to-run-a-multi-node-cluster-database-with-cassandra-on-ubuntu-14-04

10.0.1.134,10.0.1.247,10.0.1.81

10.0.1.97,10.0.1.168,10.0.1.178


"10.0.1.134:9042"
"10.0.1.247:9042"
"10.0.1.81:9042"

sudo service cassandra stop
sudo rm -rf /var/lib/cassandra/data/system/*



https://github.com/Mashape/kong/releases/download/0.10.1/kong-0.10.1.aws.rpm
sudo yum install epel-release
sudo yum install kong-0.10.1.aws.rpm --nogpgcheck




sudo cassandra -R &
