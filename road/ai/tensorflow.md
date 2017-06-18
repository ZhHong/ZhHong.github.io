## Tensorflow

### 安装
* 目前我选择最简单粗暴的安装方式，虽然不太如人意，待我了解更深入了再来自己编译。
* 平台选择Windows是因为，熟悉Windows，而且现在Windows带有Ubuntu子系统，日常应用是完全足够的。其它2个系统，肯定有不是花我的地方。
*  Windows CPU-only: [Python 3.5 64-bit](https://ci.tensorflow.org/view/Nightly/job/nightly-win/DEVICE=cpu,OS=windows/lastSuccessfulBuild/artifact/cmake_build/tf_python/dist/tensorflow-1.0.1-cp35-cp35m-win_amd64.whl)，其它版本[TensorFlow](https://github.com/tensorflow/tensorflow)
### 测试
* 先来个TensorFlow的hello world
``` python
>>> import tensorflow as tf
>>> a = tf.constant(1)
>>> b = tf.constant(2)
>>> session = tf.Session()
>>> session.run(a+b)
3
```
*过程中有警告：这应该是直接用编译好的轮子的原因,一些可以加速优化的选项没有处理，以后再来处理这些警告*
```shell
The TensorFlow library wasn't compiled to use SSE instructions, but these are available on your machine and could speed up CPU computations.
The TensorFlow library wasn't compiled to use SSE2 instructions, but these are available on your machine and could speed up CPU computations.
The TensorFlow library wasn't compiled to use SSE3 instructions, but these are available on your machine and could speed up CPU computations.
The TensorFlow library wasn't compiled to use SSE4.1 instructions, but these are available on your machine and could speed up CPU computations.
The TensorFlow library wasn't compiled to use SSE4.2 instructions, but these are available on your machine and could speed up CPU computations.
The TensorFlow library wasn't compiled to use AVX instructions, but these are available on your machine and could speed up CPU computations.
The TensorFlow library wasn't compiled to use AVX2 instructions, but these are available on your machine and could speed up CPU computations.
The TensorFlow library wasn't compiled to use FMA instructions, but these are available on your machine and could speed up CPU computations.
```