# Ex.No: 1B                     CONVERSION OF NON STATIONARY TO STATIONARY DATA
# Date: 04-08-26

### AIM:
To perform regular differncing,seasonal adjustment and log transformatio on international airline passenger data
### ALGORITHM:
1. Import the required packages like pandas and numpy
2. Read the data using the pandas
3. Perform the data preprocessing if needed and apply regular differncing,seasonal adjustment,log transformation.
4. Plot the data according to need, before and after regular differncing,seasonal adjustment,log transformation.
5. Display the overall results.
### PROGRAM:
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from statsmodels.tsa.seasonal import seasonal_decompose

# Load dataset
df = pd.read_csv("/content/ecommerce_sales_analytics_5000.csv")

# Convert order_date to datetime
df["order_date"] = pd.to_datetime(df["order_date"], errors="coerce")

# Remove invalid dates
df = df.dropna(subset=["order_date"])

# Sort by date
df = df.sort_values("order_date")

# Group revenue by date
data = df.groupby("order_date")["revenue"].sum().to_frame()

# Regular Differencing
data["Revenue_Diff"] = data["revenue"].diff()

# Seasonal Decomposition
result = seasonal_decompose(
    data["revenue"],
    model="additive",
    period=7
)

data["Seasonal_Adjustment"] = result.resid

# Log Transformation
data["Log_Revenue"] = np.log1p(data["revenue"])

# Log + Differencing
data["Log_Revenue_Diff"] = data["Log_Revenue"].diff()

# Seasonal decomposition on transformed data
result = seasonal_decompose(
    data["Log_Revenue_Diff"].dropna(),
    model="additive",
    period=7
)

data.loc[
    data["Log_Revenue_Diff"].dropna().index,
    "Log_Seasonal_Diff"
] = result.resid

# Plotting
plt.figure(figsize=(16,16))

plt.subplot(6,1,1)
plt.plot(data.index, data["revenue"])
plt.title("Original Revenue")
plt.xlabel("Date")
plt.ylabel("Revenue")

plt.subplot(6,1,2)
plt.plot(data.index, data["Revenue_Diff"])
plt.title("Regular Differencing")
plt.xlabel("Date")
plt.ylabel("Revenue Difference")

plt.subplot(6,1,3)
plt.plot(data.index, data["Seasonal_Adjustment"])
plt.title("Seasonal Adjustment")
plt.xlabel("Date")
plt.ylabel("Seasonally Adjusted Revenue")

plt.subplot(6,1,4)
plt.plot(data.index, data["Log_Revenue"])
plt.title("Log Transformation")
plt.xlabel("Date")
plt.ylabel("Log(Revenue)")

plt.subplot(6,1,5)
plt.plot(data.index, data["Log_Revenue_Diff"])
plt.title("Log Transformation + Regular Differencing")
plt.xlabel("Date")
plt.ylabel("Diff(Log Revenue)")

plt.subplot(6,1,6)
plt.plot(data.index, data["Log_Seasonal_Diff"])
plt.title("Log Transformation + Seasonal Differencing")
plt.xlabel("Date")
plt.ylabel("Seasonal Diff(Log Revenue)")

plt.tight_layout()
plt.show()

# Line plot of all generated columns
data.plot(figsize=(12,6))
plt.show()
```

### OUTPUT:


REGULAR DIFFERENCING:
<img width="1236" height="622" alt="image" src="https://github.com/user-attachments/assets/b4e4140c-1eda-4459-9b27-0228481e2adc" />


LOG TRANSFORMATION + SEASONAL ADJUSTMENT:
<img width="1192" height="517" alt="image" src="https://github.com/user-attachments/assets/911407ff-b167-4bf7-914c-e84993322f33" />



### RESULT:
Thus we have created the python code for the conversion of non stationary to stationary data on international airline passenger
data.
