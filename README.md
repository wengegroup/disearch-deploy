    
## 产品概述
**分布式搜索引擎DiSearch，基于闻海5000亿+数据运维管理中孵化而出，稳定、高效的搜索引擎，100%兼容Elasticsearch开源的分布式检索、分析套件。**
**提供Elasticsearch、Kibana等开源的产品服务能力，提供统一组件一体化部署，面向结构化、非结构化数据，构建统一数据索引，提供更快更准的数据搜索服务。**

开源Elasticsearch是一个基于Lucene的实时分布式的搜索与分析引擎，是遵从Apache开源条款的一款开源产品，是当前主流的企业级搜索引擎。作为一款基于RESTful API的分布式服务，Elasticsearch可以快速地、近乎于准实时地存储、查询和分析超大数据集，通常被用来作为构建复杂查询特性和需求强大应用的基础引擎或技术。


## 产品安装部署
### 一、安装目录说明
    “.”表示解压安装包的当前目录，例如：
    #tar -xvf disearch-v1.0.0-7.11.1.tat.gz
    #chmod 755 -R disearch-v1.0.0-7.11.1
    #cd disearch-v1.0.0-7.11.1

1、安装目录说明
```
.
├── bin                快速搭建ES集群所维护的脚本
├── conf               配置信息
├── install
├── package.sh
├── pre_commit.sh
├── README
├── release-notes.md   发布信息
├── root               存放了ES的安装包。后续搭建集群，生成的节点，也在此目录。
├── rsa
├── run
├── security-plugin
├── upgrade
└── VERSION

```

2、配置信息目录介绍 conf
```
./conf
├── config.ini        搭建集群所需要的配置信息
├── elasticsearch.ini 配置临时目录
└── es_config.ini     es节点所需配置信息
```


3、集群安装后的root目录展示，运行deploy_es.sh、config_es.sh脚本后，会自动修改以下配置参数
```
./root
├── bin
├── config
├── elasticsearch-sql-7.11.1.0
├── elasticsearch-sql-7.11.1.0.tar                                               
├── ES0   --Elasticsearch节点0
├── ES1   --Elasticsearch节点1
├── es-monitor.sh
├── jdk
├── lib
├── LICENSE.txt
├── logs
├── modules
├── NOTICE.txt
├── plugins
├── README.asciidoc
├── restart.sh
├── start.sh
└── stop.sh
```

4、重要脚本说明 安装包解压后的最外层./bin目录，用来搭建和启动集群。
```
./bin
├── deploy_es.sh  该脚本，主要修改集群启动的必要操作系统参数，修改linux内核参数。其中操作系统参数如果不做修改，则可能导致集群无法启动。
├── config_es.sh  该脚本，主要根据配置信息，生成ES的节点目录。其中生成的节点在安装包中的 root目录下，命名方式为 ES0 ES1 ES2 ES*
├── start.sh      该脚本，用来在启动ES节点。仅需要在每台机器上执行一次即可！机器上配置的多个节点都会被启动起来。
└── stop.sh       该脚本，用来关停ES节点。仅需要在每台机器上执行一次即可！机器上配置的多个节点都会被关停。
```

### 二、安装说明手册
    操作简单，先修改配置文件，然后运行脚本即可完成集群搭建。

    【注意】:
    1.仅支持在linux环境下执行。
    2.注意使用root权限用户执行脚本。root权限需要修改集群启动的必要参数，需要优化修改内核参数。
    3.请严格按照以下文档中的顺序操作。请严格按照文档操作。
    4.执行脚本的顺序为：
        1）#sh deploy_es.sh
        2）#sh config_es.sh
        3）#sh start.sh
    5.部署多个节点时，其他节点需要做相同配置。
        
#### 1、修改部署集群配置文件
 **修改conf目录下config.ini 配置文件，以下为必须修改参数，其他默认即可。**

    1.es_cluster_name : 集群名称
    2.es_host_list : 集群的ip列表，如果需要部署多台机器，则以空格分隔。例如 "10.99.10.10 10.99.10.11"
    3.es_node_num : 指定一台服务器上部署几个节点
    4.es_node_path_num : 节点挂的磁盘路径个数
    5.es_data_path_list : "/u01/isi/application/es_data01;/u01/isi/application/es_data02" 需要配置节点的目录路径。注意使用分号分隔，这里配置了两个，上边 es_node_num 应该是对照起来
    6.es_path_repo : 快照仓库的地址
    7.es_cluster_home : 安装包解压后的路径，这里会把安装的节点放在安装包中。
    8.es_run_user : 设置es启动用户。这里不能是root用户，预设为isi用户
    9.es_heap_size : 设置堆空间大小，单位为m。
    10.es_processors : cpu核心数，注意是每个节点能分配到的。 总逻辑核心数/节点数

#### 2、修改 ES集群配置参数
 **修改conf目录下es_config.ini 配置文件，以下为必须修改参数，其他默认即可。**

    1.cluster.initial_master_nodes : 指定集群启动的时候，master节点。（加速组件集群）

#### 3、初始化本地环境
    使用root用户，在disearch根目录/bin，运行 deploy_es.sh 脚本启动集群。

    #chmod 755 -R 安装包解压后的目录
    #cd bin
    #sh deploy_es.sh
    >   成功标识：es deploy done!

#### 4、创建ES集群
    使用root用户，在disearch根目录/bin，运行 config_es.sh 脚本。

    #cd bin
    #sh config_es.sh
    >   config has finished. 在root目录中，会生成ES节点。

#### 5、启动ES集群
    使用root用户，在disearch根目录/bin，运行 start.sh 脚本启动集群。

    #cd bin
    #sh start.sh
    >   All nodes joined

#### 6、停止ES集群
    使用root用户，在disearch根目录/bin，运行 stop.sh 脚本。

    #cd bin
    #sh stop.sh
