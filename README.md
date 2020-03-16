# pruning
Weight pruning aims to reduce the number of parameters and operations involved in the computationby removing connections, and thus parameters, in between neural network layers.  Practically setting the neural network parameters’ values to zero to remove what we estimate are unnecessary connectionsbetween  the  layers  of  a  neural  network.   We  can  reduce  the  size  of  the  model  for  its  storage  and/ortransmission similar. We can go a step futher and apply quantization to get even smaller model sizes.

<center>

![image](https://user-images.githubusercontent.com/3256544/76695867-834d2580-6641-11ea-9e41-03e94363c492.png)

</center>

In this example we use CIFAR10 dataset to train a MobileNetV2 model and use magnitutde-based weight pruning method to prune the whole model. We use two approaches: 

1) We use Keras optimazation toolkit which iteratively remove connections based on their magnitude, during training. Then, we run the final model through quantization.
2) We designed our own unrecoverable magnitutde-based weight pruning method which is illustrated below.

<center>

![image](https://user-images.githubusercontent.com/3256544/76794263-d9c27d00-6783-11ea-907c-b215780b5708.png)

</center>

After training the model using the first approach, we use Keras API to define the pruning parameters, we start at thesparsity level 50% and gradually train the model to reach 90% sparsity.  As a result,  as you can see below 90% of convolution layers’ weights are pruned. We noticed 5% drop of accuracy on the pruned model.

![fig42](https://user-images.githubusercontent.com/3256544/76695825-e38f9780-6640-11ea-8ffa-7dfe19b2b52e.png)

Using a generic file compression algorithm (zip), the Keras model will be reduced by approximately4x times.  The original size of the pruned model before compression was 9.15 Mb then after compression the size of the pruned model was 2.16 Mb.  With that, we then enabled post–training quantization it is  integrated  into  the  TensorFlow  Lite  conversion  tool.   We  expect  to  get  at  least  4x  reduction  in model size which matches our results of 0.56 Mb.  In  addition,  we  expect  that  most  models  will  also  have  lower  power  consumption.   See graphs  below for model size reduction for the above model.

![fig43](https://user-images.githubusercontent.com/3256544/76695827-e5595b00-6640-11ea-9d48-f141b3aef5a3.png)
