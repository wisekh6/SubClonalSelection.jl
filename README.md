# SubClonalSelection

A Julia package for inferring the strength of selection from cancer sequencing data. Package simultaneously estimates the number of subclones in the population and their relative fitnesses. This is done by generating synthetic data by simulating different population dynamics (see [CancerSeqSim.jl](https://github.com/marcjwilliams1/ApproxBayes.jl)) and fitting this to the model using Approximate Bayesian Computation (see [ApproxBayes.jl](https://github.com/marcjwilliams1/CancerSeqSim.jl)).

## Getting Started
Package has been tested extensively with [Julia](https://julialang.org/) v0.5.1 but should work with later versions. If there any problems please report an issue.

## Input data
Running an analysis requires variant allele frequencies (VAFs) as measured in deep sequencing of cancer samples. Generating the synthetic data assumes that the cancer is diploid, therefore any mutations falling in non-diploid regions should be removed. This does unfortunately mean that in highly aneuploid tumours, there will not be enough mutations to perform an analysis. We would recommend a minimum of 50 mutations.

## Running an analysis
The main function to perform an analysis is ```fitABCmodels```. This takes as its first argument either a vector of Floats or a string pointing to a text file with a vector of floats, which will be read in automatically. The second argument is the name of the sample you wish to analyse which will be used to write the data and plots to a file. There are then a number of keyword arguments set to reasonable defaults. More details of these arguments and their defaults can be found by typing ```?fitABCmodels``` in a Julia session.

There is some example data generated from the simulation found in the examples directory. For example the following command will run the inference on the ```oneclone.txt``` data set with 200 posterior samples given sequencing depth of sample is 150X, and then save the output to the example directory:
```
out = fitABCmodels("example/oneclone.txt", "oneclone", read_depth = 150, save = true, resultsdirectory = "example", nparticles = 200)
```
Also included are a number of functions to summarize the output and plot the posterior. ```show(out)``` will print a summary of the posterior model and parameter probabilities. We can also plot the posterior distributions.

Plot the posterior model probabilities.
```
plotmodelposterior(out)
```
![plot](/example/plots/modelposterior.png)

Plot the histogram for model 2.
```
plothistogram(out, 1)
```
![plot](/example/plots/histogram-1clone.png)

Plot the posterior parameter distribution for model 2.
```
plothistogram(out, 1)
```
![plot](/example/plots/posterior-1clone.png)

Finally we can also save all plots and text files with posterior distributions to a directory, unless specified the default will be to create a file a directory called ```output``` in the current working directory.

```
saveresults(out; resultsdirectory = "example")
saveallplots(out, resultsdirectory = "example")
```
