---
layout: post
title:  R and Python Grammar
description: "R vs. Python by Example"
modified: 2017-01-13
tags: [r, python]
---

Here, R and Python code for the same purpose are paired for your consideration.
They are strikingly similar.

## Expressions and Statements

### Comment

R and Python has the same grammar for writing comment.

```r
# This is comment. 
```

### Print

R and Python has the same grammar for console output.

```r
print('the same code for r and python again')
```

### Arithmetic

```r
# R
2 / 7   # 0.285714...
2 %/% 7 # 0
2 ^ 7
2 %% 7
rep('a', 3) # 'aaa'
```

```python
# Python
2 / 7  # 0.285714...
2 // 7 # 0
2 ** 7
2 % 7
'a' * 3 # 'aaa'
```

### Console Help on a Function

The same grammar(command grammar) again.

```
?max
```

### Import library

```r
# R
library(dplyr)
```

```python
# Python
import pandas as pd
from math import radians as rad
```

### Iterations

```r
# R
for(x in 2:7){ ... }

for(x in dic){ ... }

for(i in 1:nrow(df)){ ... }
```

```python
# Python
for i, x in enumerate(2:7):       # index and value simultaneously!
    ...

for k, v in dic.items():          # loop over dictionary
    ...

for label, row in df.iterrows():  # loop over DataFrame
    ...
```

### Method call

- R: like function
- Python: after `.`

Like many other OOP languages, Python uses `.` to call member function of a class. However, In R, `.` has no special meaning. you can use `.` in variable or function names. Member function is called like other top-level functions.


## Data Structures

### Basic Types

- R: logical(boolean), numeric, integer, character, vector, matrix, data.frame
- Python: bool, float, int, str

c.f. Python utility functions: type()

c.f. R support names everywhere

### List

List creation, slicing, access, append and delete code:

```r
# R
a <- list('a', 1, 'b', 2)
a[1]   # list('a')
a[-1]  # list(1, 'b', 2)
a[1:2] # list('a', 1)
a[[1]] # 'a'

b <- list(a, a) # list(list('a', 1, 'b', 2), list('a', 1, 'b', 2))

b[[1]][[2]] # 1
b[[1]][2]   # list(1)

a <- c(a, list('c', 3))  # append
a[5:6] <- NULL           # delete
```

```python
# Python
a = ['a', 1, 'b', 2]
a[0]  # 'a'
a[-1] # 2
a[0:2] # ['a', 1]

b = [a, a] # [['a', 1, 'b', 2], ['a', 1, 'b', 2]]
b[0][1] # '1'

a <- a + ['c', 3]       # append
del(a[-2:-1)            # delete
```

Note that R always performs slicing, while Python mixes indexing and slicing (like R vector).


### Vector, Matrix

|        | Vector types               | Matrix types     |
| :---   | :---                       | :---             |
| R      | vector                     | matrix           |
| Python | numpy.array, pandas.Series | numpy.array (2D) |

```r
# R
a <- c(1,2,3)  # vector creation by combine function
a[2]  # 2
b <- matrix(1:6, ncol = 2, byrow=T)  # matrix creation
b[2, 2]  # 4
```

```python
# Python
import numpy as np
a <- np.array([1, 2, 3])
a[1]    # 2

b <- np.array([1, 2], [3, 4], [5, 6])
b[1, 1] # 4
```

### Dictionary (Key-Value Store / Map / Symboltable)

- R: named data structures(list, vector, etc) do
- Python: Dictionary type exists

```r
# R
dic <- list(a = 1, b = 2)
dic['a']       # list(a = 1)
dic[['a']]     # 1
dic$a          # 1
dic$c <- 3     # list(a = 1, b = 2, c = 3)
dic$b <- NULL  # list(a = 1, c = 3)
```

```python
# Python
dic = { 'a': 1, 'b': 2 }
dic['a']       # 1
dic['c'] = 3   # { 'a': 1, 'b': 2, 'c': 3 }
del(dic['b'])  # { 'a': 1, 'c': 3 }
```

### Dataset (Table)

|        | Dataset types    |
| :---   | :---             |
| R      | data.frame       |
| Python | pandas.DataFrame |

```r
# R
df <- data.frame(id = c('A', 'B'), age = c(12, 13))
rownames(df) <- 1:2

df <- read.csv('data.csv')
df$age                         # c(12, 13) i.e. vector
data.frame(age = df$age)       # single column data.frame
                               # c.f. dplyr select
df[, 1:2]                      # data.frame with 2 columns
df[1, ]                        # 1st row
df[1:2, 1:2]                   # 1~2 rows, 1~2 columns
```

```python
# Python
import pandas as pd
df = pd.DataFrame({ 'id': ['A', 'B'], 'age': [12, 13] })
df.index = [1, 2]

df = pd.read_csv('data.csv')
df['id']                       # pandas Series [12, 13] i.e. vector
df[['id']]                     # pandas DataFrame with single column
df[['id', 'age']]              # 2 columns
df[0]                          # 1st row
df.iloc[0, 0]                  # 'A'
df.iloc[[0, 1], [0, 1]]        # 1~2 rows, 1~2 columns
                               # c.f. df.iloc[:, [0, 1]]
                               # c.f. df.loc for character names
```
## Function

### Function Definition

```r
# R

#' Double input
#'
#' @param x numeric. number to be doubled
#'
#' @return numeric 
#' @export
#'
#' @examples el.inv(iris[1:4,1:4])
#' 
ftn <- function(x){
  x * 2
}
```

```python
# Python

def ftn(x):
   # Double input"   
   return x * 2;
```

### Anonymous Function

```r
# R
function(x, y){x + y}
```

```python
# Python
lambda x, y: x + y;
```


### Function Mapping

```r
# R
sapply(df$id, tolower)       # 'a', 'b'

library(purrr)
df$id %>% map(tolower)       # 'a', 'b'
```

```r
# Python (by pandas)
df['id'].apply(str.lower)    # 'a', 'b'
```

c.f. anonymous function using formula

```r
# R
library(purrr)
xs %>% map(~ . * 2)       # xs * 2
```

## Statistics

### Vector Statistic

- R: mean, sd, cor, ...
- Python: numpy.mean, numpy.std, numpy.corrcoef, ...

### Plot Basic

```r
# R
plot(1:10, type='l')  # line plot
plot(1:10)            # scatter plot (default)
# c.f. points(1:10)

hist(runif(10))       # histogram
```

```python
# Python
import matplotlib.pyplot as plt
plt.plot(range(1, 11))  # line plot
plt.show()              # show panel
plt.clf()               # clear panel

plt.scatter(range(1, 11), range(1, 11))
plt.show()
plt.clf()

import numpy as np
plt.hist(np.random.rand(10))
plt.show()
```
