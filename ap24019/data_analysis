#1. Import Libraries
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import geopandas as gpd  # For geospatial analysis

#2. Load and Prepare the Data
# Load the earthquake dataset
file_path = 'earthquake_data.csv'  # Replace with your actual file path
df = pd.read_csv(file_path)

# Combine date and time into a single datetime column
df['datetime'] = pd.to_datetime(df['date'] + ' ' + df['time'])

# Drop original date and time columns
df.drop(columns=['date', 'time'], inplace=True)

# Convert depth and magnitude to numeric
df['depth'] = pd.to_numeric(df['depth'], errors='coerce')
df['magnitude'] = pd.to_numeric(df['magnitude'], errors='coerce')

# Check for missing data
print("Missing Data:\n", df.isnull().sum())

# Drop rows with missing values for simplicity
df.dropna(inplace=True)

3. Trend Analysis
# Set the datetime as the index
df.set_index('datetime', inplace=True)

# Resample to monthly counts
monthly_counts = df.resample('M').size()

# Plot the trend
plt.figure(figsize=(12, 6))
monthly_counts.plot()
plt.title('Monthly Earthquake Counts')
plt.xlabel('Time')
plt.ylabel('Number of Earthquakes')
plt.grid()
plt.show()

#4. Magnitude Distribution
plt.figure(figsize=(10, 6))
sns.histplot(df['magnitude'], bins=30, kde=True)
plt.title('Magnitude Distribution')
plt.xlabel('Magnitude')
plt.ylabel('Frequency')
plt.grid()
plt.show()

#5. Geospatial Analysis (Hotspots)
# Convert to GeoDataFrame
gdf = gpd.GeoDataFrame(df, geometry=gpd.points_from_xy(df['longitude'], df['latitude']))

# Plot the earthquakes on a map
world = gpd.read_file(gpd.datasets.get_path('naturalearth_lowres'))

plt.figure(figsize=(15, 10))
world.plot(ax=plt.gca(), color='lightgray')
gdf.plot(ax=plt.gca(), color='red', alpha=0.5, markersize=10)
plt.title('Earthquake Locations')
plt.xlabel('Longitude')
plt.ylabel('Latitude')
plt.show()

#6. Depth and Magnitude Correlation
plt.figure(figsize=(10, 6))
sns.scatterplot(data=df, x='depth', y='magnitude', hue='source', alpha=0.7)
plt.title('Depth vs Magnitude')
plt.xlabel('Depth (km)')
plt.ylabel('Magnitude')
plt.grid()
plt.show()

# Calculate and display correlation
correlation = df[['depth', 'magnitude']].corr()
print("Correlation between Depth and Magnitude:\n", correlation)

#7. Source Comparison
# Boxplot of magnitudes by source
plt.figure(figsize=(12, 6))
sns.boxplot(data=df, x='source', y='magnitude')
plt.title('Magnitude Distribution by Source')
plt.xlabel('Source')
plt.ylabel('Magnitude')
plt.grid()
plt.show()

#8. Time of Day Analysis
# Extract hour of the day
df['hour'] = df.index.hour

# Count earthquakes by hour
hourly_counts = df['hour'].value_counts().sort_index()

# Plot the hourly distribution
plt.figure(figsize=(12, 6))
hourly_counts.plot(kind='bar')
plt.title('Earthquake Frequency by Hour of the Day')
plt.xlabel('Hour of the Day')
plt.ylabel('Number of Earthquakes')
plt.grid()
plt.show()

#9. Save the Results
# Save cleaned data
df.to_csv('cleaned_earthquake_data.csv', index=False)
