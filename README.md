# A Keras Implementation of Sketch-RNN
In this repo there's a Kares implementation for the Sketch-RNN algorithm, as described in the paper [A Neural Representation of Sketch Drawings](https://arxiv.org/pdf/1704.03477.pdf) by David Ha and Douglas Eck (Google AI).

The implementation is ported from the [official Tensorflow implementation](https://github.com/tensorflow/magenta/tree/master/magenta/models/sketch_rnn) that was released under project [Magenta](https://magenta.tensorflow.org/) by the authors.

## Overview
Sketch-RNN consists of a Sequence to Sequence Variational Autoencoder (Seq2SeqVAE), which is able to encode a series of pen strokes (a sketch) into a latent space, using a bidirectional LSTM as the encoder. The latent representation can then be decoded back into a series of strokes.  
The model is trained to reconstruct the original stroke sequences while maintaining a normal distribution across latent space elements. Since encoding is performed stochastically, and so does the sampling mechanism in the decoder, the reconstructed sketches are always different.  
This allows to let a trained model draw new and unique sketches that it has not seen before. Designing the model as a variational autoencoder also allows to perform latent space manipulations to get interesting interpolations between different sketches.

![](https://cdn.rawgit.com/tensorflow/magenta/master/magenta/models/sketch_rnn/assets/sketch_rnn_schematic.svg)

There's no need to elaborate on the specifics of the algorithm, since many great resources exist for this end.  
I recommend David Ha's blog post [Teaching Machines to Draw](https://ai.googleblog.com/2017/04/teaching-machines-to-draw.html).

### Implementation Details
You can find in this repo some useful solutions for common pitfalls when porting from TF to Keras (and writing Keras in general), for example:
  * Using custom generators to wrap data loader classes
  * Using an auxiliary loss term that uses intermediate layers' outputs rather than the model's predictions
  * Using a CuDNN LSTM layer, allowing inference on CPU
  * Resuming a training process from checkpoint when custom callbacks are used with dynamic internal variables


### Dependencies
Tested with the following dependencies:
  * Keras 2.2.4 (Tensorflow 1.11 backend)
  * Python 3.5
  * Windows OS

Hopefully, soon I will update with minimum requirements

## Usage

### Training
To train a model, you need a dataset in the appropriate format. You can download one of many prepared skeches datasets that were [released by Google](https://console.cloud.google.com/storage/browser/quickdraw_dataset/sketchrnn). Simply download one or more `.npz` files and save them in the same directory (recommended to use a `datasets` directory within the project's main directory).
### Using a trained model to draw
