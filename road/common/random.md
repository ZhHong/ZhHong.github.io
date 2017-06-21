### 随机概率算法
    目前我能想到的2种随机概率算法，偷下懒，就用node实现，其它语言类似。
*盒子*
* 将所有随机项都加入到盒子中随机
```javascript
var rd_len = box.length;
var rd = Math.floor(Math.random()*rd_len);
var hit = box[rd];              //取得命中索引

var hit_info = config[hit];

hit_info.current_hit +=1;
if(hit_info.max_hit != 999){  //对命中次数进行处理
    ood.box.splice(rd,1);
}
```
*概率*
* 直接计算概率
```javascript
var rd = Math.random();
//先计算出每个元素命中的概率
for(var a in rate){
    if(min_dis == null){
        min_dis = Math.abs(rate[a] - rd);
        min_index = a;
    }else{
        if(min_dis > Math.abs(rate[a] -rd)){
            min_dis = Math.abs(rate[a] -rd);
            min_dis = a;
        }
    }
}
```
*总结*
* 相同点
    * 两种算法都能模糊计算出命中点。
    * 两种算法都是不精确的。
* 区别
    * 盒子方式，随着元素不断减少，变现的增加了后面的元素的概率，具体的没计算，这个想法来源于实际生活中的抓奖，可能盒子方式出现的结果并不符合正态分布，但是在处理有干扰，能控制数量得情况还是挺不错的，虽然效率不是很高，但是能满足准确性。
    * 概率方式，用一个随机值来和概率MAP的偏差来确定是否命中，但是这个是有系统生成0-1的随机数，可以说是非常不可靠，有限制和干扰的时候还需要去做特殊处理。如果能对生成随机数做一下特殊处理，把这种临近的方式换成更好的计算偏差的方式，这种方法表现比盒子方式是要好的。

*[测试代码](https://github.com/ZhHong/zh_example/tree/master/node/random)*  