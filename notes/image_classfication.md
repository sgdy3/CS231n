Image classification
=========

# 1.Image classfication problem
1.  Viewpoint Variation
2.  Illumination
3.  Deformation
4.  Occlusion
5.  Background clutter
6.  Intraclass variation
# 2. Image classifier
Usually to get a  imamge classfier contains 2 parts.<br>
Traning part: use traing-set to train a model.<br>
Predicting part: use the model to do the classification on test-set.
# 3. Nearest Neighbor Classifier
## i.primal version
Nearest Neighbor is a very simple classifier. In traning part, it just remeber all trainging images and their labels. In prediction part, we'll predict the label depened on the most similar image in traning-set. To define the "simlar", we need to caculate distance between two images. We have 2 common distance mertric<br>
L1 (Manhattan) distance:
<center>

$d(I_1,I_2)=\sum\limits_{p}|I_1^P-I_2^P|$
</center>

L2 (Euclidean) distance:
<center>

$d(I_1,I_2)=\sqrt{\sum\limits_{P}(I_1^P-I_2^P)^2}$
</center>

> L1 distance depends on the choice of coordinate. The deformation of coordinate will change the L1 distance. In contrast, L2 distance has nothing to do with corrdinate. So if some our input features have specified meaing, then we might choose L2 distance. However, we had better use both of the distance and choose the one had better performance in validation-set.
## ii. k-nearest
Intuitively,  asserting a sample's label only based on the nearest neighbor in training-set is not a good decision. A little depature or disturbing will cause huge different. So a much reasonable choice is to do the decision baed on K most nearest neighbors. This will make our decition boundary much smooth and rudeuce the risk of random noise.

## iii. backwards
In fact, nearest neighbor is rarely used in image classification.<br>
1. Costly, we need to caculate distance between every test-samples and training-set.
2. Distance metrics on pixel is not imformative, different images may have the same distance with a fidiucial pic.
3. Curse of dimensionality, to find the most nearest neighbor, we must have enough neighbors to fill the feature space. Otherwise, we can't ensure a new sample's nearest neighbor (the distance may need to less than a datum.)
# 4.Fold strategy
As we introduced before, we need to decide the distance and "K" we use in prediction. These facetors are called hyper-parameters.
Directly applying the options in training-set and use the one has best performance is not a wise choice, as it will chose the option most suitable for traing-set. For exameple ,"k" would be alwys equal to 1 as the model alread contains the sample we used to predice. How about apply these options in test-set ? It's not a good choice either. As test-set is used to meaurse the performance of model, if we use test-set to choos the hyper-parameters, then we'll always get a satisfying but cheating grades.
So except traing-set do the model training, test-set do the evaluation, we need another set called validation-set to help with choosing hyper-parameters.
## i. leave-out validation
Divide out input datas in 3 stable parts: traing-set, validatino-set, test-set.
## ii. Cross validation
Divide out input datas in 2 parts: training-set, test-set. While training, we will split the training-set in k parts, then cycle through the choice of which is the validation part.After k times caculations, take average of the performace to tune the hyper-parameters. This way cause lots of time and resourse, so it's suitable for small datas.

# 5. Linear Classifier
Beside **data-driven** image classifier like Nearest Neighbor classifier, there exits another sort of classifier called **parametric approach** classifier. It's general form like:
<center>

$f(x,W)$<br>
$x$ is the image, $W$ is parameters
</center>
A simple parametric approach image classifer is linear classifier.
<center>

$f(x,W,b)=Wx+b$ <br>
$x\in R^n,W\in R^{k*n},b\in R^k$<br>
$n$ is the size of flattnn pic; $k$ is the number of class.
</center>

## i. interpretation
We can understand linear classifier from 2 perspectives. 

### a). templae matching 
Each row of $W$ can been seen as a typcial pic of $i_{th}$ class. When we caculater the inner product of input flatten image with $W$, we actually measure the similarity between input image with these typical pics. $b$ is the bias is introduced to remedy the category imbalance problem.
### b). high dimension point view
Every $x$ is a point in feature space. By caculating $W^{iT}x+b$, we draw a decision decison boudary in feature space to judge an point belons to one class or not. So with $Wx+b$, we split feature space into different parts linearly, each part may represent one class.
## ii. hard cases 
There exists some hard cases for a linear classifier.(nonlinear)
1. multimodel data
2. parity probelm (Xor)
3. torodial data 
