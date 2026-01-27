
## Step 1: To complete the following tasks, several R packages are required.
Packages should usually be installed once in the Console panel.
```
# Install the remotes package (used to install packages from GitHub)
install.packages("remotes")

# Install yfR directly from GitHub
remotes::install_github("msperlin/yfR")

# Install dplyr for data manipulation
install.packages("dplyr")

# Install e1071 for skewness and kurtosis
install.packages("e1071")

# Install writexl to export data to Excel
install.packages("writexl")

```

### Explanation

- `install.packages()` downloads packages from **CRAN**
- `remotes::install_github()` installs packages hosted on **GitHub**
- These packages provide tools for **data manipulation**, **statistics**, and **file export**

---

### Step 2: Load packages in your script

```r
library(dplyr)
library(e1071)
```
### Explanation

- `library()` makes the functions in a package available for use
- This step must be repeated **each time you start a new R session**

## Task 1: Working with Monthly Stock Data

### Set working directory (you can create a new folder only for lab)

```r
setwd("/Users/.../")
getwd()
```

### Explanation
- Sets the working directory where R will look for files.
- Replace `/Users/.../` with the actual folder path on your computer
  
### Import data
data <- read.csv("MonthlyData.csv")

---

## Task 1: Basic Data Inspection

### (1) Number of rows in the data

```r
dim(data)[1]
```

Explanation:
- `dim(data)` returns the dimensions of the data frame.
- The first element corresponds to the number of rows.
- This tells us how many observations are in the dataset.

---

### (2) Time range of the data

```r
range(data$date)
```

Explanation:
- `data$date` extracts the `date` column.
- `range()` finds the minimum and maximum values.
- This gives the start and end dates of the data.

---

### (3) Data types of columns

```r
class(data$date)
class(data$volume)
```

Explanation:
- `class()` checks the data type of a variable.
- This helps confirm whether a column is numeric, character, or date.

---

### (4) Create a data frame containing only AAPL

```r
AppleData = data[data$ticker == "AAPL", ]
```

Explanation:
- `data$ticker == "AAPL"` creates a logical condition.
- Only rows where the ticker equals AAPL are selected.
- The comma keeps all columns.
- The result is stored in `AppleData`.

---

### (5) Calculate returns

```r
data <- data %>%
  dplyr::mutate(
    Price_lag = lag(price.adjusted),
    ret_discrete = (price.adjusted - Price_lag) / Price_lag,
    ret_continuos = log(price.adjusted / Price_lag)
  )
```

Explanation:
- `%>%` passes the data frame into `mutate()`.
- `lag(price.adjusted)` shifts prices by one period.
- `ret_discrete` computes simple (discrete) returns.
- `ret_continuos` computes logarithmic (continuous) returns.
- New columns are added to the data frame.

---

### Save the data

```r
write.csv(data, file = "/Users/.../new_data.csv")
```

Explanation:
- `write.csv()` saves the data frame as a CSV file.

---

## Task 2: Data Cleaning, Returns, and Visualization
```
data_2 = read.csv('lab1.csv')
```
### (1) Rename column and change date format

```r
names(data_2)[names(data_2) == "FTSE"] <- "FTSE_ALL"
```

Explanation:
- `names(data_2)` returns all column names.
- The column named `FTSE` is renamed to `FTSE_ALL`.

```r
data_2$Date <- as.Date(data_2$Date, format = "%m/%d/%Y")
```

Explanation:
- Converts the `Date` column from character to Date format.

```r
save(data_2, file = "/Users/.../lab1.RData")
```

Explanation:
- Saves the data frame as an RData file.

---

### (2) Generate logarithmic returns

```r
data_2 <- data_2 %>%
  dplyr::mutate(
    lag_price = lag(data_2$BARCLAYSPI, 1),
    RET = log(data_2$BARCLAYSPI / lag_price)
  ) %>%
  select(-lag_price)
```

Explanation:
- `lag()` creates a one-period lag of prices.
- Log returns are calculated.
- The temporary lag variable is removed.

---

### (3) Plot price and returns on the same graph

```r
par(mar = c(5, 4, 4, 6))
```

Explanation:
- Adjusts plot margins to allow a second y-axis.

```r
plot(data_2$Date, data_2$BARCLAYSPI, type = "l", col = "blue")
```

Explanation:
- Plots the price series as a blue line.

```r
par(new = TRUE)
```

Explanation:
- Allows a new plot to be drawn on top of the existing one.

```r
plot(data_2$Date, data_2$RET, type = "l", col = "red", axes = FALSE, ann = FALSE)
```

Explanation:
- Adds the return series without axes or labels.

```r
axis(side = 4, at = pretty(range(data_2$RET, na.rm = TRUE)))
```

Explanation:
- Adds a right-side y-axis for returns.

```r
mtext(side = 4, "BARCLAYSPI Returns", line = 3)
```

Explanation:
- Adds a label to the right y-axis.

---

### Separate plots

```r
plot(data_2$Date, data_2$BARCLAYSPI, type = "l")
plot(data_2$Date, data_2$RET, type = "l")
```

Explanation:
- Plots prices and returns in separate figures.

---

### Kernel density estimation

```r
plot(density(data_2$BARCLAYSPI))
```

Explanation:
- Estimates and plots the price distribution.

```r
RET_clean <- na.omit(data_2$RET)
```

Explanation:
- Removes missing values from returns.

```r
plot(density(RET_clean))
```

Explanation:
- Plots the return density.

---

### Descriptive statistics

```r
mean(data_2$RET, na.rm = TRUE)
median(data_2$RET, na.rm = TRUE)
skewness(data_2$RET, na.rm = TRUE)
kurtosis(data_2$RET, na.rm = TRUE)
```

Explanation:
- Computes mean and median.
- Computes skewness and excess kurtosis.

---

### Histogram

```r
hist(data_2$RET)
```

Explanation:
- Displays the distribution of returns.

---

## Task 3: Download Financial Data

```r
library(yfR)
```

Explanation:
- Loads the yfR package.

```r
StockSet <- c("AAPL", "MSFT", "INTC", "AMZN", "AMD")
```

Explanation:
- Defines the stock tickers.

```r
first_date <- "2000-01-01"
last_date <- "2025-01-31"
```

Explanation:
- Sets the start and end dates.

```r
data <- yf_get(
  tickers = StockSet,
  first_date = first_date,
  last_date = last_date,
  freq_data = "monthly"
)
```

Explanation:
- Downloads monthly stock data from Yahoo Finance.

```r
write_xlsx(data, ".../Desktop/part1_new.xlsx")
```

Explanation:
- Saves the downloaded data to an Excel file.












