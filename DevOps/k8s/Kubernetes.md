###  第一部分：Kubernetes概念和架构

#### k8s概述

* 谷歌开发的，内部用了15年。于2014年开源的内容编排引擎。
* 使用k8s进行容器化应用部署
* 使用k8s利用应用扩展
* k8s目标实施让部署容器化应用更高效，简洁

#### k8s特性

1. 自动装箱：基于容器对应用运行环境的资源配置要求组懂部署应用容器
2. 自我修复：容器失败时，对容器进行重启，容器没有通过监控检查时，会关闭此容器知道容器正常运行时，才对外提供服务
3. 水平扩展：通过简单的恶指令，用户界面或基于CPU等资源使用情况，对应用容器进行规模扩大或规模剪裁
4. 服务发现：负载均衡
5. 滚动更新：加了三个应用，每个都检测没有问题才对外更新
6. 版本回滚：进行历史版本即时回退
7. 密钥和配置管理：在不需要重新构建镜像的情况下，可以部署和更新密钥和应用配置，类似于热部署
8. 存储编排：自动实现存储系统挂载机应用，特别对有状态应用实现数据持久化非常重要。
9. 批处理：提供一次性服务，定时任务；满足于批量数据处理和分析的场景 

#### k8s架构组件

* k8s:
  * Master (node) 主控节点：
    * `apiserver`：集群对外的统一入口，以restful方式，交给etcd存储
    * `scheduler`：节点调度，选择node做应用部署
    * `controller-manager`： 处理集群中常规后台任务，一个资源对应一个控制器
    * `etcd`：存储系统，用于保存集群相关的数据
  * (Worker) node 工作节点：
    * `kubelet`：Master在worker node里建的一个agent，管理本机容器的各种操作
    * `kube-proxy`：提供网络代理，负载均衡等操作

#### k8s核心概念

* `Pod`：
  * k8s中最小的部署单元
  * 一组容器的集合
  * 一个Pod中的容器是共享网络的
  * 生命周期是短暂的
* `Controller`：
  * 确保预期的`Pod`的副本数量
  * 无状态应用部署（随时可以用）/有状态应用部署（需要一定条件，如ip唯一）
  * 确保所有的node运行同一个pod
  * 一次性任务和定时任务
* `Service`：
  * 定义一组`Pod`的访问规则

### 第二部分：从零搭建k8s集群

1. 搭建k8s环境平台规划
   1.1 单master集群 

   ![单master集群 ](/Users/chris/Coding Bootcamp/Notes/atguigu-notes/DevOps/k8s/images/single_master_cluster.png)
   1.2 多master集群
   ![多master集群](/Users/chris/Coding Bootcamp/Notes/atguigu-notes/DevOps/k8s/images/multimaster_cluster.png)

2. 服务器硬件配置要求
   2.1 测试环境：
   `master` ： 2core CPU; 2G RAM; 20G Storage
   `node`： 4 core CPU; 4G RAM; 40G Storage
   2.2 生产环境：
   更高

3. 搭建k8s集群部署方式
   3.1 `kubeadm`：官方社区发布的k8s的部署工具，用于快速部署
   3.2 二进制包：手动部署，能够清晰看到每一步是怎么做到的

#### 基于客户端工具kubeadm

两步走：

第一步、创建一个Master节点，`kubeadm init`
第二步、将`Node`节点加入到当前集群中 `$ kubeadm join <Master 节点的IP和端口>`

 ```shell
 # 关闭防火墙
 systemctl stop firewalld # 临时关闭
 systemctl disable firewalld # 永久关闭
 
 # 关闭selinux
 sed -i 's/enforcing/disabled/' /etc/selinux/config  # 永久
 setenforce 0  # 临时
 
 # 关闭swap
 swapoff -a  # 临时
 sed -ri 's/.*swap.*/#&/' /etc/fstab    # 永久
 
 # 根据规划设置主机名
 hostnamectl set-hostname <hostname>
 
 # 在master添加hosts
 cat >> /etc/hosts << EOF
 192.168.44.146 k8smaster
 192.168.44.145 k8snode1
 192.168.44.144 k8snode2
 EOF
 
 # 将桥接的IPv4流量传递到iptables的链
 cat > /etc/sysctl.d/k8s.conf << EOF
 net.bridge.bridge-nf-call-ip6tables = 1
 net.bridge.bridge-nf-call-iptables = 1
 EOF
 sysctl --system  # 生效
 
 # 时间同步
 yum install ntpdate -y
 ntpdate time.windows.com
 ```



#### 基于二进制包方式

### 第三部分：k8s核心概念

* Pod
* Controller
* Service Ingress

### 第四部分：搭建集群监控平台系统

### 第五部分：从零搭建高可用k8s集群

### 第六部分：在集群环境部署项目

