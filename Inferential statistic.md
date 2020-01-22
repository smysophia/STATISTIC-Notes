# Week1
summarise  
```r
ames %>%
  summarise(mu = mean(area), pop_med = median(area), 
            sigma = sd(area), pop_iqr = IQR(area),
            pop_min = min(area), pop_max = max(area),
            pop_q1 = quantile(area, 0.25),  # first quartile, 25th percentile
            pop_q3 = quantile(area, 0.75))  # third quartile, 75th percentile
```
make a sample
```r
samp1 <- ames %>%
  sample_n(size = 50)
```
Generate 15000 samples, sample size = 50
```r
sample_mean50 <- ames %>% rep_sample_n(size=50, reps=15000,replace=TRUE)
```
