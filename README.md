# k8s

---

k8s 容器编排文件,统一为 `yml` 格式.

###目录

* heapster: 容器监控,用于采集容器,节点的资源信息.
	* influxb: 我们采用influxdb作为存储
* skydns: k8s 的 dns
* test: 测试用的编排文件

###使用须知

这里主要收集一下我踩过的坑.

####镜像

因为一些编排文件使用的时google镜像仓库.为了解决翻墙问题,我暂时使用阿里云的国外服务器编译功能,编译到阿里云的服务器仓库作为中转.

####heapster

**heapster-controller.yaml**

`--source=kubernetes:http://masterIp:8080`,这里替换成自己的k8s-master的地址.

**docker版本问题**

现在使用的是1.9.1,但是会报告

		Failed to get pwuid struct: user: unknown userid 4294967295
		
错误.

我尝试使用1.11.2版本的`docker-engine`,可以解决这个问题.但是这个版本的`docker stats`会出现统计不到数据的bug.这里会影响 heapster 获取容器内部资源使用数据的问题.

所以我还是替换为1.9.1版本.

####skydns

**skydns-rc.yaml**

我再运行skydns发现其中报错,找不见k8s-master的地址.所以解决方案如下:

在`kube2sky`中的`args:`中添加如下参数:

		--kube_master_url=http://masterIp:8080




