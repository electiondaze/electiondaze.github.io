
# r-cheatsheet
```
library(scaling)
library(tidyverse)

### import data
bias <- read.csv('media_bias - Sheet1.csv')

### view headers
head(bias)
X         X.1    X.2
1                 News Source Reliability   Bias
2                    ABC news       49.47  -1.85
3                  Al Jazeera       49.47  -3.71
4                    Alternet        21.4 -19.16
5            American Thinker        23.1  29.82

### drop header row
bias2 <- bias[-1,]

### rename headers
bias %>%
     rename(
         source = X,
         reliability = X.1,
         bias = X.2)

### add partisan catagory col
bias <- bias %>%
     mutate(partisan = case_when(X.2 < 0 ~ "blue",
                                 X.2 >  0 ~ "red"))

### add ranged partisan col "partisan2"
df$partisan2 <-
            ifelse(df$bias >= -2 & df$bias <= 2, "Non-partisan",
                   ifelse(df$bias < -2, "Blue",
                          ifelse(df$bias > 2, "Red", NA)
                   ))
				source 		reliability		bias		partisan		partisan2
2			ABC news       		49.47		-1.85 			blue		Non-partisan
3			Al Jazeera     		49.47		-3.71 			blue		Blue
4			Alternet       		21.4		-19.16  		blue		Blue
5			American Thinker   23.1		29.82			red			Red

# drop partisan // 4th column
df = select(df, -4)

           source reliability   bias    partisan2
2         ABC news       49.47  -1.85 Non-partisan
3       Al Jazeera       49.47  -3.71         Blue
4         Alternet        21.4 -19.16         Blue
5 American Thinker        23.1  29.82          Red

# rescale values to 0-100 score, can be done as decimal % without specifying range
df$newcol <- rescale(df$reliability, to = c(0,100))
```
