control_dependencies 
* It is a mechanism to add dependencies to whatever ops you create in the with block.
* More specifically, what you specify in the argument to the control_dependencies is ensured to be evaluated before anything you define in the with block.


As of TensorFlow 1.0 (February 2017) there's also the high-level tf.layers.batch_normalization API included in TensorFlow itself.

It's super simple to use:

# Set this to True for training and False for testing
training = tf.placeholder(tf.bool)

x = tf.layers.dense(input_x, units=100)
x = tf.layers.batch_normalization(x, training=training)
x = tf.nn.relu(x)
...except that it adds extra ops to the graph (for updating its mean and variance variables) in such a way that they won't be dependencies of your training op. You can either just run the ops separately:

extra_update_ops = tf.get_collection(tf.GraphKeys.UPDATE_OPS)
sess.run([train_op, extra_update_ops], ...)
or add the update ops as dependencies of your training op manually, then just run your training op as normal:

extra_update_ops = tf.get_collection(tf.GraphKeys.UPDATE_OPS)
with tf.control_dependencies(extra_update_ops):
    train_op = optimizer.minimize(loss)
...
sess.run([train_op], ...)
