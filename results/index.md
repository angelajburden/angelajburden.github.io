{: id="results_banner"}
## Results

+ From the results below we chose our lambda value to be 0.5, the number of nodes in the hidden layer to be 50 and although 1000 iterations did not give much improvement over 500, we chose to do 1000 iterations as the code only took about 5 mins to run.

{:.center}
![alt text](/images/params_NN.png "parameters")

+ For 10 data input parameters, lambda = 0.5, 2 hidden layers with 50 nodes each, a positive classification value defined as y_NN > 0.5 and 1000 iterations we recovered a test set accuracy of 92%.

+ The classifications are shown below. The blue are the true positive results i.e. correctly guessed QSO, the green are the false positive, that is QSO that our network missed. The red are the correctly identified PLO (True negative) and the black are the PLO that were wrongly classified as QSO.

{:.center}
![alt text](/images/hist_results_FT_PN2.jpg "classificartions")

+ To inspect the locations in parameter space where the neural network fails the plot below shows: 
the true distributions (grey underlying contours), the true positive (top left), true negative (bottom left), false negative (top right), and false positive (bottom right) results as colourful contours. The plots to the LHS show where the NN has guessed correctly and on the right show the incorrect guesses. From the plots on the right, it is clear to see why the NN made the wrong choice for a subset of the test data which doesn't lie in the same parameter space as the majority of that class (i.e. the contours are misaligned).

{:.center}
![alt text](/images/TP_contours.jpg "contour_colour")

+ The incorrectly identified objects (RHS) are too degenerate with the other class.

+ Finally we show the efficiency of the algorithm when we change the positive classification limit from >0.5 to a range of values. The efficiency is defined as the fraction of objects in a particular class with y_NN > y_NN_min.

{:.center}
![alt text](/images/efficiency_plot.png "efficiency")
