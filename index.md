


## Context
+ To measure properties of our universe galaxy surveys set up large telescopes to observe the light of millions of objects. By measuring the distribution of different classes of objects we can infer different things about the universe such as what it is made up of. 

+ We can measure the light using two different methods, photometric and spectroscopic observations. 

    - Photometic observations measure the amount of light passing through coloured filters. Therefore you will get the intensity of light but only as a function of a few wavelengths. However you can observe many objects at once.

    - For spectroscopic observations the light travels through a spectrometer and you recover the continuous intensity of light as a function of wavelength. For each object you get a lot more information but this method is more expensive and not as many objects can be observed at one time.

+ As spectroscopic observations are more expensive we want to be sure that we point our optical fibers on the objects that we wish to obtain spectra for. Therefore we first conduct photometric observations of mny objects in the region of the sky we are interested in, and from these measurement try and distinguish between objects so that we know the positions of objects we wish to target with our spectroscopic survey.


## Aim 
Given a list of objects with similar photometric attributes, i.e. no distinct characteristics that we can use to make cuts, train a neural network to identify quasars from other objects. 

## Data
+ The data is publicly available here 

http://skyserver.sdss.org/dr7/en/tools/search/sql.asp

+ We choose 30000 known QSO (quasars) and their properties using the SQL commands

```
SELECT TOP 30000 s.z,s.zErr, s.specClass, p.psfMag_u, p.psfMag_g, p.psfMag_r, p.psfMag_i, p.psfMag_z, p.psfMagErr_u, p.psfMagErr_g, p.psfMagErr_r,p.psfMagErr_i,p.psfMagErr_z FROM BESTDR7 as s

JOIN BESTDR7 AS p ON s.bestObjID = p.objID

WHERE (s.specClass = dbo.fSpecClass('QSO') OR s.specClass = dbo.fSpecClass('HIZ_QSO')) AND s.zConf>0.35 AND s.z>0.5 AND s.z<5 AND p.psfMag_u>0 AND p.psfMag_u<26 AND p.psfMag_g>19 AND p.psfMag_g<25 AND p.psfMag_r>0 AND p.psfMag_r<24 AND p.psfMag_i>0 AND p.psfMag_i<24 AND p.psfMag_z>0 AND p.psfMag_z<24 AND p.psfMagErr_u>0 AND p.psfMagErr_g>0 AND p.psfMagErr_r>0 AND p.psfMagErr_i>0 AND p.psfMagErr_z>0

```

+ We choose 30000 other point-like objects (PLO) using the following commands


```
SELECT TOP 30000 s.z,s.zErr, s.specClass, p.psfMag_u,p.psfMag_g,p.psfMag_r,p.psfMag_i,p.psfMag_z, p.psfMagErr_u,p.psfMagErr_g,p.psfMagErr_r,p.psfMagErr_i,p.psfMagErr_z FROM BESTDR7 as s

JOIN BESTDR7 AS p ON s.bestObjID = p.objID

WHERE (s.specClass = dbo.fSpecClass('STAR')) AND p.psfMag_u>0 AND p.psfMag_u<26 AND p.psfMag_g>19 AND p.psfMag_g<25 AND p.psfMag_r>0 AND p.psfMag_r<24 AND p.psfMag_i>0 AND p.psfMag_i<24 AND p.psfMag_z>0 AND p.psfMag_z<24 AND p.psfMagErr_u>0 AND p.psfMagErr_g>0 AND p.psfMagErr_r>0 AND p.psfMagErr_i>0 AND p.psfMagErr_z>0 

```

The samples have the same data cuts in their photometric properties. 

+ The samples are labelled 1 for QSO and 0 for PLO and are combined, shuffled and split into a training-set, cross-validation set and a test-set with 0.5, 0.25 and 0.25 of the data respectively.

+ In astronomy we define the colours of an object to be the difference in the values between two bands for example 
psfMag_u - psfMag_g above is the u-g band. The u-g, g-r, r-i, i-z colours are added to the data.

+ In the neural network we initially use 10 input parameters (following https://arxiv.org/abs/0910.3770 ). They are the 4 colours defined above, the g magnitude and the 5 magnitude errors i.e. psfMagErr_z etc.

+ To show that the objects cannot be separated with data cuts we show plots of the input parameters of each object below using the training sample. Red objects are the QSO and blue are the PLO. 

### Training set input parameters

+ The 10 input parameters of each object are hown in the two plots below. The QSOs are in red and the PLOs in blue. Clearly they cannot be identified by applying simple cuts to any of these attributes.

![alt-text-1](/images/col_col.jpg "colours"){:height="51.8%" width="51.8%"}![alt-text-2](/images/hist_cats.jpg "colour errors"){:height="48%" width="48%"}


## Method

The neural network is set up as shown in the figure below.

The network is trained on the training set. The procedure is as follows

1. Decide on the input parameters for the data (**x**), the number of hidden layers and the number of nodes in each hidden layer.

2. Initialise the weights of the network. These will be randomly generated numbers, our starting point. As the network runs these weights become optimised as the neural network learns which information is important to correctly guess the catagory of the object. The weights connect each node in the previous layer to each node in the present layer, thus the weights are a matrix with size (layer_i x (layer_i+1 +1)) NB there is an extra node added in layer_i+1 which is the zero node.

3. Using the initial weights, compute how well the network predicts the true outcome. 
For layers 1 to 4 (i.e input, 2xhiddenlayers and output) we compute the following (the superscript denotes the layer number): 

   $$ \color{black}
    \begin{align*} 
    & a^{(1)} = \mathbf{x},\\
    & z^{(2)} = \Theta^{(1)}a^{(1)}, \quad a^{(2)} = \frac{1}{(1 + \exp{-z^{(2)}})}, \quad \textrm{add}\quad a_0^{(2)} \\
    & z^{(3)} = \Theta^{(2)}a^{(2)}, \quad a^{(3)} = \frac{1}{(1 + \exp{-z^{(3)}})}, \quad \textrm{add}\quad a_0^{(3)} \\
    & z^{(4)} = \Theta^{(3)}a^{(3)}\\
    & y_{NN} = a^{(4)} = \frac{1}{(1 + \exp{-z^{(4)}})}\\
    \end{align*}
   $$

    - Different functions can be used here but we use the cost function to descibe how close our model is to predicting the correct classification. It is defined as

   $$ \color{black} J(\mathbf{\Theta}) = -\frac{1}{m} \sum_{i=1}^{m} y^i \log y_{NN}(\mathbf{\Theta})^i + (1-y^i)\log(1-y_{NN}   (\mathbf{\Theta})^i) + \frac{\lambda}{m}\sum_{i=1}^{L-1}\mathrm{tr}(\mathbf{\Theta}^T\mathbf{\Theta})$$

    - where m is the number of data points in the sample, L are the total number of layers and

   $$ \color{black} \mathrm{tr}(A^T A) = \sum_{i=1}^n \sum_{j=1}^m a_{i,j}^2 $$

4. To reduce the cost function and train the network we then work backwards (back propagation) to compute the gradient of the cost function which we can feed in our our optimiser inorder for it to search for the Theta parameters that return minimum cost and therefore best model the training data.

    - We compute a delta for layers L down to 2.

   $$ \color{black} \delta^{(L)} = y_{NN} - y$$

   $$ \color{black} \delta^{(L-i)} = \mathbf{\Theta}^{(L-i) \mathbf{T}} \delta^{(L-i+1)}  a^{(L-i)}(1-a^{(L-i)})$$

    - The gradient at each layer (l) and each unit (j) in that layer is 

   $$ \color{black}
    \frac{\partial J}{\partial \Theta_j} = \sum_{i=1}^{m} \frac{1}{m} a_j^{(l)}\delta^{(l+1)} + \sum_{i=1, j\neq 0}^{m}\lambda \Theta_j^{(l)}.
   $$

5. With the cost function and gradient of cost function algorithm we can now take advantage of built-in optimisation routines in python. I have used the scipy.optimize.fmin_cg function.

6. Now the neural network is set up we can test that the gradient are being computed correctly by running a small sample through our gradient computation routine and comparing the outcome with that from a simple finite difference routine.

7. If that looks good the next step is computing the best regularisation parameter Lambda, and the number of nodes required in the hidden layers using the cross-validation data set.

8. We chose the values of Lambda and the number of hidden layer nodes as those that give the lowest cost-function/highets accuracy on our cross-validation data set.

9. Now we can train the network using these values and see how well it performs on the test data set as a function of the number of iterations reuired by the optimisation function.


## Code

[neural network main code](https://www.google.com)

[neural functions code](https://www.google.com)

## Results

![alt text](/images/params_NN.jpg "colours")



