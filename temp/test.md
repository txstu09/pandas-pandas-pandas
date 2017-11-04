# Quertiles and Outliers

## Instructions

* Take a look at the list in the [samples file](Unsolved/samples.py). Identify the median, upper quartile, and lower quartiles by hand.

* For large data sets, identifying these data points by hand is obviously infeasible. 

  * The median of a sorted list is at the index `len(arr) / 2` — with some small adjustments if the list of even length, of course).

  * Similarly, we can find the lower and upper quartiles at the indices `len(arr) / 4` and `3 * len(arr) / 4`, respectively.

  * Use these formulas to find the median, upper quartile, and lower quartiles of the provided list, and compare them to what you found by hand.

* Recall that one way to remove outliers is to throw away all values less than the lower quartile and greater than the upper quartile.

  * Create a list containing the small excluded numbers; one containing the high excluded numbers; and one containing the included numbers.

* Plot these lists on the same figure.

  * Plot the included numbers as a line plot, and plot the excluded numbers as scatter plots.

  * Think carefully about how to define your x-axis.

* The difference between the upper and lower quartile is called the **interquartile range**, or IQR.

  * Like the standard deviation, the IQR describes how "spread out" the data set is.

  * Calculate the IQR for this list.

* Finally, print the median; lower quartile; upper quartile; and IQR for your data.

* As a **bonus**, write a function that calculates the lower and upper quartiles and IQR of a list of numbers.
