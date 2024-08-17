# Supermarket Sales Data Analysis

This project involves analyzing and visualizing supermarket sales data using Python in a Jupyter Notebook. The data is processed to show the evolution of product quantities across different product lines over a specific period.

## Project Structure

- `supermarket_sales - Sheet1.csv`: The dataset containing supermarket sales records.
- `Supermarket_Sales_Analysis.ipynb`: The Jupyter Notebook that loads the data, processes it, and visualizes the evolution of product quantities over time.

## Requirements

Make sure you have the following Python packages installed:

- `pandas`
- `matplotlib`
- `numpy`

You can install these packages using pip:

```bash
pip install pandas matplotlib numpy
```

## How It Works

### 1. Load the Data
The notebook starts by loading the data from the CSV file using pandas.

```python
data = pd.read_csv("./supermarket_sales - Sheet1.csv")
```

### 2. Convert Date Column to Datetime
The `Date` column is converted to a datetime format for easier sorting and filtering.

```python
data['Date'] = pd.to_datetime(data['Date'])
```

### 3. Sort the Data by Date
The data is sorted in ascending order based on the `Date` column.

```python
sorted_data = data.sort_values(by="Date", ascending=True)
```

### 4. Identify Unique Product Lines
The notebook identifies all unique product lines in the dataset.

```python
productLine = sorted_data["Product line"].unique()
```

### 5. Create a New DataFrame for Aggregated Product Quantities
An empty DataFrame is created with columns for each product line and the date.

```python
product_line_df = pd.DataFrame(columns=np.append(productLine, 'Date'))
```

### 6. Aggregate Product Quantities
The notebook iterates through the original data, aggregating product quantities by date and product line.

```python
for index, row in sorted_data.iterrows():
    if row['Date'] not in product_line_df['Date'].values:
        new_row = {'Date': row['Date']}
        for PL in productLine:
            new_row[PL] = 0
        product_line_df = pd.concat([product_line_df, pd.DataFrame([new_row])], ignore_index=True)
    product_line_df.loc[product_line_df['Date'] == row['Date'], row['Product line']] += 1 * row['Quantity']
```

### 7. Filter the Data by Date Range
The data is filtered to include only the specified date range (from January 1, 2019, to January 4, 2019).

```python
filtered_data = product_line_df[(product_line_df['Date'] >= '2019-01-01') & (product_line_df['Date'] <= '2019-01-04')]
```

### 8. Plot the Data
Finally, the notebook generates a plot showing the evolution of product quantities over the specified date range for each product line.

```python
plt.figure(figsize=(12, 6))
for column in filtered_data.columns:
    if column not in ['Unnamed: 0', 'Date']:
        plt.plot(filtered_data['Date'], filtered_data[column], marker='o', label=column)

plt.title('Evolution of Products Quantity (01/01/2019 - 01/04/2019)')
plt.xlabel('Date')
plt.ylabel('Quantity')
plt.xticks(rotation=45)
plt.legend(title='Product Line')
plt.grid(True)
plt.show()
```

## Running the Notebook

To run the notebook:

1. Open `Supermarket_Sales_Analysis.ipynb` in Jupyter Notebook or JupyterLab.
2. Execute the cells sequentially to load the data, process it, and generate the plot.

