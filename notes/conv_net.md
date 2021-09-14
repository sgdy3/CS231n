convolutional neural network
==========

# Convolutional layer
Let a filter slide over all the location on the input image, caculate the dot prouducts between filter and the region it covers. Through this process, we can get the output image after convolutional layer. General, the filter should have the same depth as input image, and we always apply mulitple filters to do the convolution and stack the output image up to synthesize a deep one.<br>
There are some other details about convolution:<br>
1. In signal processing, for convolutional operation, we should rotate the filter by 180 degree first. Instead, we'd call this procedure "correlation". But we don't excute this procedure, this may because the values in the filter are actually initiallized randomly, so the locations of values don't change anything. 
2. If we slide the filter over the primal image, we'll find that the the size of output image is no longer it of input image as there aren't enough pixels for filter to conver at the border (left side,bottom .etc). So to maintain the size, we usually pads the border in different form like padding with zero.
3. Sliding may means move one step at a time. But sometimes we want to downsample the image to make the output a smaller one. In this case, we'll need fewer parameters in the next layer. To acompish this goal, we can slide the fileter with other stride istead of 1. 
4. With padding $P$ and specified strides$S$, we can derive the size of output image:$W'=(W-F+2P)/S+1,H'=(H-F+2P)/S+1$.
5. Comparision with full connected layer, con-lay preserve the sptial structure of inputy image. Each value of the output image is conntected with a region in the input image. Through serval filter, we can get different imformation froms the same region. 
# Convolutional net
ConvNet is a sequence of Convolution Layers, interspersed with activation functions. In the low-level, the activation map will present some simple features like edges. With layer's level becoming much higher, the features it contains will be much comlicated that we may not able to interperte. This is beacause the input of high-level layer, the input image is alread sets of primal features, after aggregation, it's natural to genertae a complex feature mapping. 
# Fully connected layer
The same form with linear regeression: $W^Tx$. The object of applying Fully connected layer is very simple--sum and weight all features.
# Pooling layer
Pooling layed is used to downsample the image, have the same effect with stride we mentioned berfore. Common form of pooling layer is max pooling, which meas replace a region with the maxmum value in it. A intuitive thought of this process is that by applying pooling, we actually want to represent a region in much resource-saved way, and the value corresbond to the degree reosponding to the filter. So, taking the maximum value is trying to maintain the response to the greatest extent possible.For example, in low-level layer, we can get the most prominent edge information by doing so. Another popular to do the pooling is average pooling.<br>
According to cs231n, as sliding the filter in much bigger stride can also dowsample the image, resarcher have alreadly started to replace pooling layer with lager stride in conv-layer.

# Summary
In this section, we've learned conv-layer and how to compute the size of it's output image and the number of parameters need to set.
Besides, we also got to konw the intuitive thought of conv-layer. At last, we get in touch with other layes in convulution neural network.