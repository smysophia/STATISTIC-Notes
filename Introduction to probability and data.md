# Week1
```r
library(dplyr)
library(ggplot2)
library(statsr)
data(arbuthnot)
```
the dimensions of this data frame by typing:  
`dim(arbuthnot)`   
the names of these columns (or variables) by typing:  
`names(arbuthnot)`
graphics(ggplot package)  
`ggplot(data = arbuthnot, aes(x = year, y = girls)) + geom_point()`  

add a coloum   
```r
arbuthnot <- arbuthnot %>%
mutate(total = boys + girls)
```

# week2

a quick peek at your data frame, and viewing its dimensions and data types is str `str(nycflights)`  
The `dplyr` package offers seven verbs (functions) for basic data manipulation:
* filter()
* arrange()
* select()
* distinct()
* mutate()
* summarise()
* sample_n()  

```{r hist-dep-delay}
ggplot(data = nycflights, aes(x = dep_delay)) +
  geom_histogram(binwidth = 60)
```
filter the data for flights headed to RDU (dest == "RDU") and then make a histogram of only departure delays of only those flights.  
```r
rdu_flights <- nycflights %>%
  filter(dest == "RDU")
ggplot(data = rdu_flights, aes(x = dep_delay)) +
  geom_histogram()
```
Summary statistics: Some useful function calls for summary statistics for a single numerical variable are as follows:
* mean
* median
* sd
* var
* IQR
* range
* min
* max  
   
 arrange these average delays in descending order `%>% arrange(desc(median_dd))`  
 side-by-side box plots:`ggplot(nycflights, aes(x = factor(month), y = dep_delay)) + geom_boxplot()` where factor(month) changes month(as integer) to catagorical variable.  
   
ifelse
```r
nycflights <- nycflights %>%
mutate(dep_type = ifelse(dep_delay < 5, "on time", "delayed"))
```

a segmented bar plot.`ggplot(data = nycflights, aes(x =  origin, fill = dep_type)) + geom_bar()`

# Week3
In a simulation, you set the ground rules of a random process and then the computer uses random numbers to generate an outcome that adheres to those rules.
```r
coin_outcomes <- c("heads", "tails")
sample(coin_outcomes, size = 1, replace = TRUE)
sim_fair_coin <- sample(coin_outcomes, size = 100, replace = TRUE)
table(sim_fair_coin)
```
to simulate an unfair coin that we know only lands heads 20% of the time. We can adjust for this by adding an argument called prob, which provides a vector of two probability weights.
```r
sim_unfair_coin <- sample(coin_outcomes, size = 100, replace = TRUE, 
                          prob = c(0.2, 0.8))
 ```
  
# Week4  
However we can subset the columns of interest and take a closer look at them. 
```r
selected_nzes2011 %>% 
  select(jpartyvote, jdiffvoting, _singlefav) %>% 
  str()
```

We can search the names for a fragment of the name by using the `grep("FRAGMENT", variable, value = TRUE)` command
```r
grep("singlefav", names(selected_nzes2011), value = TRUE)
```
An easy way of tabulating these data to see how many times each level of is to use the group_by() function along with the summarise() command:
```r
selected_nzes2011 %>% 
  group_by(jpartyvote) %>% 
  summarise(count = n())
```
get rid of "don't know" anwers:
```r
selected_nzes2011 %>% 
  filter(jpartyvote != "Don't know") %>%
  group_by(jpartyvote) %>% 
  summarise(count = n())
```
get rid of NA:
```r
selected_nzes2011 %>% 
  filter(!is.na(X_singlefav)) %>%
  group_by(X_singlefav) %>% 
  summarise(count = n())
```
  
numerical summaries (get rid of NA first):
```r
selected_nzes2011 %>% 
  filter(!(is.na(jage))) %>%
  summarise(agemean = mean(jage), agemedian = median(jage), agesd = sd(jage), 
            agemin = min(jage), agemax = max(jage))
```
filter two outcomes (to select only two of the possible levels in how much people like NZ First):
```r
selected_nzes2011 %>% 
  filter(jnzflike %in% c("0","10")) %>%
  group_by(jnzflike) %>% 
  summarise(count = n())
```
age and liking NZ First:
```r
selected_nzes2011 <- selected_nzes2011 %>% 
  mutate(retiredage = ifelse(jage >= 65, "retired age", "working age"))
selected_nzes2011 %>% 
  group_by(retiredage) %>% 
  summarise(count = n())
```
Change the type of data:  
factor(storage order ) -> character(text string) -> number(numeric)  
```r
selected_nzes2011 <- selected_nzes2011 %>% 
  mutate(numlikenzf = as.numeric(as.character(jnzflike)))
```


