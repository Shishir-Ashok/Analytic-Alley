---
title: When Maximum Meets Square Root
date: 2024-12-05
images: 
  - /portfolio/When Maximum Meets Square Root/thumbnail.png
authors:
  - name: Shishir Ashok
    link: https://github.com/shishir-ashok
    image: https://github.com/shishir-ashok.png
tags:
  - Statistics
  - R
---

I recently came across a [YouTube Short](https://www.youtube.com/shorts/Pny70rNPJLk) by Grant Sanderson, and I have to say, it blew my mind! The idea that the distributions describing the result of the maximum of two random numbers and the square root of one of those numbers are the same is simply too good to be true. His explanation in the video is pure brilliance!

Inspired by this, I decided to write a quick R script to simulate both of these processes and compare their distributions.


``` r
n_samples <- 100000

# Generate 100,000 pairs of random numbers between 0 and 1
rand_pairs <- replicate(n_samples, runif(2, min = 0, max = 1))

# Calculate the maximum of each pair
max_rand <- apply(rand_pairs, 2, max)

# Calculate the square root of a randomly selected number from each pair
sqrt_rand <- apply(rand_pairs, 2, function(x) sqrt(sample(x, 1)))
```

<img src="/portfolio/When Maximum Meets Square Root/When Maximum Meets Square Root_files/figure-html/unnamed-chunk-3-1.png" width="672" />


### Why This is So Cool

What I love about this is that it's not just a random coincidence. There's a beautiful symmetry in how the square root transformation and the maximum function work in tandem to produce the same kind of distribution. It’s a reminder of how interconnected different concepts in probability and statistics can be, and it really made me appreciate the elegance of randomness!

<img src="/portfolio/When Maximum Meets Square Root/evolution.gif" width="672" />
