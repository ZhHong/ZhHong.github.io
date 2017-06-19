## numpy


* [numpy doc](https://docs.scipy.org/doc/numpy-dev/user/quickstart.html)文档永远是最好的老师
* 安装[numpydoc](https://github.com/numpy/numpydoc.git)
* windows版本编译
  ```shell
  >>>python setup.py bdist_wininst  #编译windows版本的包
  >>>python setup.py --help-commands #查看其它
  ``` 
* numpy对象的基本属性
    * ndarray.ndim (int)数组的维度
    * ndarray.shape (元组)数组的形状，矩阵的形状
    * ndarray.size (int)数组的长度
    * ndarray.dtype (numpy.dtype)数据类型
    * ndarray.itemsize (int)数组元素的大小
    * ndarray.data (memoryview)数组的内存地址
    * ```python
        import numpy as np
        a = np.arange(20).reshape(4,5);
        ### arange(20) ===>return array([1,...20]) int的一维数组
        ### reshape(4,5) ===>指定形状
        
        ### a.ndim =2
        ### a.shape =(4,5)
        ### a.itemsize =4
        ### a.size =20
        ### a.dtype =dtype('int32')

        b= np.array([
            [
                [1,2,3],
                [4,5,6],
                [7,8,9]
            ],
            [
                [1,2,3],
                [4,5,6],
                [7,8,9]
            ],
            [
                [1,2,3],
                [4,5,6],
                [7,8,9]
            ],
        ])

        ### b.ndim = 3
        ### b.shape = (3,3,3)
        ### b.itemsize =4
        ### b.size = 27
        ### b.dtype =dtype('int32')

        c= np.array([
            [
                [1,2,3],
                [4,5,6],
                [7,8,9]
            ],
            [
                [1,2,3],
                [4,5,6],
                [7,8,9]
            ],
            [
                [1,2,3],
            ],
        ])

        ### c.ndim  =1 (这里会把不规则的列表当成一个元素)
        ### c.shape = (3,) (数组不规则，数组的后续长度无法确定)
        ### c.itemsize =8 (item变成的object)
        ### c.size =3 
        ### c.dtype =dtype('O') 'Object'
      ```

