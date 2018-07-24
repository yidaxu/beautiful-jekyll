
#### Lifecycle of a Node Calue
* When you evaluate a node, TensorFLow automatically determines the set of nodes that it depends on and  it evaluates these nodes first.
* IT is important to note that it will not reuse the result of the previous evaluation of w and x. In short, the preceding code evalutates w and x twice.
* All nodes values are dropped between graph runs, except variable values, which are maintained by the session across graph runs.
* A variable starts its life when its initializer is run, and it ends when the session is clossed.
* If you want to evaluate y and z efficiently, without evaluationg w and x  twicie as in the previous code, you must ask Tensorflow to evaluate both y and z in just one graph run, as shown in the following code:
```python
with tf.Session() as sess:
  y_valu, z_valu = sess.run([y, z])
  print(y_valu)
  print(z_valu)
```
* In single-process Tensorflow, multiple sessions do not share any state, even if they reuse the same graph (each session would habe its own copy of every variable). In distributed TensorFlow, variable state is stored on the servers, not in the sessions, so multiple sessions can share the same variables.



#### Tensorflow operations(also called ops for short)
Tensorflow operations can take any number of inputs and produce any number of outputs.
* the adddition and multiplications ops each take two inputs and produce one outputs.
* constants and variables take no input (they are called source ops)
* The inputs and outputs are multidimensional arrays, called tensor.
  * Just like Numpy arrays, tensors have a type and a shape.
  
#### Attention
* operations use Numpy runs immediately
* tensorflow operations do not perform any computations immediately.
  * they create nodes in the graph that will perform them wehn the graph is run.
* The main benefit of this code versus computing the Normal Equatiion directly using NumPy is the TensorFlow will automatically run this on your GPU card if you have one.
