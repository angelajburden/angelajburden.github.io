[home](https://github.com/angelajburden/angelajburden.github.io/blob/master/index.md)

## Results

+ From the results below we chose our lambda value to be 0.5, the number of nodes in the hidden layer to be 50 and although 1000 iterations did not give much improvement over 500, we chose to do 1000 iterations as the code only took about 5 mins to run.

{:.center}
![alt text](/images/params_NN.png "parameters"){:height="60%" width="60%"}

+ For 10 data input parameters, lambda = 0.5, 2 hidden layers with 50 nodes each, a positive classification value defined as y_NN > 0.5 and 1000 iterations we recovered a test set accuracy of 92%.

+ The classifications are shown below. The blue are the true positive results i.e. correctly guessed QSO, the green are the false positive, that is QSO that our network missed. The red are the correctly identified PLO (True negative) and the black are the PLO that were wrongly classified as QSO.

{:.center}
![alt text](/images/hist_results_FT_PN2.jpg "classificartions"){:height="60%" width="60%"}

+ To look more closely at our results, the following plots show the true positive (top left), true negative (bottom left), false negative (top right), and false positive (bottom right) as contour plots as a function of their colours.

{:.center}
![alt text](/images/contour_ratios "contour_colour"){:height="60%" width="60%"}

+ It looks like the incorrectly identified objects (RHS) are just too degenerate with the other class.

+ Finally we show the efficiency of the algorithm when we change the positive classification limit from >0.5 to a range of values. The efficiency is defined as the fraction of objects in a particular class with y_NN > y_NN_min.

{:.center}
![alt text](/images/efficiency_plot.png "efficiency"){:height="60%" width="60%"}
