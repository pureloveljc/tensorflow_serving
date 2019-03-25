# tensorflow_serving
1 简单介绍</br>
TensorFlow Serving 是一个用于机器学习模型 serving 的高性能开源库。它可以将训练好的机器学习模型部署到线上，使用 restful api或者grpc 作为接口接受外部调用，部署 TensorFlow Serving 后，你再也不需要为线上服务操心，只需要关心你的线下模型训练，
模型训练好打包成pd文件，传入tf-serving 中，调用即可。</br>

sudo docker pull tensorflow/serving
