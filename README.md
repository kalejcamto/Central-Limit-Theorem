# Central-Limit-Theorem
Inference and Modeling course: Code: Monte Carlo simulation using a set value of p
Key points
We can run Monte Carlo simulations to compare with theoretical results assuming a value of .
In practice,  is unknown. We can corroborate theoretical results by running Monte Carlo simulations with one or several values of .
One practical choice for  when modeling is , the observed value of  in a sample.
Code: Monte Carlo simulation using a set value of p
p <- 0.45    # unknown p to estimate
N <- 1000

# simulate one poll of size N and determine x_hat
x <- sample(c(0,1), size = N, replace = TRUE, prob = c(1-p, p))
x_hat <- mean(x)

# simulate B polls of size N and determine average x_hat
B <- 10000    # number of replicates
N <- 1000    # sample size per replicate
x_hat <- replicate(B, {
    x <- sample(c(0,1), size = N, replace = TRUE, prob = c(1-p, p))
    mean(x)
})
Code: Histogram and QQ-plot of Monte Carlo results
library(tidyverse)
library(gridExtra)
p1 <- data.frame(x_hat = x_hat) %>%
    ggplot(aes(x_hat)) +
    geom_histogram(binwidth = 0.005, color = "black")
p2 <- data.frame(x_hat = x_hat) %>%
    ggplot(aes(sample = x_hat)) +
    stat_qq(dparams = list(mean = mean(x_hat), sd = sd(x_hat))) +
    geom_abline() +
    ylab("X_hat") +
    xlab("Theoretical normal")
grid.arrange(p1, p2, nrow=1)


# Key points
# The spread between two outcomes with probabilities p and 1 is 2p-1 
 p−(1−p) = 2p−1. 
The expected value of the spread is 2X-1
The standard error of the spread is 2SE(X)
The margin of error of the spread is 2 times the margin of error of X.

The competition is to predict the spread, not the proportion
p.
However, because we are assuming there are only two parties,
we know that the spread is just p minus (1 minus p),
which is equal to 2p minus 1. (2p−1) 
So everything we have done can easily be adapted to estimate to p minus 1.
Once we have our estimate, X-bar, and our estimate of our standard error
of X-bar, we estimate the spread by 2 times X-bar minus 1, just plugging
in the X-bar where you should have a p.
And, since we're multiplying a random variable by 2,
we know that the standard error goes up by 2.
So the standard error of this new random variable
is 2 times the standard error of X-bar.
Note that subtracting the 1 does not add any variability,
so it does not affect the standard error.
So, for our first example, with just the 25 beads, our estimate of p
was 0.48 with a margin of error of 0.2.
This means that our estimate of the spread
is 4 percentage points, 0.04, with a margin of error of 40%, 0.4.
Again, not a very useful sample size.
But the point is that once we have an estimate and standard error for p,
we have it for the spread 2p - 1.



# Key points
# An extremely large poll would theoretically be able to predict election results almost perfectly.
These sample sizes are not practical. In addition to cost concerns, polling doesn't reach everyone in the population (eventual voters) with equal probability, and it also may include data from outside our population (people who will not end up voting).
These systematic errors in polling are called bias. We will learn more about bias in the future.

# Code: Plotting margin of error in an extremely large poll over a range of values of p
library(tidyverse)
N <- 100000
p <- seq(0.35, 0.65, length = 100)
SE <- sapply(p, function(x) 2*sqrt(x*(1-x)/N))
data.frame(p = p, SE = SE) %>%
    ggplot(aes(p, SE)) +
    geom_line()


    
