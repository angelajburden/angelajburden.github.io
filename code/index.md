[home](#home)

## <a name="home"></a>Code

[neural network code](https://github.com/angelajburden/QSO_neural_network)

The main code that runs the neural network can be found here
 
## neural_network_galaxy.py

In this part of the code you can change the lambda (regularisation) parameter, the number of nodes in the hidden layers, the and number of iterations reuired to train the network.   

The parameters we need to specify are:

**input_layer_size**: Number of attributes of our data set that we wish to use to make a prediction of the classification of the data. In this case there are 10 (see link..data)

**hidden_layer_size**: Number of nodes in each hidden layer

**num_labels**: Number of output classifications (in out case 1 which will be 1 for QSO and 0 for PLO)

**lamparam**: Lambda value, the regularisation parameter.

The functions called in the main code are in

## NN_functions_param.py

    + CostFunction        
        #compute the cost function

    
    #compute the gradient

  


randInitializeWeights(L_in, L_out):
    W = np.zeros((L_out, 1 + L_in))
    INIT_EPSILON = 0.1
    W = np.random.random((L_out,1 + L_in)) * (2 * INIT_EPSILON) - INIT_EPSILON
    return W

def predict(Theta1, Theta2, Theta3, X):
    m = X.shape[0]
    input_layer_size = X.shape[1]
    p = np.zeros((m, 1))
    X1s = np.ones((m,1))
    X = np.append(X1s,X, 1)   
    h1 = sigmoid(np.dot(X,Theta1.transpose()))    
    h1 = np.append(X1s,h1, 1)     
    h2 = sigmoid(np.dot(h1,Theta2.transpose())) 
    h2 = np.append(X1s,h2, 1)     
    h3 = sigmoid(np.dot(h2, Theta3.transpose()))    
    return h3

sigmoid(z):
    g =  1./(1 + np.exp (-z))
    return g  

sigmoidGradient(z):    
    x = 1./(1 + np.exp(-z))
    return np.multiply(x, (1.-x))
    
trainReg(X, y,input_layer_size, hidden_layer_size,num_labels, lamparam, it_no):
    print('\nInitializing Neural Network Parameters ...\n')
    print('reloaded\n')
    initial_Theta1 = randInitializeWeights(input_layer_size, hidden_layer_size)
    initial_Theta2 = randInitializeWeights(hidden_layer_size, hidden_layer_size)
    initial_Theta3 = randInitializeWeights(hidden_layer_size, num_labels)
    #Unroll parameters
    tTheta1 = initial_Theta1.flatten()
    tTheta2 = initial_Theta2.flatten()
    tTheta3 = initial_Theta3.flatten()
    tTheta1 = np.reshape(tTheta1,(tTheta1.shape[0],1))
    tTheta2 = np.reshape(tTheta2,(tTheta2.shape[0],1))
    tTheta3 = np.reshape(tTheta3,(tTheta3.shape[0],1))
    initial_nn_params = np.vstack((tTheta1,tTheta2,tTheta3))
    print('\nTraining Neural Network... \n')
    args = (input_layer_size,hidden_layer_size,num_labels, X, y, lamparam)
    print("define args\n")
    nn_params2 = scipy.optimize.fmin_cg(cost, x0=initial_nn_params, args=args, fprime=grad, maxiter=it_no, full_output=1)
    nn_params2 = np.array(nn_params2)
    nn_params = nn_params2[0]
    print("returning optimised parameters\n")
    return nn_params
    
Several functions commented out. These are the graient checking algorithm that checks the code is doing what it is meant to, the validaion curve algorithm that outputs the accuracy of the cross validation and training sets for different lambda values and the learning curve algorithm that outputs the cost function of the training and cross validation sample when the network is trained on different sized training data sets.
