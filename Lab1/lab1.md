# Lab 1: Introduction to R and RStudio

## 1. Learning RStudio

### 1.1 Four Panels in RStudio
RStudio typically consists of four panels:
- **Source**: where you write scripts
- **Console**: where code is executed
- **Environment / History**: displays objects and command history
- **Files / Plots / Help / Viewer**: shows outputs and documentation

---

### 1.2 Create an R Script in RStudio
To create a new R script:
1. Click **File → New File → R Script**
2. Save the script with a `.R` extension
3. Write and run code using `Ctrl + Enter` (Windows) or `Cmd + Enter` (Mac)

---

### 1.3 Notes and Sections in R

```r
# (note you give to the code)
# this code is used for calculating the mean value
mean()

# this is the section ----
# this is the section ====
# this is the section ####
```

---

### 1.4 Help in R

```r
# help(function name)
help(mean)

# help(package = "package name")
help(package = "BAS")
```

---

## 2. Basic Operations

### 2.1 Mathematical Operations

```r
# try =, +, -, *, /, ^ in different variables
a = 1
b <- 2
c = a + b^2
```

---

### 2.2 Comparison and Logical Operations

```r
# Comparison operations: >, <, ==, !=, >=, <=
l1 = c(1, 2)
l2 = c(3, 4)

print(l1 == l2)
print(l2 >= l1)
```

```r
# Logical operations: &, |, !
m = 2
print((m > 1) & (m == 2))
```

---

### 2.3 Data Structures

```r
vec_1 <- c(1, 2, 3)
vec_2 <- c("bag", "book", "pencil")
matrix_1 <- matrix(1:6, nrow = 2)
data_frame <- data.frame(vec_1, vec_2)
list_obj <- c(vec_1, vec_2, matrix_1, data_frame)
```

---

## 3. Functions

### 3.1 Basic Function

```r
fun1 <- function(a, b) {
  c = a * b + 1
  return(c)
}

fun1(1, 2)
```

---

### 3.2 Summary Statistics

```r
x = c(1, 2, 3)

min(x)
max(x)
sum(x)
sum(x > 3)
which(x > 2)
prod(x)
sqrt(x)
exp(x)
log(9, 3)

mean(x)
var(x)
sd(x)
summary(x)
```
