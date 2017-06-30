### node c++扩展
* 安装C++扩展包
```shell
npm install node-gyp -g
```
* binding.gyp
```python
{
  "targets": [
    {
      "target_name": "addon",
      "sources": [ "hello.cc" ]
    }
  ]
}
```
* cpp
```c++
```
* build
```shell
>>> node-gyp configure build
```
