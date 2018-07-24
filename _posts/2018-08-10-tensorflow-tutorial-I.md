
图和会话


Tensorflow两大步骤
* 定义图形（graph）
* 利用会话执行图形中的操作（session）

我们在python中第一个号一个图形，然后把图像交个tensorflow，让tensorflow去运行，感谢谷歌大大，tensorflow运行的时候调用的是优化后的C++，所以他快哇。

You first define in Python a graph of computations to perform, and then TensorFlow takes the graph and runs it efficiently using optimized C++ code.
提供了slim APIs 来简化建立， 训练 和 评价网络的过程。



* Python API offers much more flexibiility
* Highly effecient C++ implementations of many ML operations, particularly those needed to build neural networks.
* There is also a C++ API to define your own high-performance operations.
# 如何查看tensorflow的版本
```python
python3 -c 'import tensorflow; print(tensorflow.__version__)'

```
#### Tensorflow build graph
* The most important thing to understand is that this code does not actually perform any computation.
* Only creates a computation graph.
* In fact, even the variables are not initialized yet.

#### Session
* To evaluate this graph, you need to open a TensorFlow session.
* Use it to initialize the variables and evaluate f
* A Tensorflow session takes care of placing the operations onto devices such as CPUs and GPUs。 Session把操作拉到CPUs和GPUs上进行运算搞事情。
* Session holds all the variable values. Session存储所有的变量值。

#### with tf.Session() as sess
```python
with tf.Session() as sess:
  x.initializer.run()  # equals to tf.get_default_session().run(x.initializer)
  result = f.eval()  # is equivalent with tf.get_defualt_session().run(f)
```
* inside the with block, the session is set as the default session.
* The session is automatically closed at the end of the block.

#### initializer 
* Instead of manually running the initializer for every single variable, you can use the global_variable_initializer() function.
* it does not actually perform the initialization immediately, but rather creates a node in the graph that will initialize all variables when it is run.

```python
init = tf.global_variables_initializer() # prepare an init node
with tf.Session() as sess:
  sess.run(init)    # actually initialize all the variables
  result = sess.run(f)
```

#### interactive session
* Inside Jupyter or within a Python shell you may prefer to create an InteractiveSession
* The only difference from a regular Session is that when an InteractiveSession is created 
  * it automatically sets itself as the defualt session.
  * you don't need a with block (but you do need to close the session manually when you are done with it.
```python
sess = tf.InteractiveSession()
init.run()
result = f.eval()
sess.close
```

#### A TensorFlow Program
A tensorflow program is typically split into two parts:
  * The first part builds a compuation graph (the construction phase)
    * The construction phase typically builds a computation graph representing the ML model.
    * And the computations required to train it.

  * The second part runs the compuation graph （the execution phase）
    * runs a loop that evaluates a training step repeatedly.
    * gradually improving the model parameters.

#### Managing Graphs
* any node you create is automatically added to the default graph.
* In most cases this is fine, but sometimes you may want to manage multiple independent graphs.
  * You can do this by creating a new Graph
  * Temporarily making it the default inside a with block, like so.

```python
graph = tf.Graph()
with graph.as_default():
  x2 = tf.Variable(2)
  
x2.graph is graph
True
x2.graph is tf.get_default_graph()
False.
```
* In Jupyter or in a Python shell, it is common to run the same commands more than once while you are experimenting.
* As a result, you may end up with a default graph containing many duplicate nodes. One solution is to restart the Jupyter kernel (or the Python shell), but a more convenient solution is to just reset the default graph by running tf.reset_default_graph()
