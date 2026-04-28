# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION
Date: 28:04:26
### AIM:
To Implement Linear and Polynomial Trend Estiamtion Using Python.

### ALGORITHM:

Import necessary libraries (NumPy, Matplotlib)

Load the dataset

Calculate the linear trend values using least square method

Calculate the polynomial trend values using least square method

End the program

### PROGRAM:

A - LINEAR TREND ESTIMATION

```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Load dataset
data = pd.read_csv('/content/TSLA.csv', parse_dates=['Date'], index_col='Date')

# Use Close column
data = data[['Close']]

# Resample yearly
resampled_data = data['Close'].resample('Y').mean().to_frame()

# Convert index to year
resampled_data.index = resampled_data.index.year
resampled_data.reset_index(inplace=True)
resampled_data.rename(columns={'Date': 'Year'}, inplace=True)

years = resampled_data['Year'].tolist()
values = resampled_data['Close'].tolist()

# Center X
X = [i - years[len(years)//2] for i in years]

# ---------- LINEAR TREND ----------
x2 = [i**2 for i in X]
xy = [i*j for i,j in zip(X, values)]
n = len(X)

b = (n*sum(xy) - sum(values)*sum(X)) / (n*sum(x2) - (sum(X)**2))
a = (sum(values) - b*sum(X)) / n

linear_trend = [a + b*X[i] for i in range(n)]


plt.figure(figsize=(8,5))
plt.plot(years, values, marker='o', color='blue', label='Actual')
plt.plot(years, linear_trend, linestyle='--', color='black', label='Linear Trend')

plt.title("Linear Trend Estimation")
plt.xlabel("Year")
plt.ylabel("Close Price")
plt.legend()
plt.grid(True)
plt.show()

```
B- POLYNOMIAL TREND ESTIMATION

```
x3 = [i**3 for i in X]
x4 = [i**4 for i in X]
x2y = [i*j for i,j in zip(x2, values)]

coeff = [
    [len(X), sum(X), sum(x2)],
    [sum(X), sum(x2), sum(x3)],
    [sum(x2), sum(x3), sum(x4)]
]

Y = [sum(values), sum(xy), sum(x2y)]

A = np.array(coeff)
B = np.array(Y)

a_poly, b_poly, c_poly = np.linalg.solve(A, B)

poly_trend = [a_poly + b_poly*X[i] + c_poly*(X[i]**2) for i in range(n)]

plt.figure(figsize=(8,5))
plt.plot(years, values, marker='o', color='blue', label='Actual')
plt.plot(years, poly_trend, linestyle='--', color='black', label='Polynomial Trend')

plt.title("Polynomial Trend Estimation (Degree 2)")
plt.xlabel("Year")
plt.ylabel("Close Price")
plt.legend()
plt.grid(True)
plt.show()

```
### OUTPUT
A - LINEAR TREND ESTIMATION

<img width="1289" height="643" alt="image" src="https://github.com/user-attachments/assets/b10d24a2-dfc1-41e0-bd90-fd3b0f32eecd" />

B- POLYNOMIAL TREND ESTIMATION

<img width="1170" height="612" alt="image" src="https://github.com/user-attachments/assets/07f901ff-8770-498b-8347-9a399055ba6e" />


### RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
