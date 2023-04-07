# Regression-for-EE399
By Riley Estes
https://github.com/rileywe/Regression-for-EE399/
##### Abstract:
This program explores the effects of model complxity, initial parameter guesses, overfitting and minima location on machine learning regression/curve fitting models. Overall, less complexity is more reliable, but when a lot can be known about the input data, a more complex model yields better error results.

### Introduction and Overview
This project aims to complete three tasks:<br>
<ol>
  <li>Fit a X/Y graph dataset of 30 points using the function f(x) = Acos(Bx) + Cx + D and least-squares error by minimizing the error with parameters A, B, C, and D.</li>
  <li>Using the discovered parameters that generate a local minima and least-squares error, hold two of the parameters and sweep through different values for the other two in order to create a 2D array of the error at each parameter combination. These arrays can be graphed as a color map to graphically show the nearby minima.</li>
  <li>Fit a linear, parabolic, and 19th degree polynomial to the same set of data but with two different data partitions. The first partition uses the first 20 points as training data, and the last 10 as test data. The second partition uses the first and last 10 points as training data, and the middle 10 as test data. The least-squares errors are calculated in both cases for each of the three models and compared.</li>
</ol>
<br>
The program uses the numpy, scipy.optimize, and matplotlib.pyplot libraries. 
Using the algorithms in these packages, this program is able to explore and show how different line-fitting functions and models interact with the given data. Notably, the effect of initial values, function choice, data distribution, data selection, and error minimization algorithms are explored in this program.

### Theoretical Background
##### Model Complexity: 
In machine learning applications, regression models are often used to guess at a model for an observed set of data. It's the task of finding an equation that can very accurately produce the same observed results given the same input that created the observed data set. Linear, or first-order models are the one of the simplist, and with only 2 parameters and no complicated derivatives, are very easy to solve. Higher order polynomials require more processing power, yet are still not very hard to compute with a matrix as the derivative is so easy to compute. However, adding more complicated math such as cosines or exponentials (or cosines in exponentials) can be much more difficult, or even unsolvable without using rigorous mathematical tricks like the Taylor Series. Therefore, it is in the industry's best interest to find an effective way to make a regression model with only polynomial or otherwise simple curve fit functions. The first task in this program however explores the use of a relatively simple although non-polynomial fitting model.<br>
##### Starting Parameters and optimization:
The optimization functions used in this program with adjust the parameters in small steps until all further adjustments increase the error of the model. This poses an issue, because the model will often find itself in a local error minima, but not the absolute minimum or combination of parameters that would reduce the error as much as possible. Without a method to get itself out of these local minima, the only thing determining which minima the model will fall into is where the model starts, which is given by the programmer as a guess for the starting parameters. In this program, these parameters determine where the model will end up, so they bear a significant weight on the end result. This why the second task is to sweep 2 of the parameters at a time. In doing so, the local error landscape can be visualized, and you can see how the parameters could be changed to maintain the same error, or to increase/decrease it. <br>
##### Overtraining: 
Overtraining may happen in machine learning applications such as these, where the model is only effective against the training data but nothing else. This happens when the model sacrifices consistency for accuracy at each individual training point, disregarding the space between the points. In low-order polynomials, this is not too big of an issue, but when the order approaches the number of training data points, overtraining becomes more of an issue. This idea is explored in the third task with differing polynomial order fits. 

### Algorithm Implementation and Development
##### First Task:
To complete the first task, scipy.optimize.minimize (with Nelder-Mead optimization) was used in order to implement the function and find it's parameters to minimize the least-squares error to the observed data. A function taking the 4 parameters, x and y data was created which would output the error with these inputs to the inputted y data. This function was then inputted to the minimize algorithm with an initial guess for each parameter in order to minimize the error of the function and improve the parameters. The outputted improved parameters were plugged back into the function in the next step in order to graph the fit line against the observed data. Note that this approach was based on an approach made by Nathan Kutz at the University of Washington (2023). Notice the choice of 4, 0.9, 0.8, and 30 as the initial guess for the fit parameters. These numbers are used in order to generate an error minima that looks to fit the line pretty well. Different values produced fits like the one shown, or fits that would oscillate much more frequently or would be very similar to a linear fit. The guess numbers used here were produced with the assistance of the Desmos online graphing calculator. <br>
##### Second Task: 
The second task defines a sweep function to sweep 2 of the 4 variables at a time and create a 2D array of the error as the 2 variables changed across their respective axis. The parameters were swept across the optimized value from part 1 +-5 with resolution 0.01, for a range of 10 for each parameter and a total of 1000 values each. 6 color graphs are produced (one for each possible combination). Note that the optimized parameters are rounded to the nearest 0.001 to better display values on the graph. There is an error in one of the calculations that causes a very small 0.000...001 or 0.000...0015 to be added or subtracted from the actual value of the parameter that is used in the error function. It is not clear why this happens, and it only happens sometimes, but it only affects the graph tick labels, not the actual values (at least not in any significant way). The axis are labeled for which paramter, A, B, C, or D the axis is sweeping. <br>
##### Third Task: 
The third task uses numpy.polyfit given the divided training and test data and the order of the polynomial (1, 2, and 19) to fit. The errors for each fit are calculated using the least-squares error by using numpy.polyval given the optimized parameters to calculate the values for f(x) in the error equation. 

### Computational Results
##### First Task: 
The first task's fit line had the same general shape as the data, but did not follow the data as close as it potentially could have. Overall, the fit function however works better than a linear or polynomial fit likely would, so although the chosen model is more complex than a polynomial, it does perform better, and the trade off was worthwhile. It should be noted that this model only works because the data is easily visualizable, and a cosine pattern could be seen, and the initial guess parameters could be easily optimized with 2D graphing software. Without these tricks, the fit model would likely be optimized to be nearly a straight line, with considerably more error. When the initial values can be chosen effectively, this approach works well. But in higher dimensional space or with more complex data, this optimization approach would likely not work very well at all. 
##### Second Task: 
The second task generally shows showed that one parameter in the combination effected the error much more than the other, or was even the sole determiner of the error between the two parameters. The exception is when A and D were swept, where both parameters play an equal role in the error determination. Also, when C and D were swept, the minima "valley" is at an angle, showing that C has dominant control on the error in that case, but sweeping D has a non-zero effect as well. Most strangely, sweeping A with B and B with D created a mostly average graph but with a lot of inconsistent lines with more or less error than the surrounding points. This is a unique case where certain combinations of parameter values may work better or worse than slightly different combinations. By observing which parameters the minima valley focus on, the parameters can be ranked in terms of importance in determining the error. In every graph with C, C is the most dominant determiner of error, so C is the most important parameter. A and D are both more deterministic than B, but when compared together, have an equal effect on the error. B has very little control of the error in all plots. Therefore, C is the most important, A and D are tied for second most, and B is the least important. 
##### Third Task: 
The 19th degree polynomial fit in the third task overtrains the data in the first data case (first 20 are training data) and the second data case (first and last 10 are training data). This is seen with the very small training error, but astronomical test error. The second data partition does help reduce overtraining, but the model still performs very poorly. <br>
The parabola fit overtrains slightly on the first data partition, and considerably less so on the second. Although the test error is nearly twice the training error on the second partition, it still yields the best results out of the three fits for that data partition, only very slightly beating out the linear fit, although the two fits are almost identical. <br>
The linear fit performs the best with the test data in the first data partition, and minorly improves with the second data partition against the training and test data. This fit seems to be the most reliable for the given data, but not the best option for absolutely minimizing the error. <br>
Overall, the "dumber" fits do well to prevent overtraining, but also aren't as optimized as they could be. Especially when given a lot of information about the data being processed, a better fit function can be chosen and implemented in a way that prevents overtraining but also minimizes error as much as possible. 

### Summary and Conclusions
Overall, the program shows that a complex regression/fitting model can be useful if enough is known about the data to properly fit the model to it (and the computer can efficiently solve it). However, in higher dimensions or when little can be known about the data, a simpler fitting model will perform more reliably well. With better knowledge about the data the model is trying to fit, the starting parameters can be more wisely chosen to guide the model to a lower error minima. Without a way to avoid high-error local minima, this step is especially crucial in getting the best error from a regression model. It should be noted that more complex models increase the risk of overtraining the data as well and creating a worthless regression model, adding to the risk/reward of using a complex model without perfect knowledge about the observed training and test data. Additionally, it was shown in this program that changing some of the parameters in a fitting model will have less of an effect on the error than changing other parameters in the model will. Also that some particular parameter combinations exist that find a local minima without much indication around it that a local minima exists there. If a parameter is adjusted only slightly while in these minima, the error increases dramatically and stays stagnant with most other adjustments. 
