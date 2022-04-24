# Reservoir Sampling

### 基本思想

* 这个算法主要用来解决当内存无法加载全部数据时，如何从包含未知大小的数据流中随机选取k个数据，并且要保证每个数据被抽取到的概率相等
* K = 1
  * 首先考虑简单的情况，当k=1时，如何制定策略：
  * 假设数据流含有N个数，我们知道如果要保证所有的数被抽到的概率相等，那么每个数抽到的概率应该为 1 / N
    * 遇到第1个数 ![\[公式\]](https://www.zhihu.com/equation?tex=n\_1) 的时候，我们保留它， ![\[公式\]](https://www.zhihu.com/equation?tex=p%28n\_1%29%3D1)
    * 遇到第2个数 ![\[公式\]](https://www.zhihu.com/equation?tex=n\_2) 的时候，我们以 ![\[公式\]](https://www.zhihu.com/equation?tex=%5Cfrac%7B1%7D%7B2%7D) 的概率保留它，那么 ![\[公式\]](https://www.zhihu.com/equation?tex=p%28n\_1%29%3D1%5Ctimes+%5Cfrac%7B1%7D%7B2%7D%3D%5Cfrac%7B1%7D%7B2%7D) ，![\[公式\]](https://www.zhihu.com/equation?tex=p%28n\_2%29%3D%5Cfrac%7B1%7D%7B2%7D)
    * 遇到第3个数 ![\[公式\]](https://www.zhihu.com/equation?tex=n\_3) 的时候，我们以 ![\[公式\]](https://www.zhihu.com/equation?tex=%5Cfrac%7B1%7D%7B3%7D) 的概率保留它，那么 ![\[公式\]](https://www.zhihu.com/equation?tex=p%28n\_1%29%3Dp%28n\_2%29%3D%5Cfrac%7B1%7D%7B2%7D%5Ctimes%281-%5Cfrac%7B1%7D%7B3%7D%29%3D%5Cfrac%7B1%7D%7B3%7D) ， ![\[公式\]](https://www.zhihu.com/equation?tex=p%28n\_3%29%3D%5Cfrac%7B1%7D%7B3%7D)
    * 遇到第i个数 ![\[公式\]](https://www.zhihu.com/equation?tex=n\_i) 的时候，我们以 ![\[公式\]](https://www.zhihu.com/equation?tex=%5Cfrac%7B1%7D%7Bi%7D) 的概率保留它，那么 ![\[公式\]](https://www.zhihu.com/equation?tex=p%28n\_1%29%3Dp%28n\_2%29%3Dp%28n\_3%29%3D%5Cdots%3Dp%28n\_%7Bi-1%7D%29%3D%5Cfrac%7B1%7D%7Bi-1%7D%5Ctimes%281-%5Cfrac%7B1%7D%7Bi%7D%29%3D%5Cfrac%7B1%7D%7Bi%7D) ， ![\[公式\]](https://www.zhihu.com/equation?tex=p%28n\_i%29%3D%5Cfrac%7B1%7D%7Bi%7D)
    * 这里的概率代表的是每个数被pick的概率
    * 所以当有n个数时，第一个数在第nth被pick的概率，就是 1 \* 1/2 \* 2/3 \* ... n - 1 / n = 1/n
    * 第二个数在nth被pick的概率就是1/2 \* 2/3 \* ... n - 1/n = 1/n
* K > 1
  *
