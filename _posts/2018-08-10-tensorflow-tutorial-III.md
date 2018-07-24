Gradient Descent:
  When using Gradient Descent, remember that it is important to first normalize the input feature vectors, 
  or else training may be much slower. 


Saving and Restoring Models:

* Once you have trained your model, you should save its parameters to disk so you can come back to it whenever you want.
* Moreover, you probably want to save checkpoints at regular intervals during training so that if your 
computer crashes during training you can continue from the last checkpoint rather thant start over from scratch


##### Tensorflow makes saving and restoring a model very easy.
  * create a Saver node at the end of the construction phase
  * in execution phase, just call its save() method whenever you want to save the model, passing it the session and path of the checkpoint file.
  
  ```python
  with tf.Session() as sess:
    sess.run(init)

    for epoch in range(n_epochs):
        if epoch % 100 == 0:  # checkpoint every 100 epochs
            save_path = saver.save(sess, "/tmp/my_model.ckpt")

        sess.run(training_op)

    best_theta = theta.eval()
    save_path = saver.save(sess, "/tmp/my_model_final.ckpt")  
  ```
  
  ##### Restore a model is just as easy.
  * You can create a Saver at the end of the construction phase just like before.
  * Then at the beginning of the execution phase, instead of initializing the variables using the init node, 
    you call the restore() method of the Saver object:
    
   ```python
   
   with tf.Session() as sess:
      saver.restore(sess, "/tmp/my_model_final.ckpt")
    [...]
   
   ```
   
   * By default a Saver saves and restores all variables under their own name, but if you need more control
    * you can specify which variables to save or restore
    * and what names to use.
    * For example, the following Saver will save and restore only the theta variable under the name weights:
    
    ```python
    
    saver = tf.train.Saver({"weights": theta})
    
    ```
#### By default 
  * the save() method also saves the structure of the graph in a second file with the same name plus a .meta extension.
  * You can load this graph structure using ```tf.train.import_meta_graph()```. 
      *  This adds the graph to the default graph, 
      * and returns a Saver instance that you can then use to restore the graph's state
  
  ```python
  saver = tf.train.import_meta_graph("/tmp/my_model_final.ckpt.meta")
  with tf.Session() as sess:
    saver.restore(sess, "tmp/ my_model_final.ckpt")
  ```
  This allows you to fully restore a saved model, including both the graph structure and the variable values.
  without having to search for the code that built it.
  
  
  
  
  ### Visualizing the Graph and Training Curves Using TensorBoard.
  
  * If you feed it some training stats, tensorBoard will display nice interactive visualizations of learning curves in your web browser.
  * You can also provide it the graph's definition and it will give you a great interface to browse through it.
  
  * This is very useful to identify errors in the graph, to find bottlenecks and so on.
  
  
  
  #### The first step
  is to tweak your program a bit so it writes 
    * the graph definition
    * and some training stas, for example, the training error (MSE)
   to a log directory that TensorBoard will read from.
   
   
   * You need to use a different log directory every time you run your program 
    * else TensorBoard will merge stats from different runs, which will mess up the visualizations.
    * The simplest solution for this is to include a timestamp in the log dierctory name. Add the following code 
      at the beginning of the program:
 ```     
      from datetime import datetime
      now = datetime.utcnow().strftime("%Y%m%d%H%M%S")
      root_logdir = "tf_logs"
      logdir = "{}/run-{}/".format(root_logdir, now)
 ```     
 Next, add the following code at the very end of the construction phases:
 
    ```python
    mse_summary = tf.summary.scalar('MSE', mse)
    file_writer = tf.summary.FileWriter(logdir, tf.get_default_graph())
    ```
    
 * The first line creates a node in the graph that will evaluate the MSE value and write it to a TensorBoard-compatible
 binary log string called a summary. 
 * The second line creates a FileWriter that you will use to write summaries to logfiles in the log directory.
    * the first parameter indicates the path of the log directory (relative to the current directory)
    * the second (optional) parameter is the graph you want to visualize.
 * Upon creation
    * the FileWrite creates the log directory if it does not already exist( and its parent directory if neeeded)
    * writes the graph definition in a binary logfile called an events file.


#### Next you need to update the execution phase
  * to evaluate the mse_summary node regularly during training (e.g. every 10 mini-batches)
  * This will output a summary that you can then write to the events file using the file_write.
```python
[...]
    for batch_index in range(n_batches):
        X_batch, y_batch = fetch_batch(epoch, batch_index, batch_size)
        if batch_index % 10 == 0:
            summary_str = mse_summary.eval(feed_dict={X: X_batch, y: y_batch})
            step = epoch * n_batches + batch_index
            file_writer.add_summary(summary_str, step)
        sess.run(training_op, feed_dict={X: X_batch, y: y_batch})
    [...]

```
* Avoid logging training stats at every single training step, as this would significantly slow down training.
* Finally, you want to close the FileWriter at the end of the program
```python
file_writer.close()
```

#### After you run
```bash
ls -l tf_logs/run* 
```
to list the contents of the log directory.

#### It's time to fire up the TensorBoard server.

Start the server by 
* running the tensorboard command
* pointing it to the root log directory
* This starts the TensorBoard web server, listening on port 6006 (which is "goog" written upside down)

```python
$ source env/bin/activate
$ tensorboard --logdir tf_logs/
Starting TensorBoard  on port 6006
(You can navigate to http://0.0.0.0:6006)
```

#### Tensorboard
* In the Events tab you should see MSE on the right.
* Click on the Graphs tab. You should see the graph.

#### Tip

If you want to take a peak at the graph directly within Jupyter, you can use the show_graph() function available in 
the notebook for this chapter. 
