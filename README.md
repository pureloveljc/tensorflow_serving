# tensorflow_serving
1 简单介绍</br>
TensorFlow Serving 是一个用于机器学习模型 serving 的高性能开源库。它可以将训练好的机器学习模型部署到线上，使用 restful api或者grpc 作为接口接受外部调用，部署 TensorFlow Serving 后，你再也不需要为线上服务操心，只需要关心你的线下模型训练，
模型训练好打包成pd文件，传入tf-serving 中，调用即可。</br>
2 安装tf-serving</br>
docker安装
sudo docker pull tensorflow/serving</br>
git clone https://github.com/tensorflow/serving</br>
TESTDATA="$(pwd)/serving/tensorflow_serving/servables/tensorflow/testdata"</br>

2 docker 安装 tensorflow-serving ,using your gpu
TensorFlow Serving最简单的安装方法之一就是使用Docker，其他安装方式不在介绍。

2.1 step 1 安装 install nvida-docker
2.2 step 2 pull一个tfserver镜像
docker pull tensorflow/serving:latest-gpu

3 TensorFlow Serving gpu的使用
docker run --runtime=nvidia -p 8501:8501 \<br>

--mount type=bind,\<br>

source=/tmp/tfserving/serving/tensorflow_serving/servables/tensorflow/my_model,\<br>

target=/models/my_model \<br>

-e MODEL_NAME=half_plus_two -t tensorflow/serving:latest-gpu \<br>

--model_config_file =/models/models.config<br>

解释 ：<br>

source 模型源地址<br>

Target docker 映射地址<br>

-e MODEL_NAME 模型名字<br>

-t 用到的镜像<br>

--model_config_file 配置文件（单模型可以不加这个参数）<br>

多模型下有一点务必要注意的是，models.conf文件中model的base_path应该是与target的路径保持一致，否则会报错找不到模型。<br>

tf-serving 正在加载模型，几秒后看到暴露8501端口，<br>

则部署成功。<br>


4 TensorFlow Serving 客户端如何调用<br>
模型是 predict Api, 其他的模型请求方式请看官方文档<br>

链接：https://www.tensorflow.org/tfx/guide/serving<br>

curl -d '{"instances": [1.0, 2.0, 5.0]}' -X POST<br>

http://localhost:8501/v1/models/yolo_digit/versions/版本号:predict<br>

当然你可以编写你的脚本文件<br>

返回的预测结果以json格式：<br>

{

"predictions": [

{

"detection_classes": [2, 9, 10],

"detection_boxes": [[0.0138582, 0.791607, 0.847469, 0.989209], [0.0783433, 0.0179386, 0.96922, 0.329528], [0.0382449, 0.376244, 0.919879, 0.685031]],

"detection_scores": [0.999997, 0.999993, 0.999994]

}

]

}

5 TensorFlow Serving 总结<br>
TensorFlow Serving是GOOGLE开源的一个服务系统，适用于部署机器学习模型，灵活、性能高、可用于生产环境。 TensorFlow Serving可以轻松部署新算法和实验，同时保持相同的服务器架构和API，它具有以下特性：<br>

1支持模型版本控制和回滚<br>

2支持并发，实现高吞吐量<br>

3开箱即用，并且可定制化<br>

4支持多模型服务<br>

5支持批处理<br>

6支持热更新<br>

7支持分布式模型<br>

8易于使用的inference api<br>

9为gRPC expose port 8500，为REST API expose port 8501<br>

注意 : tensorflow serving 服务不会占用一整块gpu，当你去post数据的时候才会调用服务器的gpu运算，本此测试用本14000张循环请求，服务器GPU占用率在5%左右，性能非常好。<br>

有问题 微信 ljcyygyepq1<br>

文章链接 https://zhuanlan.zhihu.com/p/60473780<br>