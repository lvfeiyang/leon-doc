## 常用命令
清除无用代码:
staticcheck -checks U1000 .\service .\service\httphandler .\recommender .\recommender\... .\appconfig .\pbdata\...
staticcheck -checks U1000 .\... > unused.txt
codecoroner -ignore vendor,pbdata funcs ./... > unused.txt

抓包压测:
tcpdump -c 6000 -w test.pcap dst port 2051
tcpdump -c 6000 -w test.pcap host 172.16.168.22

./sendPcap -ip="172.16.133.2" -host="test-ftrl-predict.mtrec.com" -url="/horizon-ctr-ftrl/predict"

ab -n 1000 -c 8 -p bestTest.json -T 'application/json' http://inner-pre-doc-recall.qutoutiao.net/
ab -n 1000 -c 8 -p recReq.json -T 'application/json' http://127.0.0.1:2051/recommend

go test -bench=BenchmarkFeaProcTraversal -run=none -benchmem // -benchtime="3s" -timeout="5s" -cpu=8 -cpuprofile=cpu.prof -memprofile
go tool pprof -http=:7301 Downloads\cpu.prof

ssh:
sftp -i C:\Users\leon\.ssh\qtt_rsa -P 2222 lvxiaojun@j7.qutoutiao.net
ssh -i C:\Users\leon\.ssh\qtt_rsa -p 2222 lvxiaojun@j7.qutoutiao.net
ssh -i C:\Users\leon\.ssh\innotech -p 58422 lvxiaojun@jps.innotechx.com
scp -P 58422 sendPcap root@192.168.11.85:/data/lxj/

go编译:
GOPROXY=https://goproxy.io
go build -mod=vendor -o C:\Users\leon\go\bin\
go build -mod=readonly -o C:\Users\leon\go\bin\engine.exe .\main\main.go
set http_proxy=http://127.0.0.1:1080
set https_proxy=http://127.0.0.1:1080
set no_proxy=git.qutoutiao.net,git.innotechx.net
go test -v -timeout 30s git.qutoutiao.net\gopher\librec\client -run ^TestHttpPostWithHeader$
GODEBUG=gctrace=1 //看gc过程
go build -gcflags '-m -m -l' 3.go // 逃逸分析 -l disable inlining go tool compile --help
export GONOPROXY=gitlab.innotechx.com
export GOPRIVATE=gitlab.innotechx.com
GOMAXPROCS=1 GODEBUG=schedtrace=1000 ./example
cgo编译：
gcc -c add.c
ar cvr libadd.a add.o
go build -buildmode=c-archive -o go-export.a go-export.go
gcc -o test c-test-hash.c -L./golib -lgo

protobuf相关：
protoc --proto_path=./ --proto_path=C:\Users\leon\go\src\gitlab.innotechx.com\gopher\librec\pbdata\modelfeature --go_out=./ dumpdata.proto
protoc --go_out=C:\Users\leon\go\src defea.proto  带go_package

prometheus max step max_over_time $__interval

centos默认程序:
systemctl enable supervisord
systemctl start supervisord
item_sync.ini:
[program:item_sync]
directory=/data/lxj
command=/data/lxj/item-sync
redirect_stderr=true
user=root
stdout_logfile=/data/lxj/console.out

萌推的测试curl:
curl -H "X-Qtt-Hitexpids: 23" -H "Content-Type: application/json" -X POST -d '{"uid":"4834294", "count":10, "client_version":34013184}' http://127.0.0.1:7050/recommend
curl -H "Content-Type: application/json" -d '{"mlname":"mtctr","mlver":"v1a"}' http://corp-di-offline-train-dataai.h.innotechx.com:32222/dlapiserver/v1/model/current

etcd
export ETCDCTL_API=3 && ./etcdctl --endpoints=http://192.168.11.50:2379 get /innotech/recommender/rm --prefix

export ETCDCTL_API=3 && ./etcdctl --endpoints=http://172.16.91.59:2379 get /innotech/recommender/psonline/rec --prefix

hadoop
hadoop fs -ls hdfs://inner-di-hdfs.1sapp.com:8020/pub1/innodata/user/rec_mengtui/dnn_model/check_files


常用网站
golang的GPM说明、垃圾回收
https://juejin.im/post/5b7678f451882533110e8948
https://zhuanlan.zhihu.com/p/77943973

mysql等数据处理相关:
explain select k.id, k.exposure_day, k.exposure_content, ifnull(sum(ifnull(pv.rec_show_pv, 0)+ifnull(pv.similar_show_pv,0)+ifnull(pv.other_show_pv,0)),0) as show_pv from keep_num_info as k join recommender_content as c on k.id = c.author_id left join ( select id, rec_show_pv, similar_show_pv, other_show_pv from t_doc_history_pv where date = "2018-09-04") as pv on pv.id = c.id where c.publish_time > 1534694400000 and c.status = 2 and c.source != "趣投稿" and k.status = 1 and k.ce_date > 1535990400 group by k.id;
hive设置队列 set mapreduce.job.queuename = root.develop.adhoc.lechuan;
awk '{sum[$3] += 1} END {for (k in sum){print k, ":", sum[k]}}' history.data
sort -k 2 -n -r docreadtime.data| head -n 20| awk '{print $1}'| xargs -i awk '$1 == {}' docpvcount.data
tail -1000 content.data |awk -F '[\t]' '{if ($15>60&&$8==0) print $0}'

docker 服务
docker run -ti --rm -m 30720M --cpus=15 --network host -v /data/go:/go \
--mount type=bind,source=/data1/lxj/src/git.qutoutiao.net/ranker/predict,target=/var/lxj/predict \
registry.qtt6.cn/qtt-di-public/dev-env-go-innotech-mengtui-predict_dnn:v1 bash

docker run -ti --rm --name leon_dev --network host --mount type=bind,source=/data1/lxj/wp,target=/var/lxj/wp ubuntu:18.04 bash

docker run -it --rm -v /data1/lxj/wp/cpp/leon-c/test-debug:/home/lxj/debug proxygen:v2

## 知识总结
golang的调度机制
https://www.ardanlabs.com/blog/2015/02/scheduler-tracing-in-go.html
https://colobu.com/2019/04/28/gopher-2019-concurrent-in-action

这个特性应该与函数参数的传值特性进行区分：① Golang 中函数的参数以及返回都是数值的传递，而非引用的传递；也就是说，即使入参是一个指针，在函数运行的时候起作用的也是一个被拷贝出来的指针。② 闭包内的外部变量会跟随外部变量的变化，就好像在闭包内引用的永远是变量的指针（哪怕变量是一个普普通通的数值）；比如上面代码中 a1 和 a2 均是 int 类型的值，但在闭包内的使用就好像是指针。
解决方法
1. 声明新变量：
声明新变量：j := i，且把之后对 i 的操作改为对 j 操作。

https://www.cnblogs.com/tenyearboyue/p/12028913.html parquet的rl与dl


大heap的处理：https://blog.gopheracademy.com/advent-2018/avoid-gc-overhead-large-heaps/


hive复杂结构
https://blog.csdn.net/u010670689/article/details/72885944


## 部分资料
zeromq 协议
服务网格（Service Mesh）
www.jsss.fun
jsss.ws jsss.top

beyond compare 密钥：
w4G-in5u3SH75RoB3VZIX8htiZgw4ELilwvPcHAIQWfwfXv5n0IHDp5hv
1BM3+H1XygMtiE0-JBgacjE9tz33sIh542EmsGs1yg638UxVfmWqNLqu-
Zw91XxNEiZF7DC7-iV1XbSfsgxI8Tvqr-ZMTxlGCJU+2YLveAc-YXs8ci
RTtssts7leEbJ979H5v+G0sw-FwP9bjvE4GCJ8oj+jtlp7wFmpVdzovEh
v5Vg3dMqhqTiQHKfmHjYbb0o5OUxq0jOWxg5NKim9dhCVF+avO6mDeRNc
OYpl7BatIcd6tsiwdhHKRnyGshyVEjSgRCRY11IgyvdRPnbW8UOVULuTE

--- BEGIN LICENSE KEY ---
H1bJTd2SauPv5Garuaq0Ig43uqq5NJOEw94wxdZTpU-pFB9GmyPk677gJ
vC1Ro6sbAvKR4pVwtxdCfuoZDb6hJ5bVQKqlfihJfSYZt-xVrVU27+0Ja
hFbqTmYskatMTgPyjvv99CF2Te8ec+Ys2SPxyZAF0YwOCNOWmsyqN5y9t
q2Kw2pjoiDs5gIH-uw5U49JzOB6otS7kThBJE-H9A76u4uUvR8DKb+VcB
rWu5qSJGEnbsXNfJdq5L2D8QgRdV-sXHp2A-7j1X2n4WIISvU1V9koIyS
NisHFBTcWJS0sC5BTFwrtfLEE9lEwz2bxHQpWJiu12ZeKpi+7oUSqebX+
--- END LICENSE KEY -----

mkl 序列码
VJDC-NZ3NXFT4
33RM-RDRJWB75
N2R2-D47T6B58


死锁例子：如果线程A锁住了记录1并等待记录2，而线程B锁住了记录2并等待记录1，这样两个线程就发生了死锁现象

docker run -it --runtime=nvidia --network host -v /data6/yudaoming:/var/ydm-tf nvidia/cuda:10.0-devel-ubuntu18.04 bash

安装hdfs等apache arrow资源 https://arrow.apache.org/install/

代理设置
docker run -it --rm --network host -v /data1/lxj/v2ray:/mnt/v2ray v2fly/v2fly-core /usr/bin/v2ray -config /mnt/v2ray/cli.json

bazel build -c opt --copt=-mavx --copt=-mavx2 --copt=-mfma --copt=-mfpmath=both --copt=-msse4.1 --copt=-msse4.2 --config=mkl --config=cuda -k //tensorflow/tools/pip_package:build_pip_package


tf-gpu环境安装：
https://www.tensorflow.org/install/source

1、docker 参考https://docs.docker.com/engine/install/ubuntu/
镜像代理 https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors

2、nvidia gpu driver 参考https://docs.nvidia.com/datacenter/tesla/tesla-installation-notes/index.html#ubuntu-lts

3、nvidia-docker2 https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker

4、从源码构建 docker pull tensorflow/tensorflow:devel-gpu-py3
docker run --runtime=nvidia -it -w /tensorflow -v $PWD:/mnt tensorflow/tensorflow:devel-gpu-py3 bash
bazel build --config=opt --config=cuda --config=noaws --config=mkl //tensorflow/tools/pip_package:build_pip_package

5、assemble的dockers环境启动
docker run -e NVIDIA_VISIBLE_DEVICES=all -e NVIDIA_DRIVER_CAPABILITIES=compute,utility --runtime=nvidia \
-v /home/lxj/tmp:/mnt -v /data1:/data -it --rm --network host tf-gpu-dev:v4 bash

docker run -it --name leon_midu_dev -e NVIDIA_VISIBLE_DEVICES=2,3 -e NVIDIA_DRIVER_CAPABILITIES=compute,utility --runtime=nvidia -v /data1:/data -v /home/lxj/tmp:/mnt registry.qtt6.cn/rec/assemble-build:assemble_tfv2_2.3 bash



export http_proxy=socks5://127.0.0.1:10808
export https_proxy=socks5://127.0.0.1:10808
unset http_proxy
git config --global http.proxy 'socks5://127.0.0.1:10808'
git config --global https.proxy 'socks5://127.0.0.1:10808'
git config --global --unset http.proxy
alias wget='tsocks wget'
unalias


gpups解决hash表链的原子操作问题
show click 少， 只用1维embed

--no-upgrade
