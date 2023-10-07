# BioUrja_Test
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
dataset = pd.read_csv('biourja_Data.csv')
data={ 'Zone': ['East', 'North', 'West', 'South'],
        'Zonal Forecast': [2800, 1500, 2000, 6500]}
df = pd.DataFrame(data)
individual_forecasts = dataset[['Plant_Name', 'Zone', 'Forecast', 'Capacity']]
zonal_forecasts = df[['Zone', 'Zonal Forecast']]
state_forecast = 12000
total_sum = df['Zonal Forecast'].sum()
print(total_sum)
# Calculate the ratio for each row
df['Ratio'] = df['Zonal Forecast'] / total_sum
# Create a new column with the values obtained by multiplying the ratio
df['NewValue'] = df['Ratio'] * state_forecast
# Display the updated DataFrame
print(df)
area_sums = individual_forecasts.groupby('Zone')['Forecast'].sum().reset_index()
print(area_sums)
zone_sums = individual_forecasts.groupby('Zone')['Forecast'].sum().reset_index()
print(zone_sums)
df['Ratio_to_Zone_Sum'] = df.apply(lambda row: row['NewValue'] / zone_sums.loc[zone_sums['Zone'] == row['Zone'], 'Forecast'].values[0], axis=1)
ratio_list = df['Ratio_to_Zone_Sum'].tolist()


# Display the list of ratios
print(ratio_list)


# Display the list of zone-wise ratios
print(zone_ratio_mapping)
individual_forecasts['Forecast_Capacity_Ratio'] = individual_forecasts['Forecast'] / individual_forecasts['Capacity']


# Display the updated DataFrame with the new column
print(individual_forecasts)
zone_ratio_mapping = {'East': ratio_list[0],
                      'North': ratio_list[1],
                      'West': ratio_list[2],
                      'South': ratio_list[3]}


# Add a new column with zone-wise ratios linked correctly to the zone
individual_forecasts['Zone_Wise_Ratio'] = individual_forecasts['Zone'].map(zone_ratio_mapping)


# Display the updated DataFrame with the new column
print(individual_forecasts)
individual_forecasts['New Forecast']=individual_forecasts['Zone_Wise_Ratio'] * individual_forecasts['Capacity']*individual_forecasts['Forecast_Capacity_Ratio']
print(individual_forecasts)
individual_forecasts['Minimum'] = individual_forecasts.apply(lambda row: min(row['New Forecast'], row['Capacity']), axis=1)
print(individual_forecasts)
Final_Answer = individual_forecasts[['Plant_Name', 'Minimum']].copy()


# Display the new DataFrame
print(Final_Answer)
pd.set_option('display.max_rows', None)  # Show all rows
pd.set_option('display.max_columns', None)  # Show all columns
Final_Answer['Combined_Columns'] = Final_Answer.apply(lambda row: f"{row['Plant_Name']}, {row['Minimum']}", axis=1)
# Display the DataFrame with the new column
print(Final_Answer['Combined_Columns'])
