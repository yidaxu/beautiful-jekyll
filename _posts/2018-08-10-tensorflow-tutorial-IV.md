### Name Scope

复杂网络的图可能会有成千上万个节点， 我们可以利用 “name scope” 来组合有关系的节点。

```python
with tf.name_scope("loss") as scope:
  error = y_pred -y
  mse = tf.reduce_mean(tf.square(error), name = "mse")
```
* The name of each op defined within the scope is now prefixed with "loss/"

```python
print(error.op.name)
loss/sub
print(mse.op.name)
loss/mse
```
* In Tensorboard, the mse and error nodes now appear inside the loss namespace, which appears collapsed by default.


### Name Mechanisum (Modularity)
* repetitive code is hard to maintain and error-prone.
* Stay DRY (Don't Repeat Yourself):
    * create a function to build a RELU
    
* Note that when you create a node, TensorFlow checks whether its name already exists,
    * if it does it appends an underscore followed by an index to make the name unique.
    * for example
        * the first ReLU contains nodes named "weights", "bias", "z" and "relu"
        * the second ReLU contains nodes named "weights_1", "bias_1"
        
* Using name scopes, you can make the graph much clearer.
  * Simply move all the content of the relu() function inside a name scope
  
  ```python
  def relu(X):
    with tf.name_scope("relu"):
      [.....]
  ```
  
