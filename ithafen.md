# remove previously loaded items from the current environment and remove previous graphics.
rm(list=ls())
graphics.off()

# Here, I set the seed each time so that the results are comparable. 
# This is useful as it means that anyone that runs your code, *should*
# get the same results as you, although random number generators change 
# from time to time.
set.seed(1)

library(SIBER)

# load in the included demonstration dataset
data("demo.siber.data")
#
# create the siber object
siber.example <- createSiberObject(demo.siber.data)


# Or if working with your own data read in from a *.csv file, you would use
# This *.csv file is included with this package. To find its location
# type
# fname <- system.file("extdata", "demo.siber.data.csv", package = "SIBER")
# in your command window. You could load it directly by using the
# returned path, or perhaps better, you could navigate to this folder
# and copy this file to a folder of your own choice, and create a 
# script from this vingette to analyse it. This *.csv file provides
# a template for how your own files should be formatted.

# mydata <- read.csv(fname, header=T)
# siber.example <- createSiberObject(mydata)
Plotting the raw data
With the siber object created, we can now use various functions to create isotope scatterplots of the data, and also calculate some summary statistics on each group and/or community in the dataset.

Community 1 comprises 3 groups and drawn as black, red and green circles community 2 comprises 3 groups and drawn as black, red and green triangles

Various plotting options are collated into lists and then passed to the high-level SIBER plotting function plotSiberObject which is a wrapper function for easy plotting. We will access the more specific plotting functions directly a little later on to create more customised graphics.

ax.pad determines the padding applied around the extremes of the data.

iso.order is a vector of length 2 specifying which isotope should be plotted on the x and y axes.N.B. there is currently a problem with the addition of the group ellipses using if you deviate from the default of iso.order = c(1,2). This argument will be deprecated in a future release, and plotting order will be achieved at point of data-entry. I recommend you set up your original data with the chemical element you want plotted on the x-axis being the first column, and the y-axis in the second.

Convex hulls are drawn between the centres of each group within a community with hulls = T.

Ellipses are drawn for each group independently with ellipses = T. These ellipses can be made to be maximum likelihood standard ellipses by setting p = NULL, or can be made to be prediction ellipses that contain approximately p proportion of data. For example, p = 0.95 will draw an ellipse that encompasses approximately 95% of the data. The parameter n determines how many points are used to make each ellipse and hence how smooth the curves are.

Convex hulls are draw around each group independently with group.hulls = T.

# Create lists of plotting arguments to be passed onwards to each 
# of the three plotting functions.
community.hulls.args <- list(col = 1, lty = 1, lwd = 1)
group.ellipses.args  <- list(n = 100, p.interval = 0.95, lty = 1, lwd = 2)
group.hulls.args     <- list(lty = 2, col = "grey20")



par(mfrow=c(1,1))
plotSiberObject(siber.example,
                  ax.pad = 2, 
                  hulls = F, community.hulls.args = community.hulls.args, 
                  ellipses = T, group.ellipses.args = group.ellipses.args,
                  group.hulls = T, group.hulls.args = group.hulls.args,
                  bty = "L",
                  iso.order = c(1,2),
                  xlab = expression({delta}^13*C~'\u2030'),
                  ylab = expression({delta}^15*N~'\u2030')
                  )
