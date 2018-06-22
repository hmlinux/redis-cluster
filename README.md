## Redis集群(多主从节点)搭建和使用

### Version
    
    redis-4.0.10.tar.gz

### 软件安装

    tar -zxf redis-4.0.10.tar.gz
    cd redis-4.0.10/ && make && make install
    
#### Node1 (6381,6382)
    
    mkdir /opt/redis-cluster/{6381,6382}
    cd redis-4.0.10/
    scp -r redis-4.0.10/utils /opt/redis-cluster/6381/
    scp -r redis-4.0.10/utils /opt/redis-cluster/6382/
    cp redis-4.0.10/redis.conf /opt/redis-cluster/6381/utils/
    cp redis-4.0.10/redis.conf /opt/redis-cluster/6382/utils/ 
    
#### Node2 (6383,6384)
    
    mkdir /opt/redis-cluster/{6383,6384}
    scp -r redis-4.0.10/utils /opt/redis-cluster/6383/
    scp -r redis-4.0.10/utils /opt/redis-cluster/6384/
    cp redis-4.0.10/redis.conf /opt/redis-cluster/6383/utils/
    cp redis-4.0.10/redis.conf /opt/redis-cluster/6384/utils/
    
#### Node3 (6385,6386)
    
    mkdir /opt/redis-cluster/{6385,6386}
    scp -r redis-4.0.10/utils /opt/redis-cluster/6385/
    scp -r redis-4.0.10/utils /opt/redis-cluster/6386/
    cp redis-4.0.10/redis.conf /opt/redis-cluster/6385/utils/
    cp redis-4.0.10/redis.conf /opt/redis-cluster/6386/utils/
    
### Install
    
    cd redis-4.0.10/utils/
    ./install_server.sh
    
    /opt/redis-cluster/6384/redis.conf
    /opt/redis-cluster/6384/data
    
    /opt/redis-cluster/6385/redis.conf
    /opt/redis-cluster/6385/data
    
    /opt/redis-cluster/6386/redis.conf
    /opt/redis-cluster/6386/data
    
### Install Ruby
    
    yum install rubygems ruby-devel rubygems rpm-build
    yum install readline readline-devel openssl openssl-devel zlib zlib-devel
    cd ruby-2.5.1
    ./configure --prefix=/usr/local/ruby
    make
    make install
    
    export PATH=$PATH:/usr/local/ruby/bin
    
### gem install redis
    
    gem install redis

### Create redis masster-slave cluster
    
    cp redis-4.0.10/src/redis-trib.rb /usr/local/sbin/
    redis-trib.rb create --replicas 1 172.26.3.93:6381 172.26.3.93:6382 172.26.3.97:6383 172.26.3.97:6384 172.26.3.77:6385 172.26.3.77:6386
    
### Cluster command
    
    redis-cli -h 172.26.3.97 -p 6383 -c
    > cluster info
    > cluster nodes

