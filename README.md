# Enter your network definition here.
# Use Shift+Enter to update the visualization.
name: "ResNet-18"
layer {
  name: "data"
  type: "ImageData"
  top: "data"
  top: "label"
  include {
    phase: TRAIN
  }
  transform_param {
    mirror: false
    mean_value: 109.419
    mean_value: 106.767
    mean_value: 111.707
  }
  image_data_param {
    mirror: true
    source: "train2loss.txt"  
    root_folder: "/home/../"  
    new_height: 64
    new_width: 42 
    batch_size: 256  
    shuffle: true  
    label_dim: 2
   }
}

layer {
  name: "data"
  type: "ImageData"
  top: "data"
  top: "label"

  include {
    phase: TEST
  }
  transform_param {
    mirror: false
    mean_value:109.419
        mean_value: 106.767 
        mean_value: 111.707
  }
  image_data_param {
    source:"train2loss.txt"
    batch_size: 1
    new_height: 64
    new_width: 42
  }
}

layer {
  name: "slice"
  type: "Slice"
  bottom: "label"
  top: "label_1"
  top: "label_2"
  slice_param {
    axis: 1
    slice_point:1
  }
}

layer {
  name: "conv1"
  type: "Convolution"
  bottom: "data"
  top: "conv1"
  convolution_param {
    num_output: 64
    pad: 3
    kernel_size: 7
    stride: 2
    weight_filler {
      type: "xavier"
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "bn_conv1"
  type: "BatchNorm"
  bottom: "conv1"
  top: "conv1"
  batch_norm_param { 
  }
}
layer {
  name: "scale_conv1"
  type: "Scale"
  bottom: "conv1"
  top: "conv1"
  scale_param {
    bias_term: true
  }
}
layer {
  name: "conv1_relu"
  type: "ReLU"
  bottom: "conv1"
  top: "conv1"
}
layer {
  name: "pool1"
  type: "Pooling"
  bottom: "conv1"
  top: "pool1"
  pooling_param {
    pool: MAX
    kernel_size: 3
    stride: 2
  }
}
layer {
  name: "res2a_branch1"
  type: "Convolution"
  bottom: "pool1"
  top: "res2a_branch1"
  convolution_param {
    num_output: 256
    bias_term: false
    pad: 0
    kernel_size: 1
    stride: 1
    weight_filler {
      type: "xavier"
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "bn2a_branch1"
  type: "BatchNorm"
  bottom: "res2a_branch1"
  top: "res2a_branch1"
  batch_norm_param { 
  }
}
layer {
  name: "scale2a_branch1"
  type: "Scale"
  bottom: "res2a_branch1"
  top: "res2a_branch1"
  scale_param {
    bias_term: true
  }
}
layer {
  name: "res2a_branch2a"
  type: "Convolution"
  bottom: "pool1"
  top: "res2a_branch2a"
  convolution_param {
    num_output: 64
    bias_term: false
    pad: 0
    kernel_size: 1
    stride: 1
    weight_filler {
      type: "xavier"
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "bn2a_branch2a"
  type: "BatchNorm"
  bottom: "res2a_branch2a"
  top: "res2a_branch2a"
  batch_norm_param { 
  }
}
layer {
  name: "scale2a_branch2a"
  type: "Scale"
  bottom: "res2a_branch2a"
  top: "res2a_branch2a"
  scale_param {
    bias_term: true
  }
}
layer {
  name: "res2a_branch2a_relu"
  type: "ReLU"
  bottom: "res2a_branch2a"
  top: "res2a_branch2a"
}
layer {
  name: "res2a_branch2b"
  type: "Convolution"
  bottom: "res2a_branch2a"
  top: "res2a_branch2b"
  convolution_param {
    num_output: 64
    bias_term: false
    pad: 1
    kernel_size: 3
    stride: 1
    weight_filler {
      type: "xavier"
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "bn2a_branch2b"
  type: "BatchNorm"
  bottom: "res2a_branch2b"
  top: "res2a_branch2b"
  batch_norm_param { 
  }
}
layer {
  name: "scale2a_branch2b"
  type: "Scale"
  bottom: "res2a_branch2b"
  top: "res2a_branch2b"
  scale_param {
    bias_term: true
  }
}
layer {
  name: "res2a_branch2b_relu"
  type: "ReLU"
  bottom: "res2a_branch2b"
  top: "res2a_branch2b"
}
layer {
  name: "res2a_branch2c"
  type: "Convolution"
  bottom: "res2a_branch2b"
  top: "res2a_branch2c"
  convolution_param {
    num_output: 256
    bias_term: false
    pad: 0
    kernel_size: 1
    stride: 1
    weight_filler {
      type: "xavier"
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "bn2a_branch2c"
  type: "BatchNorm"
  bottom: "res2a_branch2c"
  top: "res2a_branch2c"
  batch_norm_param { 
  }
}
layer {
  name: "scale2a_branch2c"
  type: "Scale"
  bottom: "res2a_branch2c"
  top: "res2a_branch2c"
  scale_param {
    bias_term: true
  }
}
layer {
  name: "res2a"
  type: "Eltwise"
  bottom: "res2a_branch1"
  bottom: "res2a_branch2c"
  top: "res2a"
}
layer {
  name: "res2a_relu"
  type: "ReLU"
  bottom: "res2a"
  top: "res2a"
}
layer {
  name: "res2b_branch2a"
  type: "Convolution"
  bottom: "res2a"
  top: "res2b_branch2a"
  convolution_param {
    num_output: 64
    bias_term: false
    pad: 0
    kernel_size: 1
    stride: 1
    weight_filler {
      type: "xavier"
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "bn2b_branch2a"
  type: "BatchNorm"
  bottom: "res2b_branch2a"
  top: "res2b_branch2a"
  batch_norm_param { 
  }
}
layer {
  name: "scale2b_branch2a"
  type: "Scale"
  bottom: "res2b_branch2a"
  top: "res2b_branch2a"
  scale_param {
    bias_term: true
  }
}
layer {
  name: "res2b_branch2a_relu"
  type: "ReLU"
  bottom: "res2b_branch2a"
  top: "res2b_branch2a"
}
layer {
  name: "res2b_branch2b"
  type: "Convolution"
  bottom: "res2b_branch2a"
  top: "res2b_branch2b"
  convolution_param {
    num_output: 64
    bias_term: false
    pad: 1
    kernel_size: 3
    stride: 1
    weight_filler {
      type: "xavier"
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "bn2b_branch2b"
  type: "BatchNorm"
  bottom: "res2b_branch2b"
  top: "res2b_branch2b"
  batch_norm_param { 
  }
}
layer {
  name: "scale2b_branch2b"
  type: "Scale"
  bottom: "res2b_branch2b"
  top: "res2b_branch2b"
  scale_param {
    bias_term: true
  }
}
layer {
  name: "res2b_branch2b_relu"
  type: "ReLU"
  bottom: "res2b_branch2b"
  top: "res2b_branch2b"
}
layer {
  name: "res2b_branch2c"
  type: "Convolution"
  bottom: "res2b_branch2b"
  top: "res2b_branch2c"
  convolution_param {
    num_output: 256
    bias_term: false
    pad: 0
    kernel_size: 1
    stride: 1
    weight_filler {
      type: "xavier"
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "bn2b_branch2c"
  type: "BatchNorm"
  bottom: "res2b_branch2c"
  top: "res2b_branch2c"
  batch_norm_param { 
  }
}
layer {
  name: "scale2b_branch2c"
  type: "Scale"
  bottom: "res2b_branch2c"
  top: "res2b_branch2c"
  scale_param {
    bias_term: true
  }
}
layer {
  name: "res2b"
  type: "Eltwise"
  bottom: "res2a"
  bottom: "res2b_branch2c"
  top: "res2b"
}
layer {
  name: "res2b_relu"
  type: "ReLU"
  bottom: "res2b"
  top: "res2b"
}
layer {
  name: "res2c_branch2a"
  type: "Convolution"
  bottom: "res2b"
  top: "res2c_branch2a"
  convolution_param {
    num_output: 64
    bias_term: false
    pad: 0
    kernel_size: 1
    stride: 1
    weight_filler {
      type: "xavier"
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "bn2c_branch2a"
  type: "BatchNorm"
  bottom: "res2c_branch2a"
  top: "res2c_branch2a"
  batch_norm_param { 
  }
}
layer {
  name: "scale2c_branch2a"
  type: "Scale"
  bottom: "res2c_branch2a"
  top: "res2c_branch2a"
  scale_param {
    bias_term: true
  }
}
layer {
  name: "res2c_branch2a_relu"
  type: "ReLU"
  bottom: "res2c_branch2a"
  top: "res2c_branch2a"
}
layer {
  name: "res2c_branch2b"
  type: "Convolution"
  bottom: "res2c_branch2a"
  top: "res2c_branch2b"
  convolution_param {
    num_output: 64
    bias_term: false
    pad: 1
    kernel_size: 3
    stride: 1
    weight_filler {
      type: "xavier"
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "bn2c_branch2b"
  type: "BatchNorm"
  bottom: "res2c_branch2b"
  top: "res2c_branch2b"
  batch_norm_param { 
  }
}
layer {
  name: "scale2c_branch2b"
  type: "Scale"
  bottom: "res2c_branch2b"
  top: "res2c_branch2b"
  scale_param {
    bias_term: true
  }
}
layer {
  name: "res2c_branch2b_relu"
  type: "ReLU"
  bottom: "res2c_branch2b"
  top: "res2c_branch2b"
}
layer {
  name: "res2c_branch2c"
  type: "Convolution"
  bottom: "res2c_branch2b"
  top: "res2c_branch2c"
  convolution_param {
    num_output: 256
    bias_term: false
    pad: 0
    kernel_size: 1
    stride: 1
    weight_filler {
      type: "xavier"
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "bn2c_branch2c"
  type: "BatchNorm"
  bottom: "res2c_branch2c"
  top: "res2c_branch2c"
  batch_norm_param { 
  }
}
layer {
  name: "scale2c_branch2c"
  type: "Scale"
  bottom: "res2c_branch2c"
  top: "res2c_branch2c"
  scale_param {
    bias_term: true
  }
}
layer {
  name: "res2c"
  type: "Eltwise"
  bottom: "res2b"
  bottom: "res2c_branch2c"
  top: "res2c"
}
layer {
  name: "res2c_relu"
  type: "ReLU"
  bottom: "res2c"
  top: "res2c"
}
layer {
  name: "res3a_branch1"
  type: "Convolution"
  bottom: "res2c"
  top: "res3a_branch1"
  convolution_param {
    num_output: 512
    bias_term: false
    pad: 0
    kernel_size: 1
    stride: 2
    weight_filler {
      type: "xavier"
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "bn3a_branch1"
  type: "BatchNorm"
  bottom: "res3a_branch1"
  top: "res3a_branch1"
  batch_norm_param { 
  }
}
layer {
  name: "scale3a_branch1"
  type: "Scale"
  bottom: "res3a_branch1"
  top: "res3a_branch1"
  scale_param {
    bias_term: true
  }
}
layer {
  name: "res3a_branch2a"
  type: "Convolution"
  bottom: "res2c"
  top: "res3a_branch2a"
  convolution_param {
    num_output: 128
    bias_term: false
    pad: 0
    kernel_size: 1
    stride: 2
    weight_filler {
      type: "xavier"
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "bn3a_branch2a"
  type: "BatchNorm"
  bottom: "res3a_branch2a"
  top: "res3a_branch2a"
  batch_norm_param { 
  }
}
layer {
  name: "scale3a_branch2a"
  type: "Scale"
  bottom: "res3a_branch2a"
  top: "res3a_branch2a"
  scale_param {
    bias_term: true
  }
}
layer {
  name: "res3a_branch2a_relu"
  type: "ReLU"
  bottom: "res3a_branch2a"
  top: "res3a_branch2a"
}
layer {
  name: "res3a_branch2b"
  type: "Convolution"
  bottom: "res3a_branch2a"
  top: "res3a_branch2b"
  convolution_param {
    num_output: 128
    bias_term: false
    pad: 1
    kernel_size: 3
    stride: 1
    weight_filler {
      type: "xavier"
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "bn3a_branch2b"
  type: "BatchNorm"
  bottom: "res3a_branch2b"
  top: "res3a_branch2b"
  batch_norm_param { 
  }
}
layer {
  name: "scale3a_branch2b"
  type: "Scale"
  bottom: "res3a_branch2b"
  top: "res3a_branch2b"
  scale_param {
    bias_term: true
  }
}
layer {
  name: "res3a_branch2b_relu"
  type: "ReLU"
  bottom: "res3a_branch2b"
  top: "res3a_branch2b"
}
layer {
  name: "res3a_branch2c"
  type: "Convolution"
  bottom: "res3a_branch2b"
  top: "res3a_branch2c"
  convolution_param {
    num_output: 512
    bias_term: false
    pad: 0
    kernel_size: 1
    stride: 1
    weight_filler {
      type: "xavier"
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "bn3a_branch2c"
  type: "BatchNorm"
  bottom: "res3a_branch2c"
  top: "res3a_branch2c"
  batch_norm_param { 
  }
}
layer {
  name: "scale3a_branch2c"
  type: "Scale"
  bottom: "res3a_branch2c"
  top: "res3a_branch2c"
  scale_param {
    bias_term: true
  }
}
layer {
  name: "res3a"
  type: "Eltwise"
  bottom: "res3a_branch1"
  bottom: "res3a_branch2c"
  top: "res3a"
}
layer {
  name: "res3a_relu"
  type: "ReLU"
  bottom: "res3a"
  top: "res3a"
}
layer {
  name: "res3b_branch2a"
  type: "Convolution"
  bottom: "res3a"
  top: "res3b_branch2a"
  convolution_param {
    num_output: 128
    bias_term: false
    pad: 0
    kernel_size: 1
    stride: 1
    weight_filler {
      type: "xavier"
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "bn3b_branch2a"
  type: "BatchNorm"
  bottom: "res3b_branch2a"
  top: "res3b_branch2a"
  batch_norm_param { 
  }
}
layer {
  name: "scale3b_branch2a"
  type: "Scale"
  bottom: "res3b_branch2a"
  top: "res3b_branch2a"
  scale_param {
    bias_term: true
  }
}
layer {
  name: "res3b_branch2a_relu"
  type: "ReLU"
  bottom: "res3b_branch2a"
  top: "res3b_branch2a"
}
layer {
  name: "res3b_branch2b"
  type: "Convolution"
  bottom: "res3b_branch2a"
  top: "res3b_branch2b"
  convolution_param {
    num_output: 128
    bias_term: false
    pad: 1
    kernel_size: 3
    stride: 1
    weight_filler {
      type: "xavier"
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "bn3b_branch2b"
  type: "BatchNorm"
  bottom: "res3b_branch2b"
  top: "res3b_branch2b"
  batch_norm_param { 
  }
}
layer {
  name: "scale3b_branch2b"
  type: "Scale"
  bottom: "res3b_branch2b"
  top: "res3b_branch2b"
  scale_param {
    bias_term: true
  }
}
layer {
  name: "res3b_branch2b_relu"
  type: "ReLU"
  bottom: "res3b_branch2b"
  top: "res3b_branch2b"
}
layer {
  name: "res3b_branch2c"
  type: "Convolution"
  bottom: "res3b_branch2b"
  top: "res3b_branch2c"
  convolution_param {
    num_output: 512
    bias_term: false
    pad: 0
    kernel_size: 1
    stride: 1
    weight_filler {
      type: "xavier"
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "bn3b_branch2c"
  type: "BatchNorm"
  bottom: "res3b_branch2c"
  top: "res3b_branch2c"
  batch_norm_param { 
  }
}
layer {
  name: "scale3b_branch2c"
  type: "Scale"
  bottom: "res3b_branch2c"
  top: "res3b_branch2c"
  scale_param {
    bias_term: true
  }
}
layer {
  name: "res3b"
  type: "Eltwise"
  bottom: "res3a"
  bottom: "res3b_branch2c"
  top: "res3b"
}
layer {
  name: "res3b_relu"
  type: "ReLU"
  bottom: "res3b"
  top: "res3b"
}
layer {
  name: "pool5"
  type: "Pooling"
  bottom: "res3b"
  top: "pool5"
  pooling_param {
    pool: AVE
    global_pooling: true
  }
}


layer {
  name: "conv10"
  type: "Convolution"
  bottom: "pool5"
  top: "conv10"
    param { lr_mult: 1.0  decay_mult: 1.0 }
  param { lr_mult: 2.0  decay_mult: 0 }
  convolution_param {
    num_output: 20224
    kernel_size: 1
    weight_filler {
      type: "xavier"
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}

layer {
  name: "N-loss"
  type: "SoftmaxWithLoss"
  bottom: "conv10"
  bottom: "label_1"
  top: "N-loss"
  #include {
  #  phase: TRAIN
  #}
}

layer {
  name: "conv20"
  type: "Convolution"
  bottom: "pool5"
  top: "conv20"
    param { lr_mult: 1.0  decay_mult: 1.0 }
  param { lr_mult: 2.0  decay_mult: 0 }
  convolution_param {
    num_output: 2
    kernel_size: 1
    weight_filler {
      type: "xavier"
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}

layer {
  name: "2-loss"
  type: "SoftmaxWithLoss"
  bottom: "conv20"
  bottom: "label_2"
  top: "2-loss"
  #include {
  #  phase: TRAIN
  #}
}

