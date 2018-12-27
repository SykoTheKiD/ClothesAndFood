## Running a MobileNet on Android using Tensorflow Mobile

# Setup

Getting started with TF Lite is very easy

1. First download the model. We will use MobileNet which can be downloaded [here](https://github.com/tensorflow/models/blob/master/research/slim/nets/mobilenet_v1.md).
2. Place the model file in the assets folder, along with the associated labels for the model
```java
    private MappedByteBuffer loadModelFile(AssetManager assetManager, String modelPath) throws IOException {
        AssetFileDescriptor fileDescriptor = assetManager.openFd(modelPath);
        FileInputStream inputStream = new FileInputStream(fileDescriptor.getFileDescriptor());
        FileChannel fileChannel = inputStream.getChannel();
        long startOffset = fileDescriptor.getStartOffset();
        long declaredLength = fileDescriptor.getDeclaredLength();
        return fileChannel.map(FileChannel.MapMode.READ_ONLY, startOffset, declaredLength);
    }
```
3. Load the model as a TensorFlow Interpreter
```java
interpreter = new Interpreter(classifier.loadModelFile(assetManager, modelPath));
```

4. Use the model
```java
public List<Recognition> recognizeImage(Bitmap bitmap) {
        ByteBuffer byteBuffer = convertBitmapToByteBuffer(bitmap);
        byte[][] result = new byte[1][labelList.size()];
        interpreter.run(byteBuffer, result);
        return getSortedResult(result);
    }
```