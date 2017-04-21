- Install Java 8
Install Oracle java instead of Open JDK

1. Download the JRE rpm file
cd ~
wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" \
"http://download.oracle.com/otn-pub/java/jdk/8u60-b27/jre-8u60-linux-x64.rpm"

2. Install the package
sudo yum localinstall jre-8u60-linux-x64.rpm

3. Check java version
java -version

java version "1.8.0_60"
Java(TM) SE Runtime Environment (build 1.8.0_60-b27)
Java HotSpot(TM) 64-Bit Server VM (build 25.60-b23, mixed mode)

sudo yum install jna
http://stackoverflow.com/questions/28493987/why-do-we-need-to-install-jna-on-cassandra-ring

Set java home

install apache

https://docs.datastax.com/en/landing_page/doc/landing_page/recommendedSettings.html#recommendedSettings__jvm


https://bigdatadeveloptutorial.files.wordpress.com/2013/12/datastax_cassandra_cluster_setup_with_opscenter_on_amazon_default_vpc.pdf
