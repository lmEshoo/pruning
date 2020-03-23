# pruning
Weight pruning aims to reduce the number of parameters and operations involved in the computation by removing connections, and thus parameters, in between neural network layers.  Practically setting the neural network parameters’ values to zero to remove what we estimate are unnecessary connections between  the  layers  of  a  neural  network.   We  can  reduce  the  size  of  the  model  for  its  storage  and/or transmission similar. We can go a step futher and apply quantization to get even smaller model sizes.

<p align="center">
  <img src="https://user-images.githubusercontent.com/3256544/76695867-834d2580-6641-11ea-9e41-03e94363c492.png">
</p>

In this example we use CIFAR10 dataset to train a MobileNetV2 model and use magnitutde-based weight pruning method to prune the whole model. We use two approaches: 

1) We designed our own unrecoverable flat base and layer base weight pruning methods which is illustrated below. 

<p align="center">
  <img src="https://user-images.githubusercontent.com/3256544/76794263-d9c27d00-6783-11ea-907c-b215780b5708.png" width="250" height="350">
</p>

2) We use Keras optimazation toolkit which iteratively remove connections based on their magnitude, during training. Then, we run the final model through quantization.



## First Approach

We implemented Flat pruning and layer based pruning. We compared the accuracy results of testing dataset for both of these approaches. As shown in the figure below, initial accuracy numbers are almost same for flat pruning and Layer based pruning. However for high pruning rates, Flat pruning performed much better than layer based pruning.

<p align="center">
  <img src="https://user-images.githubusercontent.com/3256544/77283675-2e855c80-6c8a-11ea-9aac-2f787d8f132a.png">
</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/3256544/77283679-304f2000-6c8a-11ea-87de-fc7162dd4db8.png">
</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/3256544/77283681-30e7b680-6c8a-11ea-93af-da2941d51b8f.png">
</p>


## Second Approach

After training the model using the first approach, we use Keras API to define the pruning parameters, we start at thesparsity level 50% and gradually train the model to reach 90% sparsity.  As a result,  as you can see below 90% of convolution layers’ weights are pruned. As a result, We were able to achive a 93% training accuracy, 67% testing accuracy using SGD as our optimizer and ‘sparsecategoricalcrossentropy‘ as our loss function. As a result, we recorded 65% accuracy on the pruned model.

![fig42](https://user-images.githubusercontent.com/3256544/76695825-e38f9780-6640-11ea-8ffa-7dfe19b2b52e.png)

Using a generic file compression algorithm (zip), the Keras model was reduced by approximately 4x times.  The original size of the pruned model before compression was 9.15 Mb then after compression the size of the pruned model was 2.16 Mb.  With that, we then enabled post–training quantization it is  integrated  into  the  TensorFlow  Lite  conversion  tool.   We  expected  to  get  at  least  4x  reduction  in model size which matches our results of 0.56 Mb. We ran evaluation on the quantized model, and we were able to obtain approximately 63% testing accuracy which is only 5% away from the testing accuracy we were getting on the initial model. In  addition,  we  expect  that  most  models  will  also  have  lower  power  consumption.   See graphs  below for model size reduction for the above model.

![fig43](https://user-images.githubusercontent.com/3256544/76695827-e5595b00-6640-11ea-9d48-f141b3aef5a3.png)


## Contributors

- Lini Mestar
- Harpreet Wassan
