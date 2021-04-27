---
layout: page
title: Dynamic OD Prediction in Transit Systems
description: A Multiresolution Spatio-Temporal Deep Learning Approach
img: /assets/img/od_framework.png
importance: 2
category: work
---

Short-term demand predictions, typically defined as less than an hour into the future, are important for implementing
dynamic control strategies and providing useful customer information
in transit applications. Knowing the expected demand enables transit operators to deploy real-time control strategies
in advance of the demand surge, and minimize the impact of
abnormalities on the service quality and passenger experience.


One of the most useful applications of demand prediction models
in transit is in predicting the congestion on station platforms and
crowding on vehicles. These require information about the origindestination
(OD) demand, providing a detailed profile of how and
when passengers enter and exit the service. However, existing
work in the literature is limited and overwhelmingly focuses
on forecasting passenger arrivals at stations. This information,
while useful, is incomplete for many practical applications. We
address this gap by developing a scalable methodology for realtime,
short-term OD demand prediction in transit systems. 

Our
proposed model consists of three modules: multiresolution spatial
feature extraction module (MRSFE) for capturing the local spatial
dependencies, auxiliary information encoding module (AIE),
and a module for capturing the temporal evolution of demand.
The OD demand at time t, represented as a NN matrix, is first
passed to MRSFE. Inside the module, three convolutional neural
network layers are utilized to learn the spatial dependencies
from the OD demand directly. Additionally, we use the discrete
wavelet transform (DWT) to capture different time and frequency
variations in the data. We then use a squeeze-and-excitation
layer to weight feature maps based on their contribution to
the final prediction. A Convolutional Long Short-term Memory
network (ConvLSTM) is then used to capture the temporal
evolution of demand. The approach is demonstrated through a
case study using 2 months of Automated Fare Collection (AFC)
data from the Hong Kong Mass Transit Railway (MTR) system.
The extensive evaluation of the model shows the superiority of
our proposed model compared to the other compared methods.


<div class="row">
    <div class="col-m mt-12 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/od_framework.png' | relative_url }}" alt="" title="example image"/>
</div>
  <!--   <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/3.jpg' | relative_url }}" alt="" title="example image"/>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/5.jpg' | relative_url }}" alt="" title="example image"/>
    </div> -->
</div>
<div class="caption">
    Fig. 1: Overview of the proposed model. It consists of three modules: mulltiresolution spatial feature extraction module
(MRSFE) for capturing the local spatial dependencies, auxiliary information encoding module (AIE), and a module for capturing
the temporal evolution of demand (ConvLSTM).
</div>


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/dwt.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>



The first layer utilizes convolutional neural networks to capture the spatial interactions. Each $$\mathbf{OD}_{t-h} ^{exits}, h = {H \dots 0}$$ is passed through two branches. In the first branch, it is passed through 3 CNN blocks, each comprising a convolutional layer, followed by a batch normalization layer and a ReLU activation function. The number of filters in the 3 CNN blocks are 64, 128, and 128, respectively, with kernel sizes of $3 \times 3$. Since we want the output to have the same dimensionality as the input, we use stride of size 1 and no pooling. The output of this branch, $$\boldsymbol{F_1}$$ is thus calculated as
$$
    \boldsymbol{F_1} = \operatorname{Conv}(\mathbf{OD}_{t-h} ^{exits}, \mathbf{W_1})
$$
where $$\mathbf{W_1}$$ denotes the weights of the convolutional layer. 
In a separate branch, a 1-level 2D-DWT decomposes $$\mathbf{OD}_{t-h} ^{exits}$$ into four sub-bands to capture different time and frequency variations in the data: LL (approximation matrix), LH (horizontal matrix), HL (vertical matrix) and HH (diagonal matrix). In this paper, we use Daubechies 2 as the mother wavelet. 
Each sub-band is further processed by a CNN with 64, 3x3 filters with stride of 1, followed by a batch normalization layer and a ReLU activation function. 

The output of this branch, $$\boldsymbol{F_2}$$, is thus:
$$
    \boldsymbol{F_2} = \operatorname{Conv}(LL, \mathbf{W_2}) \oplus \operatorname{Conv}(HL, \mathbf{W_3}) \oplus \nonumber \\
    \operatorname{Conv}(LH, \mathbf{W_4}) \oplus \operatorname{Conv}(HH, \mathbf{W_5}) 
$$

where $$\oplus$$ denotes the concatenation operation. The learned feature maps from the two branches are then concatenated. We also use a skip connection to avoid the vanishing gradient problem. 
These blocks together extract increasingly complex hierarchical demand patterns from local spatial dependencies among the OD pairs/

<div class="row justify-content-sm-center">
    <div class="col-m-4 mt-6 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/maps.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
    (a) Observed OD matrix at 7 am, 8 am, 9 am, 5 pm, and 7 pm for one day in the test set, from left to right. (b) 15-min ahead prediction of the OD matrix for the same time and day
</div>


<div class="row">
    <div class="col-m mt-12 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/predictions 01_10_2018.gif' | relative_url }}" alt="" title="example image"/>
</div>
<div class="caption">
     
</div>



<div class="row justify-content-sm-center">
    <div class="col-m-4 mt-6 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/ts.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
    Demand patterns for high (left), medium (right), and low demand OD pairs for one week in the test set. The red lines are the predicted result and the blue lines denote the ground truth.
</div>

<!-- The code is simple.
Just wrap your images with `<div class="col-sm">` and place them inside `<div class="row">` (read more about the <a href="https://getbootstrap.com/docs/4.4/layout/grid/" target="_blank">Bootstrap Grid</a> system).
To make images responsive, add `img-fluid` class to each; for rounded corners and shadows use `rounded` and `z-depth-1` classes.
Here's the code for the last row of images above:

```html
<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/6.jpg' | relative_url }}" alt="" title="example image"/>
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/11.jpg' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
``` -->
