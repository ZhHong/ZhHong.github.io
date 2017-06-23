## Tensorflow

### 安装
* 目前我选择最简单粗暴的安装方式，虽然不太如人意，待我了解更深入了再来自己编译。
* 平台选择Windows是因为，熟悉Windows，而且现在Windows带有Ubuntu子系统，日常应用是完全足够的。
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
* TensorBoard

* *Pip安装的TensorFlow：*
```
>>>tensorboard --logdir = WHERE
```
* *源码方式安装的TensorFlow：*
```
>>>TODO 待定
```

#### Tensorflow 计算模型
* 计算图(Tensorboard可看)
* 获取计算图
    ```python
    >>> import tensorflow as tf
    >>> a= tf.constant(1)
    >>> a.graph                 #获取a的计算图
    <tensorflow.python.framework.ops.Graph object at 0x0000024DF78B83C8>
    >>> tf.get_default_graph()  #当前默认的计算图
    <tensorflow.python.framework.ops.Graph object at 0x0000024DF78B83C8>
    ```
    * API：tensorflow.constant(value,dtype=None,shape=None,name="Const",verify_shape = false)
        * 方法注释 
        ```python
        Creates a constant tensor.

        The resulting tensor is populated with values of type `dtype`, as
        specified by arguments `value` and (optionally) `shape` (see examples
        below).

        The argument `value` can be a constant value, or a list of values of type
        `dtype`. If `value` is a list, then the length of the list must be less
        than or equal to the number of elements implied by the `shape` argument (if
        specified). In the case where the list length is less than the number of
        elements specified by `shape`, the last element in the list will be used
        to fill the remaining entries.

        The argument `shape` is optional. If present, it specifies the dimensions of
        the resulting tensor. If not present, the shape of `value` is used.

        If the argument `dtype` is not specified, then the type is inferred from
        the type of `value`.

        For example:

        python
        # Constant 1-D Tensor populated with value list.
        tensor = tf.constant([1, 2, 3, 4, 5, 6, 7]) => [1 2 3 4 5 6 7]

        # Constant 2-D tensor populated with scalar value -1.
        tensor = tf.constant(-1.0, shape=[2, 3]) => [[-1. -1. -1.]
                                                        [-1. -1. -1.]]
        

        Args:
            value:          A constant value (or list) of output type `dtype`.

            dtype:          The type of the elements of the resulting tensor.

            shape:          Optional dimensions of resulting tensor.

            name:           Optional name for the tensor.

            verify_shape:   Boolean that enables verification of a shape of values.

        Returns:
            A Constant Tensor.
        ```
    * API: tensor.graph
        * 方法注释
        ```python
        A TensorFlow computation, represented as a dataflow graph.

        A `Graph` contains a set of
        @{tf.Operation} objects,
        which represent units of computation; and
        @{tf.Tensor} objects, which represent
        the units of data that flow between operations.

        A default `Graph` is always registered, and accessible by calling
        @{tf.get_default_graph}.
        To add an operation to the default graph, simply call one of the functions
        that defines a new `Operation`:

        python
        c = tf.constant(4.0)
        assert c.graph is tf.get_default_graph()
        

        Another typical usage involves the
        @{tf.Graph.as_default}
        context manager, which overrides the current default graph for the
        lifetime of the context:

        python
        g = tf.Graph()
        with g.as_default():
            # Define operations and tensors in `g`.
            c = tf.constant(30.0)
            assert c.graph is g
        

        Important note: This class *is not* thread-safe for graph construction. All
        operations should be created from a single thread, or external
        synchronization must be provided. Unless otherwise specified, all methods
        are not thread-safe.

        A `Graph` instance supports an arbitrary number of "collections"
        that are identified by name. For convenience when building a large
        graph, collections can store groups of related objects: for
        example, the `tf.Variable` uses a collection (named
        @{tf.GraphKeys.GLOBAL_VARIABLES}) for
        all variables that are created during the construction of a graph. The caller
        may define additional collections by specifying a new name.
        ```
    * API: tensorflow.get_default_graph()
        * 方法注释
        ```python
        Returns the default graph for the current thread.

        The returned graph will be the innermost graph on which a
        `Graph.as_default()` context has been entered, or a global default
        graph if none has been explicitly created.

        NOTE: The default graph is a property of the current thread. If you
        create a new thread, and wish to use the default graph in that
        thread, you must explicitly add a `with g.as_default():` in that
        thread's function.

        Returns:
            The default `Graph` being used in the current thread.
        ```
* 利用TensorFlow.Graph来生成新的计算图
    ```python
    import tensorflow as tf
    g1 = tf.Graph()
    with g1.as_default():
        # v = tf.get_variable("v", initializer=tf.zeros_initializer(shape=[1]))
        # error: 参数错误 
        v = tf.get_variable("v", initializer=tf.zeros(shape=[1]))
    
    g2 = tf.Graph()
    with g2.as_default():
        # v = tf.get_variable("v", initializer=tf.ones_initializer(shape=[1])) 
        # error: 参数错误 
        # v = tf.get_variable("v", initializer=tf.ones_initializer(shape=[1])) 
        v = tf.get_variable("v", initializer=tf.ones(shape=[1]))        

    print("g1 :", g1)
    print("g2 :", g2)
    with tf.Session(graph=g1) as sess:
        # tf.initialize_all_variables().run() 老版本的初始化
        tf.global_variables_initializer().run()
        with tf.variable_scope("", reuse=True):
            print("g1 :", sess.run(tf.get_variable("v")))

    with tf.Session(graph=g2) as sess:
        # tf.initialize_all_variables().run() 老版本的初始化
        tf.global_variables_initializer().run()
        with tf.variable_scope("", reuse=True):
            print("g2 :", sess.run(tf.get_variable("v")))
    ```
    ```shell
    >>>g1 : <tensorflow.python.framework.ops.Graph object at 0x000001CBF47C8C88>
    >>>g2 : <tensorflow.python.framework.ops.Graph object at 0x000001CBF98235C0>
    >>>g1 : [ 0.]
    >>>g2 : [ 1.]
    ```
* 2017/06/18
* 最近工作比较忙都是看，没时间去写，今天准备去TensorFlow游乐场玩玩。
* tensorflow 游乐场 ==>[跳板](http://playground.tensorflow.org)
* 2017/06/22
* 然后这个项目也是开源的 ==>[跳板](https://github.com/tensorflow/playground.git)
* 安装依赖，粗略的看了下代码，应该是实现了几个比较常见的深度学习算法，让人有比较直观的感觉，先看再说。
```shell
npm install  #等待安装完成 material-design-lite 比较慢....
npm run build
npm run serve
```
<!-- 设置临时的CMD代理 set http_proxy=http://127.0.0.1:PORT -->