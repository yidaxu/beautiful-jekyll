### Encoding and Decoding images
* Encoding images are represented by scalar string Tensors
* Decoding images by 3-D unit8 tensors of shape [height, width, channels]

```python
tf.image.decode_jpeg
```
* Read a path and return an image as string. Below line of code returns the image bytes as a string

```python
tf.gfile.FastGFile(imagePath, 'rb').read()
```
