# MRDT Analysis and Visualization

This repository contains code to analyze and visualize Mortality Rate Doubling Time (MRDT) data for Angus Maddison's Group A and Group B countries. He differantiate these countries with the growth rates of GDP per capita. Group A has exponential high rate of growth. On the other hand Group B has linear small rate of change. The analysis includes calculating average MRDT values over the years and plotting them for both groups.

## Table of Contents

- [Overview](#overview)
- [Requirements](#requirements)
- [Usage](#usage)
- [Functions](#functions)
- [Data](#data)
- [Plots](#plots)

## Overview

The code calculates the mean MRDT for females, males, and the total population across different years for two groups of countries: specified (Group A) and other (Group B). The MRDT values are then visualized to compare the trends between the two groups over time.

## Requirements

The code requires the following Python packages:

- `numpy`
- `pandas`
- `seaborn`
- `matplotlib`

You can install these packages using pip:

```bash
pip install numpy pandas seaborn matplotlib
```

## Usage

1. Ensure you have the required data in two DataFrames: `specified_group_df` and `other_group_df`. These DataFrames should contain MRDT values for females, males, and the total population for each year.

2. Run the provided code to calculate the mean MRDT values and generate plots.

## Functions

### `calculate_statistics(df)`

Calculates the mean and median MRDT values for each year.

**Parameters:**

- `df` (DataFrame): Input DataFrame containing MRDT values.

**Returns:**

- `stats` (DataFrame): DataFrame with calculated statistics.

### `plot_mean_mrdt(data, value_column_mean, title, ylabel)`

Generates line plots for the mean MRDT values over the years.

**Parameters:**

- `data` (DataFrame): Combined DataFrame with MRDT values.
- `value_column_mean` (str): Column name for the mean MRDT values.
- `title` (str): Title of the plot.
- `ylabel` (str): Y-axis label for the plot.

## Data

Ensure your data is structured with the following columns:

- `Country`
- `Year`
- `MRDT_Female`
- `MRDT_Male`
- `MRDT_Total`
- `Group`

The `Group` column should contain values 'Group A' for specified countries and 'Group B' for other countries.

## Plots

The code generates the following plots:

1. Average MRDT (Female) Over Years
2. Average MRDT (Male) Over Years
3. Average MRDT (Total) Over Years

Each plot compares the trends between Group A and Group B.

## Example

```python
import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# Assuming specified_group_df and other_group_df are already defined

def calculate_statistics(df):
    stats = df.groupby('Year').agg({
        'MRDT_Female': ['mean', 'median'],
        'MRDT_Male': ['mean', 'median'],
        'MRDT_Total': ['mean', 'median'],
    })
    stats.columns = ['_'.join(col).strip() for col in stats.columns.values]
    return stats.reset_index()

mean_mrdt_specified_by_year = calculate_statistics(specified_group_df)
mean_mrdt_other_by_year = calculate_statistics(other_group_df)

# Merge the data for plotting
mean_mrdt_specified_by_year['Group'] = 'Group A'
mean_mrdt_other_by_year['Group'] = 'Group B'
combined_mean_mrdt = pd.concat([mean_mrdt_specified_by_year, mean_mrdt_other_by_year])

# Helper function to plot mean MRDT
def plot_mean_mrdt(data, value_column_mean, title, ylabel):
    plt.figure(figsize=(14, 7))

    # Plot mean MRDT
    sns.lineplot(data=data, x='Year', y=value_column_mean, hue='Group', marker='o', linestyle='-')

    plt.title(title)
    plt.xlabel('Year')
    plt.ylabel(ylabel)
    plt.legend(title='Group')
    plt.show()

# Plot mean MRDT for females
plot_mean_mrdt(combined_mean_mrdt, 'MRDT_Female_mean', 'MRDT (Female) Over Years', 'MRDT (Female)')

# Plot mean MRDT for males
plot_mean_mrdt(combined_mean_mrdt, 'MRDT_Male_mean', 'MRDT (Male) Over Years', 'MRDT (Male)')

# Plot mean MRDT for total population
plot_mean_mrdt(combined_mean_mrdt, 'MRDT_Total_mean', 'MRDT (Total) Over Years', 'MRDT (Total)')
```

## License

This project is licensed under the MIT License - see the LICENSE file for details.

---

Feel free to customize this README further based on your specific requirements.
