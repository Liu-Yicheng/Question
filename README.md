一.为什么卷积神经网络中卷积核边长一般为奇数？     
　　答：1.如果卷积核边长为偶数，根据padding公式，当采用SAME-Padding时会产生不对称填充，      
　　　　　而边长为奇数时，会进行自然填充。      
　　　　2.当卷积核边长为奇数时，卷积核具有一个中心点，方便进行中心点的定位。   
    
二.特征可视化带来的启示？   
　　答：1.一组特征的输入特征（通过重构获得）将刺激卷积网产生一个固定的输出特征。这些重构     
　　　　　特征可能只包含判别纹理结构的能力因此当输入存在一定畸变时，网络的输出结果保持不变。     
　　　　2.卷积网络不同层所展示的特征是不同的。底层可能检测物体的颜色、边缘与轮廓，中层主要展     
　　　　　示相似的纹理，高层主要开始体现类与类之间的差异   
　　　　3.特征在训练过程中的演化：在训练过程中由特定输出特征反向卷积，所获得的最强重构输入特   
　　　　　征展示了变化过程。当输入图片中的最强刺激源发生变化时对应的输出特征轮廓发生跳变，经过   
　　　　　一定次数的迭代，底层特征趋于稳定，但更高层的特征则需要更多的迭代收敛，一般需要（40-50周期）    
　　　　4.特征不变性：在底层，很小的微变都会导致输出特征变化明显，越往高层走，平移和尺度变化    
　　　　　对最终的结果影响越小。总体来说，卷积网络无法对旋转操作产生不变性，除非物体具有很强的对称性。    
　　　　5.卷积核边长过大会混杂大量高频和低频信号，跨度过大会产生混乱无用的特征。  
　　　　6.卷积网络中层对部件级的相关性更为关注，高层则会关注更高层的信息。以狗为例，中层会关    
　　　　　注眼睛鼻子的相关性，而高层则更会去区分狗的种类   
　　　　7.在5卷积层+2全连接层的网络中，删除全连接层或2个卷积隐含层，错误率也只有轻微的上升。   
　　　　　当所有的卷积隐含层都被去掉了，模型能力大幅下降。这说明模型的深度与分类效果密切相关，   
　　　　　深度越大，效果越好。改变全连接层的节点个数对分类性能印象不大，扩大中间卷积层的节点个   
　　　　　数对训练效果有提高，但同时也加大了全连接层出现过拟合的可能。
     
三.Python中Iterator和Iterable的区别？     
　　答：1.这是个和多态有关的问题，Python中关于迭代有两个概念，第一个是Iterable，第二个是Iterator，    
　　　　　协议规定Iterable的__iter__方法会返回一个Iterator, Iterator的__next__方法(Python2里   
　　　　　是next）会返回下一个迭代对象，如果迭代结束则抛出StopIteration异常。同时，Iterator自己也   
　　　　　是一种Iterable，所以也需要实现Iterable的接口，也就是__iter__，这样在for当中两者都可以使用。    
　　　　2.for i in j的步骤可等价为iter（j）返回一个迭代器，然后通过next（j）不断返回下一个迭代对象。    
　　　　　对于可迭代对象来说，它本身有__iter__方法，可以被iter，之后返回迭代器后再next进行循环。   
　　　　　对于迭代器来说，要想可以用for，就必须要提供iterable的接口，就是__iter__（或__getitem__）    
　　　　　对于自己构造的可迭代对象（类），必须要含有__iter__方法，并且方法返回的是一个迭代器。   
　　　　3.为什么list、dict、str等数据类型不是Iterator?   
　　　　　这是因为Python的Iterator对象表示的是一个数据流，Iterator对象可以被next()函数调用并不断返   
　　　　　回下一个数据，直到没有数据时抛出StopIteration错误。可以把这个数据流看做是一个有序序列，但我    
　　　　　们却不能提前知道序列的长度，只能不断通过next()函数实现按需计算下一个数据，所以Iterator的计算   
　　　　　是惰性的，只有在需要返回下一个数据时它才会计算。Iterator甚至可以表示一个无限大的数据流，例如    
　　　　　全体自然数。而使用list是永远不可能存储全体自然数的。   
　　　　4.Python中许多方法直接返回iterator，比如itertools里面的izip等方法，如果Iterator自己不是  
　　　　　Iterable的话，就很不方便，需要先返回一个Iterable对象，再让Iterable返回Iterator。生成器表达   
　　　　　式也是一个iterator，显然对于生成器表达式直接使用for是非常重要的。那么为什么不只保留Iterator的   
　　　　　接口而还需要设计Iterable呢？许多对象比如list、dict，是可以重复遍历的，甚至可以同时并发地进行遍   
　　　　　历，通过__iter__每次返回一个独立的迭代器，就可以保证不同的迭代过程不会互相影响。而生成器表达式之   
　　　　　类的结果往往是一次性的，不可以重复遍历，所以直接返回一个Iterator就好。让Iterator也实现Iterable  
　　　　　的兼容就可以很灵活地选择返回哪一种。总结来说Iterator实现的__iter__是为了兼容Iterable的接口，从   
　　　　　而让Iterator成为Iterable的一种实现．    
　　　　　链接：https://www.zhihu.com/question/44015086/answer/119281039     
　　　　5.创建一个迭代器有3种方法    
　　　　　　一.为容器对象添加 __iter__() 和 __next__() 方法（Python 2.7 中是 next()）；__iter__()     
　　　　　　　　返回迭代器对象本身 self，__next__() 则返回每次调用 next() 或迭代时的元素；     
　　　　　　二.内置函数 iter() 将可迭代对象转化为迭代器    
　　　　　　三.利用生成器。生成器通过 yield 语句快速生成迭代器，省略了复杂的__iter__() & __next__()方式        

