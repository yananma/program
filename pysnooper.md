
准备好一张 A4 纸，记录下想了解的东西，才能用 pysnooper，否则就不要用  

正确的用法是先自己通读代码，钻进去，看看到哪里就迷惑了，有什么问题，哪里没明白，想看哪里，想查什么，记下来，然后再看 pysnooper 的结果  

想 display pikachu 一样，想知道 img 的 shape、output 是什么、output\[1] 是什么等等，这样才能看懂，才有意思   

还有就是看完结果要从头再看一遍程序，通过 pysnooper 来学编程，而不是只看结果。太过依赖反而是有害的，目标是最终脱离这个工具  

**我会因为这一个工具，而成为顶级人才，只是个时间问题**  

用 pysnooper 跑《Python 编程》后面的几章  

用 pysnooper 跑《Machine Learning in action》如果不能用 notebook 就用 pycharm debug  

用 pysnooper 跑《李航统计学习方法》  

用 pysnooper 学 LeetCode  

机器学习的一些包，可以在源码上使用 pysnooper，看内部运行过程，来理解算法，而不是只调用一个接口  

编程看不懂不是因为自己没有天分，而是因为缺少 pysnooper 这样的工具  

学算法读论文，没有源码根本不行，只有源码，不会 debug，没有 pysnooper 也是白瞎  

SSD 是如此之难，靠自己读代码怎么能学会呢？一辈子都学不会  

李沐：这也是我个人信奉的一种学习方式，先一头扎进去弄懂所有细节，然后再抬头思考它背后的思想。先程序，再理论。  

重要的是这是个可以反复使用的工具，不是一个程序，而是所有程序，一天用 20 次，一年就是 7000 次，三年就是 2 万次。以前每一个都看不懂，现在看一个会一个，效果相差百万倍。再次看到方法和工具的重要性。  

成了人生中的一件大事，改变命运。现在看程序，无比清晰，和之前糊里糊涂的状态大不相同  

原来计划前 3 年肯定是以技术为主，现在看 1 年就能达到既定目标，3 年可以超过 10 年预期目标  

今后可以少受 10 倍罪，效率提高 10 倍，学习清晰程度提高百倍，学习效果提高百倍，想想能节省多少时间，产生多大影响  

原来项目计划要两年以上才能完全掌握，现在一个月就够了；原来三天一个，讲五遍效果也不行，现在一遍过  

MXNet 本来计划三到五年写 50 遍，已经写了 7、8 遍了，这要花多大精力？效果还不行  

自己试着读过 MXNet，第三章都没有完成，知道极其辛苦。  

要反复用，一直用  

再也不会模糊不清了，对学习算法有极大帮助  
<br>
<br>
一个程序几十个功能是家常便饭，几百个也是有的，虽然每一个，每一句都很简单，但是掺杂在一块就弄不清里面的逻辑了，会非常混乱。这就显示出了pysnooper的威力了，因为重复，所以看前面两三个就明白是怎么回事了。
<br>
<br>
脑子里想远远不如眼睛实实在在看到要好 
<br>
<br>
原来看程序，5 行以上，很少有彻底弄明白的。再也不会是这种情况了

<br>

看到中间过程以后总是恍然大悟，原来是这样的，没有 pysnooper 根本不知道程序做了个啥  

<br>
**用法：**
<br>
shift + tab
<br>

正确的用法是先自己通读代码，钻进去，看看到哪里就迷惑了，有什么问题，哪里没明白，想看哪里，想查什么，记下来，然后再看 pysnooper 的结果  

想 display pikachu 一样，想知道 img 的 shape、output 是什么、output\[1] 是什么等等，这样才能看懂，才有意思   

还有就是看完结果要从头再看一遍程序，通过 pysnooper 来学编程，而不是只看结果。太过依赖反而是有害的，目标是最终脱离这个工具  

<br>
import pysnooper  
@pysnooper.snoop('.\log.py', max_variable_length=None)   
<br>
一部分：with pysnooper.snoop(): 和 for 对齐  
log、prefix、max_variable_length=None、watch=('foo.bar', 'self.x["whatever"]')  
<br>
控制 for 循环次数，两三次就行，看看是怎么回事  

对于有多个函数的，一般还是应该都用，这样才能看清楚函数之间是怎么互相调用的，用 prefix  

函数用 pysnooper，表格和图像，比照对应代码。  

因为太长看不全的，就自己复制一份，在前面插入一个 Cell，for 循环改成 1 次，print 变量，或者用 if 语句控制次数  

np.set_printoptions(linewidth=500, threshold=np.inf)  

<br>

pip show pysnooper

<br>
自己修改  
<br>

**utils.py**  
<br>
get_shortish_repr 函数中，注释掉 r = r.replace('\r', '').replace('\n', '') 可以实现换行  


**tracer.py**
<br>
在 tracer class 的 \__init__ 中修改 output='.\log.py',  max_variable_length=None  
Linux 是 './log1.py'  

把 elapsed time 改成 run time  


安装 torchsnooper， 在 \__init__ 中的 class TensorFormat 的最后 return 的地方加上 + '\n' + str(tensor) 可以显示数字  




