Basic installation of Tensorflow Lite from https://www.tensorflow.org/lite/guide/python

```
echo "deb https://packages.cloud.google.com/apt coral-edgetpu-stable main" | sudo tee /etc/apt/sources.list.d/coral-edgetpu.list
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
sudo apt-get update
sudo apt-get install python3-tflite-runtime
```

Copy `label_image.py` from https://raw.githubusercontent.com/tensorflow/tensorflow/master/tensorflow/lite/examples/python/label_image.py

Modify a few lines.

Change import line to: `import tflite_runtime.interpreter as tflite`
Change instantiation of interpreter: `interpreter = tflite.Interpreter(model_path=args.model_file)`

`ImportError: libf77blas.so.3: cannot open shared object file: No such file or directory`
