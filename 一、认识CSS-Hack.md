## 认识CSS Hack
---

### 1. 什么是CSS Hack

作为一个前端，在工作中可能时常会被要求解决浏览器兼容问题，那前端涉及到的HTML、JavaScript和CSS都会遇到浏览器兼容性问题。本文主要想聊一聊解决浏览器兼容性中CSS的部分，谈到CSS浏览器兼容性问题，就不得不说说CSS Hack。那什么是CSS Hack，我就按照自己的想法尝试总结一下。同时附上百度文库对CSS Hack的定义。

百度文库的定义：

> 简单地讲，css hack指各版本及各品牌浏览器之间对CSS解释后出现网页内容的误差(比如我们常说错位)的处理。由于各浏览器的内核不同，所以会造成一些误差就像JS一样，一个JS网页特效，在微软IE6、IE7、IE8浏览器有效果，但可能在火狐（Mozilla Firefox）谷歌浏览器无效，这样就叫做JS hack ，所以我们对于CSS来说他们来解决各浏览器对CSS解释不同所采取的区别不同浏览器制作不同的CSS样式的设置来解决这些问题就叫作CSS Hack。



我的定义：


> CSS Hack，就是在CSS样式中添加一些特殊的标识，而这些特殊的标识在不同浏览器或相同浏览器的不同版本可以被区别采用，已达到在不同浏览器之间或相同版本不同版本之间达到一致性。



### 2. 什么情况下会需要CSS Hack

在我的工作经历中，一般像偏传统企业会比较要求解决浏览器兼容性问题，尤其是需要解决IE6、IE7、IE8、IE9的兼容性，当时为了解决这个问题，也是病急乱投医，上网看看哪段代码能用，就粘过来试试，觉得可以行的通就ok了。后来闲下来的时候才看到原来还有这么个东西可以解决CSS浏览器兼容性。当然，这个也是根据项目初期设定满足各个版本的最低版本，这样也方便在开发过程中，编写满足的要求的代码。

### 3. 浏览器识别字符标准对应表
![浏览器识别字符标准对应表](./images/ie.png "浏览器识别字符标准对应表")


从上图我们就可以看出不同字符被哪些浏览器所识别，接下来我就总结我日常采用的几种区分：

1. \9：区别IE和非IE
2. _和-：仅支持IE6 
3. *：仅支持IE6和IE7
4. \0：IE8、IE9支持,opera部分支持
5. \9\0：IE8部分支持、IE9支持
6. \0\9：IE8、IE9支持

### 4. CSS Hack实际例子

#### 1. 区别IE与非IE浏览器

```

#demo {
  background: blue;/*非IE下背景为蓝色*/
  background: red\9;/*IE下背景为红色*/
}

```

#### 2. 区别IE6、IE7、IE8、FF

```

#demo {
  background: blue;/*FF下为蓝色*/
  background: red\9;/*IE8下为红色*/
  *background: black;/*IE7下为黑色*/
  _background: orange;/*IE6下为橘色*/
}

```

【说明】：因为IE系列浏览器可读「\9」，而IE6和IE7可读「*」(米字号)，另外IE6可辨识「_」(底线)，因此可以依照顺序写下来，就会让浏览器正确的读取到自己看得懂得CSS语法，所以就可以有效区分IE各版本和非IE浏览器(像是Firefox、Opera、GoogleChrome、Safari等)。

#### 3. 区别IE6、IE7、FF

```

#demo {                                                                         
  background: blue;/*FF下为蓝色*/                                               
  *background: black;/*IE7下为黑色*/                                            
  _background: orange;/*IE6下为橘色*/                                           
} 

```

【说明】：IE7和IE6可读「*」(米字号)，IE6又可以读「_」(底线)，但是IE7却无法读取「_」，至于Firefox(非IE浏览器)则完全无法辨识「*」和「_」，因此就可以透过这样的差异性来区分IE6、IE7、Firefox

#### 4. 区别IE6、IE7、FF  

```

#demo {·········································································
  background: blue;/*FF下为蓝色*/···············································
  *background: black !important;/*IE7下为黑色*/
  *background: orange;/*IE6下为橘色*/···········································
}

```

【说明】：IE7可以辨识「*」和「!important」，但是IE6只可以辨识「*」，却无法辨识「!important」，至于Firefox可以读取「!important」但不能辨识「*」因此可以透过这样的差异来有效区隔IE6、IE7、Firefox。


#### 5. 区别IE7、Firefox

```

#demo {  
  background:blue;/*FF下为蓝色*/  
  *background:green!important;/*IE7下为绿色*/  
}

```

【说明】：因为Firefox可以辨识「!important」但却无法辨识「*」，而IE7则可以同时看懂「*」、「!important」，因此可以两个辨识符号来区隔IE7和Firefox。


#### 6. 区别IE6、IE7

```
#demo {
  *background:black;/*IE7下为黑色*/  
  _background:orange;/*IE6下为橘色*/  
}
```

说明】：IE7和IE6都可以辨识「*」(米字号)，但IE6可以辨识「_」(底线)，IE7却无法辨识，透过IE7无法读取「_」的特性就能轻鬆区隔IE6和IE7之间的差异。

#### 7. 区别IE6、IE7 

```
#demo {  
  background:black!important;/*IE7下为黑色*/  
  background:orange;/*IE6下为橘色*/  
} 

```

【说明】：因为IE7可读取「!important;」但IE6却不行，而CSS的读取步骤是从上到下，因此IE6读取时因无法辨识「!important」而直接跳到下一行读取CSS，所以背景色会呈现橘色。

#### 8. 区别IE6、Firefox

```

#demo {  
  background:black;/*FF下为黑色*/  
  _background:orange;/*IE6下为橘色*/  
}

```

说明】：因为IE6可以辨识「_」(底线)，但是Firefox却不行，因此可以透过这样的差异来区隔Firefox和IE6，有效达成CSShack。
