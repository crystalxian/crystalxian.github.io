---
layout: post
title:  "在Caffe中自定义smooth_L1_loss_layer"
date:   2016-08-16 
categories: Caffe 
tags: Caffe Layer 
---

* content
{:toc}

## Developing new loss layers

- Add a class declaration for smooth_L1_loss_layer layer to `include/caffe/layers/smooth_L1_loss_layer.hpp`. 
  * Include an inline implementation of `type` overriding the method `virtual inline const char* type() const { return "SmoothL1Loss"; }` 
  * Implement the `MinBottomBlobs()` methods to specify minimum blob number requirements; `MaxBottomBlobs()` methods to specify maximum blob number requirements.
  * Does layer-specific setup:override the method `vitual void LayerSetUp` as well as `vitual void Reshape`.
  * Implement `virtual void Forward_cpu` `virtual void Forward_gpu` `virtual void Backward_cpu` `virtual void Backward_gpu` methods to do the forward/backward process.(gpu and backward are optional for other layers)
  * Unlike most loss layers, in the SmoothL1LossLayer we can backpropagate to both inputs -- override to return true and always allow force_backward. **However, the backward computation above doesn't get correct results, because Caffe decides that the network does not need backward computation. To get correct backward results, we need to set `'force_backward: true'` in your network prototxt.**(optional for other layers)
  * Implement the constructor inheriting from LossLayer `explicit SmoothL1LossLayer(const LayerParameter& param): LossLayer<Dtype>(param), diff_() {}`
- Implement your layer in `src/caffe/layers/smooth_L1_loss_layer.cpp`.
  * `LayerSetUp` for one-time initialization: reading parameters, fixed-size allocations, etc.
  * `Reshape` for computing the sizes of top blobs, allocating buffers, and any other work that depends on the shapes of bottom blobs
  * `Forward_cpu` for the function your layer computes
  * `Backward_cpu` for its gradient (Optional -- a layer can be forward-only)
- (Optional) Implement the GPU versions `Forward_gpu` and `Backward_gpu` in `layers/your_layer.cu`.
- If needed, declare parameters in `proto/caffe.proto`, using (and then incrementing) the "next available layer-specific ID" declared in a comment above `message LayerParameter`
- Instantiate and register your layer in your cpp file with the macro provided in `layer_factory.hpp`. Assuming that you have a new layer `MyAwesomeLayer`, you can achieve it with the following command:
````
INSTANTIATE_CLASS(MyAwesomeLayer);
REGISTER_LAYER_CLASS(MyAwesome);
````
- Note that you should put the registration code in your own cpp file, so your implementation of a layer is self-contained.
- Optionally, you can also register a Creator if your layer has multiple engines. For an example on how to define a creator function and register it, see `GetConvolutionLayer` in `caffe/layer_factory.cpp`.
- Write tests in `test/test_your_layer.cpp`. Use `test/test_gradient_check_util.hpp` to check that your Forward and Backward implementations are in numerical agreement.

## Forward-Only Layers
If you want to write a layer that you will only ever include in a test net, you do not have to code the backward pass. For example, you might want a layer that measures performance metrics at test time that haven't already been implemented.
Doing this is very simple. You can write an inline implementation of `Backward_cpu` (or `Backward_gpu`) together with the definition of your layer in `include/caffe/your_layer.hpp` that looks like:
````
virtual void Backward_cpu(const vector<Blob<Dtype>*>& top, const vector<bool>& propagate_down, const vector<Blob<Dtype>*>& bottom) {
  NOT_IMPLEMENTED;
}
````
The `NOT_IMPLEMENTED` macro (defined in `common.hpp`) throws an error log saying "Not implemented yet". For examples, look at the accuracy layer (`accuracy_layer.hpp`) and threshold layer (`threshold_layer.hpp`) definitions.
