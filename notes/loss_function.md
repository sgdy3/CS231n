Loss Funcion and optimization
==========
# 1. loss function
We need a loss function to meaure our satisfication to a classifier.
Gieven a dataset of examples, loss over the dataset is a sum of loss over examples:
<center>

$L=\frac{1}{N}\sum\limits_{i}L_i(f(x_i,W),y_i)$<br>
$\{(x_i,y_i)\}^N_{i=1}$

</center>

## i. Multiclass SVM loss(Hinge loss)

<center>

$L_i=\sum_{j\ne y_i}max(0,s_i-s_{y_i}+1)$
</center>

This kind of loss comes up with with a requirment for our model that the score of correct class must larger than of other classes, or there will be penalty.
> Initutive thoughts: The constant "1" here seems a litte abrupt, as $s_i-s_{yi}$ is already enough to describe the gap between socres of corrcet class and other classes. I still can't find the proof of using "1" instead of other constans in multiclass svm loss. But I figuer out why "1" appears in binary-classification verion hinge-loss: $L=max(0,1-y^i(w^Tx+b))$. If we find an optimal solution $w,b$ to satisfy minimizing $L=max(0,1-y^i(w^Tx+b))$,then for problem $L'=max(0,80-y^i(w^Tx+b))$, the optimal soultion would be $80w, 80b$. However this dosen't change the decision boundary $w^Tx+b=0$. So the value of this constant is only relative to the scaling of $w$ and $b$, just like we discussed in funcional margin and geometric margin.
> All in all, we actually does'n care about the absloute scores of the loss function, we only care about the realtive difference between the scores, so maybe "1" in multiclass svm loss has the same usage. Another hypothesis is that "1" is used as a margin, requiring our prediction to have some "confidence".

**nature of hinge loss**
1. If the score of correct class was very high, a little change of it wouldn't make difference in loss.
2. min loss: 0; max loss: inf.
3. If $W$ was initialized to be small so all $s\approx 0$, the loss = k-1.(k in the number of classes)
4. If the sum was over all class, the loss would increase by 1. 
5. If we use mean instead of sum, it's meanless, as k is a fixed number.
6. If we computed the squared loos func like $\sum_{j\ne y_i}max(0,s_i-s_{y_i}+1)^2$, the loos func would change a lot. Use a squared loss or non-squared loos depends on our requirment, if we want to add more penalty in wores mistake then we'd like to use squared loss. If we think a much worse predicion may have the same weight in mild mistake, the we perfer non-squared loss.

## ii. softmax loss func
softmax loss func use a probablistic way to describe our optimizaion goal. Suppose the predicted value of $Y$ is an random variable with a PMF: $P(Y=k|X=x_i)=\frac{e^{sy_i}}{\sum_{j}e^{sj}}$. Then for the ground truth of $Y$, we have: $P(Y=y_i|X=x_i)=1$, then we can caculate KL distance between this 2 distribution and minmize it. Then we can get softmax loss func.

$L_i=-log(\frac{e^{sy_i}}{\sum_{j}e^{sj}})$

**nature of softmax loss**<br>
1. min loss: 0 (can't reach)); max loss:inf 
2. If $W$ is initialized to be small so all $s\approx 0$, the loss: $logk$

**comparison between hinge-loss and softmax-loss**<br>
1. Suppose we take a datapoint and we jiggle a bit (changing its score slightly) and the score of correct class is already high, Hinge-loss wouldn't change as softmax loss would.
2. Hinge-loss only care about ensuring the correct score to be greater than a margin above the incorrect scores. Softmax-loss always want the correct score to be much higher to drive the mass funciont to  1.

# 2.Regularization Term
When we apply our loss func in the traing-set, we actually want to train a model have good performance in the test-set rather than in training-set. So we need to add some constraints to our model to make it have much strong generalization ability, which means the model should be a simple one according to Occam's Razor theory. So a more general form for loss func is:
<center>

$L(W)=\frac{1}{N}\sum\limits_{i=1}^NL_i(f(x_i,W),y_i)+\lambda R(W)$
</center>

**common regularization term**<br>
1. L2 regularization
2. L1 regularizaion
3. Elastic net
4. Max norm regularization
5. Dropout
> L2 regularizaiont tends to equalize all terms in $W$. In contrast, L1 regularization tends to make $W$ an sparse one. e.g. $w1=[1,0,0,0],w2=[0.25,0.25,0.25,0.25]$, L1 prefer $w1$ as L2 prefer $w2$.

# 3. Optimizaion
1. random search
2. follow the slope (gradient). gradient descent and stochasic gradient descent.

# 4.Image Features
Our input $X$ was  raw pixels of image before, we can actually introduce some image features to represent a image.
1.  Color Histogram
2.  Histogram of Oriented Gradients (HoG)
3.  Bag of Words