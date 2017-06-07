# Semi-supervised learning GAN in Tensorflow

## Descriptions
This is my [Tensorflow](https://www.tensorflow.org/) implementation of **Semi-supervised Learning Generative Adversarial Networks** proposed in the paper [Improved Techniques for Training GANs](http://arxiv.org/abs/1606.03498). The goal of this work is exploiting the samples generated by GAN generators to boost the performance of image classification tasks by improving generalization.

In sum, **the main idea** is training a network playing both the roles of a *classifier* performing image classification task as well as a *discriminator* trained to distinguish samples from the *generator* distribution from real data. To be more specific, the discriminator/classifier takes an image as input and classified it into *n+1* classes, where *n* is the number of classes of a classification task. True samples are classified into the first *n* classes and generated samples are classified into the *n+1*-th class, as shown in the figure below.

<img src="figure/ssgan.png" height="300"/>

The loss of this multi-task learning framework can be decomposed into the **supervised loss** 

<img src="figure/s_loss.png" height="25"/>, 

and the **GAN loss** of a discriminator

<img src="figure/gan_loss.png" height="25"/>, 

During the training phase, we jointly minimize the total loss obtained by simply combining the two losses together.

Note that this implementation only follows the main idea of the original paper while differing a lot in implementation details such as model architectures, hyperparameters, applied optimizer, etc. Also, some useful training tricks applied to this implementation are stated at the end of this README.

## Prerequisites

- Python 2.7 or Python 3.3+
- [Tensorflow 1.0.0](https://github.com/tensorflow/tensorflow/tree/r1.0)
- [SciPy](http://www.scipy.org/install.html)
- [NumPy](http://www.numpy.org/)

## Usage

Download datasets with:
```bash
$ python download.py --dataset MNIST SVHN CIFAR10
```
Train models with downloaded dataset:
```bash
$ python trainer.py --dataset MNIST
$ python trainer.py --dataset SVHN
$ python trainer.py --dataset CIFAR10
```
Test models with saved checkpoints:
```bash
$ python evaler.py --dataset MNIST --checkpoint ckpt_dir
$ python evaler.py --dataset SVHN --checkpoint ckpt_dir
$ python evaler.py --dataset CIFAR10 --checkpoint ckpt_dir
```
Train and test your own datasets:

* Create a directory
```bash
$ mkdir datasets/YOUR_DATASET
```

* Store your data as an h5py file datasets/YOUR_DATASET/data.hy and each data point contains
    * 'image': has shape [h, w, c], where c is the number of channels (grayscale images: 1, color images: 3)
    * 'label': represented as an one-hot vector
* Maintain a list datasets/YOUR_DATASET/id.txt listing ids of all data points
* Modify trainer.py including args, data_info, etc.
* Finally, train and test models:
```bash
$ python trainer.py --dataset YOUR_DATASET
$ python evaler.py --dataset YOUR_DATASET
```
## Results

### MNIST

* Generated samples (100th epochs)

<img src="figure/result/mnist/samples.png" height="250"/>

* First 40 epochs

<img src="figure/result/mnist/training.gif" height="250"/>

### CIFAR10

* Generated samples (1000th epochs)

<img src="figure/result/cifar10/samples.png" height="250"/>

* First 200 epochs

<img src="figure/result/cifar10/training.gif" height="250"/>

## Training details

### MNIST

* The supervised loss

<img src="figure/result/mnist/s_loss.png" height="200"/>

* The loss of Discriminator

D_loss_real

<img src="figure/result/mnist/d_loss_real.png" height="200"/>

D_loss_fake

<img src="figure/result/mnist/d_loss_fake.png" height="200"/>

D_loss (total loss)

<img src="figure/result/mnist/d_loss.png" height="200"/>

* The loss of Generator

G_loss

<img src="figure/result/mnist/g_loss.png" height="200"/>

* Classification accuracy

<img src="figure/result/mnist/accuracy.png" height="200"/>

### CIFAR10

* The supervised loss


* The loss of Discriminator

D_loss_real

<img src="figure/result/cifar10/d_loss_real.png" height="200"/>

D_loss_fake

<img src="figure/result/cifar10/d_loss_fake.png" height="200"/>

D_loss (total loss)

<img src="figure/result/cifar10/d_loss.png" height="200"/>

* The loss of Generator

G_loss

<img src="figure/result/cifar10/g_loss.png" height="200"/>

* Classification accuracy

## Training tricks

* To avoid the fast convergence of the discriminator network
    * The generator network is updated more frequently.
    * Higher learning rate is applied to the training of the generator.
* One-sided label smoothing is applied to the positive labels.
* Gradient clipping trick is applied to stablize training.
* Utilize [Adam](https://arxiv.org/abs/1412.6980) optimizer with higher momentum.
* Please refer to the codes for more details.

## Related articles
* [Unsupervised and Semi-supervised Learning with Categorical Generative Adversarial Networks](https://arxiv.org/abs/1511.06390) by Springenberg
* [Semi-Supervised Learning with Generative Adversarial Networks](https://arxiv.org/abs/1606.01583) by Odena
* [Good Semi-supervised Learning that Requires a Bad GAN](https://arxiv.org/abs/1705.09783) by Dai *et. al.*
* The architecture diagram is modified from the one drawn in [Unsupervised Representation Learning with Deep Convolutional Generative Adversarial Networks](https://arxiv.org/abs/1511.06434)

## Author

Shao-Hua Sun / [@shaohua0116](https://shaohua0116.github.io/)
