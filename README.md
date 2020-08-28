## UNet++: A Nested U-Net Architecture for Medical Image Segmentation

UNet++ is a new general purpose image segmentation architecture for more accurate image segmentation. UNet++ consists of U-Nets of 
varying depths whose decoders are densely connected at the same resolution via the redesigned skip pathways, which aim to address 
two key challenges of the U-Net: 1) unknown depth of the optimal architecture and 2) the unnecessarily restrictive design of skip connections.

Following is a visual comparision of Unet (left) and Unet++ (right).

<table border="1" width="100%">
    <tr>
        <td><img src="images/unet.png" width="350px"></td>
        <td><img src="images/unetpp.png" width="350px"></td>
    </tr>
</table>

---

### An real world application of Unet++

This model was used in my Bachelor thesis for segmenting bacteria in blood. The dataset and an example Jupyter Notebook can be found at the Kaggle [Bacteria detection with darkfield microscopy](https://www.kaggle.com/longnguyen2306/bacteria-detection-with-darkfield-microscopy).

Here are some results of the work:

---

### Usage

Clone the repository and import the model with:

```python
from model.unetpp import UNetPP

if __name__ == '__main__':
    unet = UNetPP()
    unet.compile()
    unet.summary()
```

---

### Class imbalance

This model was designed for multi-classes classification and therefore uses [softmax](https://en.wikipedia.org/wiki/Softmax_function). 
as final activation. In order to fight class imbalance, which is usually the case, the high-order function 
`model.losses.weighted_loss` can be used to decorate classical loss functions.

---

### Model summary

Kera's summary of model with 32 convolution filters at the input.

```
__________________________________________________________________________________________________
Layer (type)                    Output Shape         Param #     Connected to                     
==================================================================================================
input_1 (InputLayer)            [(None, 512, 512, 51 0                                            
__________________________________________________________________________________________________
conv2d (Conv2D)                 (None, 512, 512, 32) 147488      input_1[0][0]                    
__________________________________________________________________________________________________
batch_normalization (BatchNorma (None, 512, 512, 32) 128         conv2d[0][0]                     
__________________________________________________________________________________________________
leaky_re_lu (LeakyReLU)         (None, 512, 512, 32) 0           batch_normalization[0][0]        
__________________________________________________________________________________________________
dropout (Dropout)               (None, 512, 512, 32) 0           leaky_re_lu[0][0]                
__________________________________________________________________________________________________
conv2d_1 (Conv2D)               (None, 512, 512, 32) 9248        dropout[0][0]                    
__________________________________________________________________________________________________
batch_normalization_1 (BatchNor (None, 512, 512, 32) 128         conv2d_1[0][0]                   
__________________________________________________________________________________________________
leaky_re_lu_1 (LeakyReLU)       (None, 512, 512, 32) 0           batch_normalization_1[0][0]      
__________________________________________________________________________________________________
dropout_1 (Dropout)             (None, 512, 512, 32) 0           leaky_re_lu_1[0][0]              
__________________________________________________________________________________________________
max_pooling2d (MaxPooling2D)    (None, 256, 256, 32) 0           dropout_1[0][0]                  
__________________________________________________________________________________________________
conv2d_2 (Conv2D)               (None, 256, 256, 64) 18496       max_pooling2d[0][0]              
__________________________________________________________________________________________________
batch_normalization_2 (BatchNor (None, 256, 256, 64) 256         conv2d_2[0][0]                   
__________________________________________________________________________________________________
leaky_re_lu_2 (LeakyReLU)       (None, 256, 256, 64) 0           batch_normalization_2[0][0]      
__________________________________________________________________________________________________
dropout_2 (Dropout)             (None, 256, 256, 64) 0           leaky_re_lu_2[0][0]              
__________________________________________________________________________________________________
conv2d_3 (Conv2D)               (None, 256, 256, 64) 36928       dropout_2[0][0]                  
__________________________________________________________________________________________________
batch_normalization_3 (BatchNor (None, 256, 256, 64) 256         conv2d_3[0][0]                   
__________________________________________________________________________________________________
leaky_re_lu_3 (LeakyReLU)       (None, 256, 256, 64) 0           batch_normalization_3[0][0]      
__________________________________________________________________________________________________
dropout_3 (Dropout)             (None, 256, 256, 64) 0           leaky_re_lu_3[0][0]              
__________________________________________________________________________________________________
max_pooling2d_1 (MaxPooling2D)  (None, 128, 128, 64) 0           dropout_3[0][0]                  
__________________________________________________________________________________________________
conv2d_6 (Conv2D)               (None, 128, 128, 128 73856       max_pooling2d_1[0][0]            
__________________________________________________________________________________________________
batch_normalization_6 (BatchNor (None, 128, 128, 128 512         conv2d_6[0][0]                   
__________________________________________________________________________________________________
leaky_re_lu_6 (LeakyReLU)       (None, 128, 128, 128 0           batch_normalization_6[0][0]      
__________________________________________________________________________________________________
dropout_5 (Dropout)             (None, 128, 128, 128 0           leaky_re_lu_6[0][0]              
__________________________________________________________________________________________________
conv2d_7 (Conv2D)               (None, 128, 128, 128 147584      dropout_5[0][0]                  
__________________________________________________________________________________________________
batch_normalization_7 (BatchNor (None, 128, 128, 128 512         conv2d_7[0][0]                   
__________________________________________________________________________________________________
leaky_re_lu_7 (LeakyReLU)       (None, 128, 128, 128 0           batch_normalization_7[0][0]      
__________________________________________________________________________________________________
dropout_6 (Dropout)             (None, 128, 128, 128 0           leaky_re_lu_7[0][0]              
__________________________________________________________________________________________________
max_pooling2d_2 (MaxPooling2D)  (None, 64, 64, 128)  0           dropout_6[0][0]                  
__________________________________________________________________________________________________
conv2d_12 (Conv2D)              (None, 64, 64, 256)  295168      max_pooling2d_2[0][0]            
__________________________________________________________________________________________________
batch_normalization_12 (BatchNo (None, 64, 64, 256)  1024        conv2d_12[0][0]                  
__________________________________________________________________________________________________
leaky_re_lu_12 (LeakyReLU)      (None, 64, 64, 256)  0           batch_normalization_12[0][0]     
__________________________________________________________________________________________________
dropout_9 (Dropout)             (None, 64, 64, 256)  0           leaky_re_lu_12[0][0]             
__________________________________________________________________________________________________
conv2d_13 (Conv2D)              (None, 64, 64, 256)  590080      dropout_9[0][0]                  
__________________________________________________________________________________________________
batch_normalization_13 (BatchNo (None, 64, 64, 256)  1024        conv2d_13[0][0]                  
__________________________________________________________________________________________________
leaky_re_lu_13 (LeakyReLU)      (None, 64, 64, 256)  0           batch_normalization_13[0][0]     
__________________________________________________________________________________________________
dropout_10 (Dropout)            (None, 64, 64, 256)  0           leaky_re_lu_13[0][0]             
__________________________________________________________________________________________________
max_pooling2d_3 (MaxPooling2D)  (None, 32, 32, 256)  0           dropout_10[0][0]                 
__________________________________________________________________________________________________
conv2d_20 (Conv2D)              (None, 32, 32, 512)  1180160     max_pooling2d_3[0][0]            
__________________________________________________________________________________________________
batch_normalization_20 (BatchNo (None, 32, 32, 512)  2048        conv2d_20[0][0]                  
__________________________________________________________________________________________________
leaky_re_lu_20 (LeakyReLU)      (None, 32, 32, 512)  0           batch_normalization_20[0][0]     
__________________________________________________________________________________________________
conv2d_21 (Conv2D)              (None, 32, 32, 512)  2359808     leaky_re_lu_20[0][0]             
__________________________________________________________________________________________________
batch_normalization_21 (BatchNo (None, 32, 32, 512)  2048        conv2d_21[0][0]                  
__________________________________________________________________________________________________
leaky_re_lu_21 (LeakyReLU)      (None, 32, 32, 512)  0           batch_normalization_21[0][0]     
__________________________________________________________________________________________________
dropout_14 (Dropout)            (None, 32, 32, 512)  0           leaky_re_lu_21[0][0]             
__________________________________________________________________________________________________
conv2d_transpose_6 (Conv2DTrans (None, 64, 64, 256)  524544      dropout_14[0][0]                 
__________________________________________________________________________________________________
concatenate_6 (Concatenate)     (None, 64, 64, 512)  0           conv2d_transpose_6[0][0]         
                                                                 dropout_10[0][0]                 
__________________________________________________________________________________________________
conv2d_transpose_3 (Conv2DTrans (None, 128, 128, 32) 32800       dropout_10[0][0]                 
__________________________________________________________________________________________________
conv2d_22 (Conv2D)              (None, 64, 64, 256)  1179904     concatenate_6[0][0]              
__________________________________________________________________________________________________
concatenate_3 (Concatenate)     (None, 128, 128, 160 0           dropout_6[0][0]                  
                                                                 conv2d_transpose_3[0][0]         
__________________________________________________________________________________________________
conv2d_transpose_1 (Conv2DTrans (None, 256, 256, 32) 16416       dropout_6[0][0]                  
__________________________________________________________________________________________________
batch_normalization_22 (BatchNo (None, 64, 64, 256)  1024        conv2d_22[0][0]                  
__________________________________________________________________________________________________
conv2d_14 (Conv2D)              (None, 128, 128, 32) 46112       concatenate_3[0][0]              
__________________________________________________________________________________________________
concatenate_1 (Concatenate)     (None, 256, 256, 96) 0           dropout_3[0][0]                  
                                                                 conv2d_transpose_1[0][0]         
__________________________________________________________________________________________________
conv2d_transpose (Conv2DTranspo (None, 512, 512, 32) 8224        dropout_3[0][0]                  
__________________________________________________________________________________________________
leaky_re_lu_22 (LeakyReLU)      (None, 64, 64, 256)  0           batch_normalization_22[0][0]     
__________________________________________________________________________________________________
batch_normalization_14 (BatchNo (None, 128, 128, 32) 128         conv2d_14[0][0]                  
__________________________________________________________________________________________________
conv2d_8 (Conv2D)               (None, 256, 256, 32) 27680       concatenate_1[0][0]              
__________________________________________________________________________________________________
concatenate (Concatenate)       (None, 512, 512, 64) 0           dropout_1[0][0]                  
                                                                 conv2d_transpose[0][0]           
__________________________________________________________________________________________________
conv2d_23 (Conv2D)              (None, 64, 64, 256)  590080      leaky_re_lu_22[0][0]             
__________________________________________________________________________________________________
leaky_re_lu_14 (LeakyReLU)      (None, 128, 128, 32) 0           batch_normalization_14[0][0]     
__________________________________________________________________________________________________
batch_normalization_8 (BatchNor (None, 256, 256, 32) 128         conv2d_8[0][0]                   
__________________________________________________________________________________________________
conv2d_4 (Conv2D)               (None, 512, 512, 32) 18464       concatenate[0][0]                
__________________________________________________________________________________________________
batch_normalization_23 (BatchNo (None, 64, 64, 256)  1024        conv2d_23[0][0]                  
__________________________________________________________________________________________________
conv2d_15 (Conv2D)              (None, 128, 128, 32) 9248        leaky_re_lu_14[0][0]             
__________________________________________________________________________________________________
leaky_re_lu_8 (LeakyReLU)       (None, 256, 256, 32) 0           batch_normalization_8[0][0]      
__________________________________________________________________________________________________
batch_normalization_4 (BatchNor (None, 512, 512, 32) 128         conv2d_4[0][0]                   
__________________________________________________________________________________________________
leaky_re_lu_23 (LeakyReLU)      (None, 64, 64, 256)  0           batch_normalization_23[0][0]     
__________________________________________________________________________________________________
batch_normalization_15 (BatchNo (None, 128, 128, 32) 128         conv2d_15[0][0]                  
__________________________________________________________________________________________________
conv2d_9 (Conv2D)               (None, 256, 256, 32) 9248        leaky_re_lu_8[0][0]              
__________________________________________________________________________________________________
leaky_re_lu_4 (LeakyReLU)       (None, 512, 512, 32) 0           batch_normalization_4[0][0]      
__________________________________________________________________________________________________
dropout_15 (Dropout)            (None, 64, 64, 256)  0           leaky_re_lu_23[0][0]             
__________________________________________________________________________________________________
leaky_re_lu_15 (LeakyReLU)      (None, 128, 128, 32) 0           batch_normalization_15[0][0]     
__________________________________________________________________________________________________
batch_normalization_9 (BatchNor (None, 256, 256, 32) 128         conv2d_9[0][0]                   
__________________________________________________________________________________________________
conv2d_5 (Conv2D)               (None, 512, 512, 32) 9248        leaky_re_lu_4[0][0]              
__________________________________________________________________________________________________
conv2d_transpose_7 (Conv2DTrans (None, 128, 128, 128 131200      dropout_15[0][0]                 
__________________________________________________________________________________________________
dropout_11 (Dropout)            (None, 128, 128, 32) 0           leaky_re_lu_15[0][0]             
__________________________________________________________________________________________________
leaky_re_lu_9 (LeakyReLU)       (None, 256, 256, 32) 0           batch_normalization_9[0][0]      
__________________________________________________________________________________________________
batch_normalization_5 (BatchNor (None, 512, 512, 32) 128         conv2d_5[0][0]                   
__________________________________________________________________________________________________
concatenate_7 (Concatenate)     (None, 128, 128, 288 0           conv2d_transpose_7[0][0]         
                                                                 dropout_6[0][0]                  
                                                                 dropout_11[0][0]                 
__________________________________________________________________________________________________
dropout_7 (Dropout)             (None, 256, 256, 32) 0           leaky_re_lu_9[0][0]              
__________________________________________________________________________________________________
conv2d_transpose_4 (Conv2DTrans (None, 256, 256, 32) 4128        dropout_11[0][0]                 
__________________________________________________________________________________________________
leaky_re_lu_5 (LeakyReLU)       (None, 512, 512, 32) 0           batch_normalization_5[0][0]      
__________________________________________________________________________________________________
conv2d_24 (Conv2D)              (None, 128, 128, 128 331904      concatenate_7[0][0]              
__________________________________________________________________________________________________
concatenate_4 (Concatenate)     (None, 256, 256, 128 0           dropout_3[0][0]                  
                                                                 dropout_7[0][0]                  
                                                                 conv2d_transpose_4[0][0]         
__________________________________________________________________________________________________
dropout_4 (Dropout)             (None, 512, 512, 32) 0           leaky_re_lu_5[0][0]              
__________________________________________________________________________________________________
conv2d_transpose_2 (Conv2DTrans (None, 512, 512, 32) 4128        dropout_7[0][0]                  
__________________________________________________________________________________________________
batch_normalization_24 (BatchNo (None, 128, 128, 128 512         conv2d_24[0][0]                  
__________________________________________________________________________________________________
conv2d_16 (Conv2D)              (None, 256, 256, 32) 36896       concatenate_4[0][0]              
__________________________________________________________________________________________________
concatenate_2 (Concatenate)     (None, 512, 512, 96) 0           dropout_1[0][0]                  
                                                                 dropout_4[0][0]                  
                                                                 conv2d_transpose_2[0][0]         
__________________________________________________________________________________________________
leaky_re_lu_24 (LeakyReLU)      (None, 128, 128, 128 0           batch_normalization_24[0][0]     
__________________________________________________________________________________________________
batch_normalization_16 (BatchNo (None, 256, 256, 32) 128         conv2d_16[0][0]                  
__________________________________________________________________________________________________
conv2d_10 (Conv2D)              (None, 512, 512, 32) 27680       concatenate_2[0][0]              
__________________________________________________________________________________________________
conv2d_25 (Conv2D)              (None, 128, 128, 128 147584      leaky_re_lu_24[0][0]             
__________________________________________________________________________________________________
leaky_re_lu_16 (LeakyReLU)      (None, 256, 256, 32) 0           batch_normalization_16[0][0]     
__________________________________________________________________________________________________
batch_normalization_10 (BatchNo (None, 512, 512, 32) 128         conv2d_10[0][0]                  
__________________________________________________________________________________________________
batch_normalization_25 (BatchNo (None, 128, 128, 128 512         conv2d_25[0][0]                  
__________________________________________________________________________________________________
conv2d_17 (Conv2D)              (None, 256, 256, 32) 9248        leaky_re_lu_16[0][0]             
__________________________________________________________________________________________________
leaky_re_lu_10 (LeakyReLU)      (None, 512, 512, 32) 0           batch_normalization_10[0][0]     
__________________________________________________________________________________________________
leaky_re_lu_25 (LeakyReLU)      (None, 128, 128, 128 0           batch_normalization_25[0][0]     
__________________________________________________________________________________________________
batch_normalization_17 (BatchNo (None, 256, 256, 32) 128         conv2d_17[0][0]                  
__________________________________________________________________________________________________
conv2d_11 (Conv2D)              (None, 512, 512, 32) 9248        leaky_re_lu_10[0][0]             
__________________________________________________________________________________________________
dropout_16 (Dropout)            (None, 128, 128, 128 0           leaky_re_lu_25[0][0]             
__________________________________________________________________________________________________
leaky_re_lu_17 (LeakyReLU)      (None, 256, 256, 32) 0           batch_normalization_17[0][0]     
__________________________________________________________________________________________________
batch_normalization_11 (BatchNo (None, 512, 512, 32) 128         conv2d_11[0][0]                  
__________________________________________________________________________________________________
conv2d_transpose_8 (Conv2DTrans (None, 256, 256, 64) 32832       dropout_16[0][0]                 
__________________________________________________________________________________________________
dropout_12 (Dropout)            (None, 256, 256, 32) 0           leaky_re_lu_17[0][0]             
__________________________________________________________________________________________________
leaky_re_lu_11 (LeakyReLU)      (None, 512, 512, 32) 0           batch_normalization_11[0][0]     
__________________________________________________________________________________________________
concatenate_8 (Concatenate)     (None, 256, 256, 192 0           conv2d_transpose_8[0][0]         
                                                                 dropout_3[0][0]                  
                                                                 dropout_7[0][0]                  
                                                                 dropout_12[0][0]                 
__________________________________________________________________________________________________
dropout_8 (Dropout)             (None, 512, 512, 32) 0           leaky_re_lu_11[0][0]             
__________________________________________________________________________________________________
conv2d_transpose_5 (Conv2DTrans (None, 512, 512, 32) 4128        dropout_12[0][0]                 
__________________________________________________________________________________________________
conv2d_26 (Conv2D)              (None, 256, 256, 64) 110656      concatenate_8[0][0]              
__________________________________________________________________________________________________
concatenate_5 (Concatenate)     (None, 512, 512, 128 0           dropout_1[0][0]                  
                                                                 dropout_4[0][0]                  
                                                                 dropout_8[0][0]                  
                                                                 conv2d_transpose_5[0][0]         
__________________________________________________________________________________________________
batch_normalization_26 (BatchNo (None, 256, 256, 64) 256         conv2d_26[0][0]                  
__________________________________________________________________________________________________
conv2d_18 (Conv2D)              (None, 512, 512, 32) 36896       concatenate_5[0][0]              
__________________________________________________________________________________________________
leaky_re_lu_26 (LeakyReLU)      (None, 256, 256, 64) 0           batch_normalization_26[0][0]     
__________________________________________________________________________________________________
batch_normalization_18 (BatchNo (None, 512, 512, 32) 128         conv2d_18[0][0]                  
__________________________________________________________________________________________________
conv2d_27 (Conv2D)              (None, 256, 256, 64) 36928       leaky_re_lu_26[0][0]             
__________________________________________________________________________________________________
leaky_re_lu_18 (LeakyReLU)      (None, 512, 512, 32) 0           batch_normalization_18[0][0]     
__________________________________________________________________________________________________
batch_normalization_27 (BatchNo (None, 256, 256, 64) 256         conv2d_27[0][0]                  
__________________________________________________________________________________________________
conv2d_19 (Conv2D)              (None, 512, 512, 32) 9248        leaky_re_lu_18[0][0]             
__________________________________________________________________________________________________
leaky_re_lu_27 (LeakyReLU)      (None, 256, 256, 64) 0           batch_normalization_27[0][0]     
__________________________________________________________________________________________________
batch_normalization_19 (BatchNo (None, 512, 512, 32) 128         conv2d_19[0][0]                  
__________________________________________________________________________________________________
dropout_17 (Dropout)            (None, 256, 256, 64) 0           leaky_re_lu_27[0][0]             
__________________________________________________________________________________________________
leaky_re_lu_19 (LeakyReLU)      (None, 512, 512, 32) 0           batch_normalization_19[0][0]     
__________________________________________________________________________________________________
conv2d_transpose_9 (Conv2DTrans (None, 512, 512, 32) 8224        dropout_17[0][0]                 
__________________________________________________________________________________________________
dropout_13 (Dropout)            (None, 512, 512, 32) 0           leaky_re_lu_19[0][0]             
__________________________________________________________________________________________________
concatenate_9 (Concatenate)     (None, 512, 512, 160 0           conv2d_transpose_9[0][0]         
                                                                 dropout_1[0][0]                  
                                                                 dropout_4[0][0]                  
                                                                 dropout_8[0][0]                  
                                                                 dropout_13[0][0]                 
__________________________________________________________________________________________________
conv2d_28 (Conv2D)              (None, 512, 512, 32) 46112       concatenate_9[0][0]              
__________________________________________________________________________________________________
batch_normalization_28 (BatchNo (None, 512, 512, 32) 128         conv2d_28[0][0]                  
__________________________________________________________________________________________________
leaky_re_lu_28 (LeakyReLU)      (None, 512, 512, 32) 0           batch_normalization_28[0][0]     
__________________________________________________________________________________________________
conv2d_29 (Conv2D)              (None, 512, 512, 32) 9248        leaky_re_lu_28[0][0]             
__________________________________________________________________________________________________
batch_normalization_29 (BatchNo (None, 512, 512, 32) 128         conv2d_29[0][0]                  
__________________________________________________________________________________________________
leaky_re_lu_29 (LeakyReLU)      (None, 512, 512, 32) 0           batch_normalization_29[0][0]     
__________________________________________________________________________________________________
dropout_18 (Dropout)            (None, 512, 512, 32) 0           leaky_re_lu_29[0][0]             
__________________________________________________________________________________________________
conv2d_30 (Conv2D)              (None, 512, 512, 3)  99          dropout_18[0][0]                 
==================================================================================================
Total params: 8,340,483
Trainable params: 8,333,827
Non-trainable params: 6,656
```
