## **A COMPREHENSIVE U.S BORDER CROSSING ANALYSIS DASHBOARD**

### **Introduction:**
Borders play a crucial role in a nation's economy, security, and global relations. The United States shares extensive borders with Canada and Mexico, which serve as vital channels for trade, tourism, and immigration. Monitoring and analyzing the patterns of crossings at these borders is essential for ensuring the smooth operation of ports, optimizing border management strategies, and responding to emerging trends or policy changes. This project is designed to provide a comprehensive analysis of U.S. border crossings using a Power BI dashboard, offering valuable insights into the flow of traffic, the performance of key ports, and the impact of various crossing types. By leveraging advanced data visualization techniques and DAX (Data Analysis Expressions) measures, this dashboard aims to be a powerful tool for stakeholders, enabling them to make informed decisions based on data-driven insights.

### **Objectives:**

The primary purpose of this dashboard is to visualize and analyze border crossing activity across the U.S.-Canada and U.S.-Mexico borders. This includes:

**Understanding Traffic Patterns:**
By visualizing trends over time, the dashboard helps identify patterns in border crossings, such as seasonal fluctuations, growth, or decline in traffic, and the impact of external factors like policy changes or global events.

**Analyzing Port Performance:**
The dashboard provides insights into the performance of individual ports, highlighting high-traffic areas and comparing the efficiency of different ports based on various metrics.

**Exploring the Distribution of Crossing Types:**
By breaking down the data by crossing type (e.g., personal vehicles, buses, pedestrians), the dashboard reveals how different types of crossings contribute to overall traffic and how they are distributed across various ports and borders.

**Geographical Insights:**
The map visualizations in the dashboard help stakeholders understand the geographical distribution of border crossings, identifying key regions and ports with high traffic volumes.
Interactive Exploration:
The dashboard is designed for interactive use, allowing users to filter the data by date, port, crossing type, and other factors, enabling customized analysis that meets specific user needs.

### **Dataset Overview:**
The dataset used for this analysis is the Border Crossing Entry Data, which provides detailed information on inbound crossings at U.S.-Canada and U.S.-Mexico borders. Key attributes of the dataset include:

**Port Name:** The specific port where the crossing occurred.
**State:** The U.S. state where the port is located.
**Port Code:** A unique identifier for each port.
**Border:** The border (U.S.-Canada or U.S.-Mexico) at which the crossing took place.
**Date:** The month and year of the crossing data.
**Measure:** The type of crossing (e.g., Buses, Trucks, Pedestrians).
**Value:** The number of crossings.
**Latitude & Longitude:** Geospatial data for the port's location.
The data is collected monthly, providing a temporal aspect that is crucial for analyzing trends and changes over time.

### **DAX Measures Created**
DAX measures were created to support the in-depth analysis of border crossings. Each measure was crafted with a specific purpose in mind, ensuring that the dashboard delivers actionable insights.

**1. Total Crossings**

Formula: Total Crossings = SUM('Border_Crossing_Entry_Data'[Value])

**Purpose:**
This measure calculates the total number of crossings recorded in the dataset for any selected criteria (e.g., by port, by crossing type, by date).

**Analytical Value:**
Overview of Traffic Volume: This measure serves as the fundamental metric for understanding the total volume of border crossings. It is essential for any analysis that seeks to quantify traffic at a high level.
Benchmarking: It can be used to compare total crossings across different ports, borders, or time periods, providing a benchmark for performance.
Input for Other Calculations: This measure is often the base for more complex calculations like averages, growth percentages, or ratios.

**2. Crossing Trend (3-Month Moving Average)**

Formula:
Crossing Trend (3-Month Moving Average) = 
CALCULATE(
    AVERAGEX(
        DATESINPERIOD('Border_Crossing_Entry_Data'[Date], 
                      LASTDATE('Border_Crossing_Entry_Data'[Date]), 
                      -3, 
                      MONTH), 
        [Total Crossings]
    )
)

**Purpose:**
This measure calculates the moving average of border crossings over the last three months, smoothing out short-term fluctuations to highlight longer-term trends.

**Analytical Value:**
Trend Analysis: The 3-month moving average helps to identify underlying trends in the data by reducing the noise caused by short-term variations. This is especially useful in spotting seasonal trends or long-term shifts in border traffic.
Predictive Insights: By analyzing the trend, stakeholders can predict future traffic patterns, aiding in decision-making related to resource allocation or policy adjustments.
Comparative Analysis: The trend can be compared across different ports or crossing types to see which areas are experiencing growth or decline.

**3. Port Crossing Percentage**

Formula:
Port Crossing Percentage = 
DIVIDE(
    SUM('Border_Crossing_Entry_Data'[Value]),
    CALCULATE(SUM('Border_Crossing_Entry_Data'[Value]), ALL('Border_Crossing_Entry_Data'[Port Name]))
) * 100

**Purpose:**
This measure calculates the percentage of total crossings that occur at each port, providing a sense of how significant each port is relative to others.

**Analytical Value:**
Port Significance: This measure highlights which ports are the most significant in terms of traffic volume. Ports with higher percentages are critical nodes in the border crossing network and may require more resources or attention.
Performance Monitoring: By tracking the percentage over time, stakeholders can monitor how the significance of different ports changes, possibly indicating shifts in trade routes or travel patterns.
Resource Allocation: Ports with a higher percentage of crossings may need more resources (e.g., staffing, infrastructure improvements) to manage their traffic effectively.

**4. Cumulative Crossings**

Formula:
Cumulative Crossings = 
CALCULATE(
    [Total Crossings],
    FILTER(
        ALL('Border_Crossing_Entry_Data'[Port Name]),
        'Border_Crossing_Entry_Data'[Port Name] <= MAX('Border_Crossing_Entry_Data'[Port Name])
    )
)

**Purpose:**
This measure calculates the cumulative total of crossings up to the current data point, helping to show how traffic accumulates over time.

**Analytical Value:**
Progress Tracking: This measure is useful for tracking progress over time, showing how the total number of crossings accumulates. It’s particularly useful in time series analysis or for cumulative reporting periods.
Trend Analysis: By examining the cumulative total, stakeholders can identify periods of rapid growth or decline in traffic.
Goal Setting: Cumulative crossings can be compared against targets or goals to assess performance.

**5. Cumulative Percentage**

Formula:
Cumulative Percentage = 
DIVIDE([Cumulative Crossings], CALCULATE(SUM('Border_Crossing_Entry_Data'[Value]), ALL('Border_Crossing_Entry_Data')))

**Purpose:**
This measure calculates the cumulative percentage of total crossings, which can help in understanding the contribution of each data point to the overall total.
Analytical Value:
Contribution Analysis: Cumulative percentage helps in understanding how much each port or crossing type contributes to the overall total as data accumulates. This is valuable for identifying key contributors and outliers.
Pareto Analysis: This measure can support Pareto analysis by identifying the point at which 80% of the traffic is accumulated, which can highlight the most important factors driving overall performance.
Performance Milestones: It’s useful for tracking when certain milestones are reached (e.g., the first 50% of total crossings).

**6. Passenger to Vehicle Ratio**

Formula:
Passenger to Vehicle Ratio = 
DIVIDE([Total Passengers], [Total Vehicles])

**Purpose:**
This measure calculates the ratio between passenger crossings and vehicle crossings, providing insight into the balance between these two types of traffic.
Analytical Value:
Traffic Composition: This ratio helps in understanding the composition of traffic at different ports or borders. A higher ratio of passengers to vehicles may indicate ports that are more oriented towards pedestrian traffic.
Resource Allocation: Ports with a high passenger-to-vehicle ratio may require different resources (e.g., more pedestrian processing facilities) compared to those dominated by vehicles.
Trend Monitoring: By tracking changes in this ratio over time, stakeholders can monitor shifts in the nature of border traffic, possibly reflecting changes in travel behavior or economic activity.

**7. Previous Year Crossings**

Formula:
Previous Year Crossings = 
CALCULATE(
    [Total Crossings], 
    SAMEPERIODLASTYEAR('Border_Crossing_Entry_Data'[Date])
)

**Purpose:**
This measure retrieves the total number of crossings from the same period in the previous year, enabling year-over-year comparisons.

**Analytical Value:**
Year-over-Year Comparison: This measure is crucial for identifying trends and patterns by comparing current data with the same period in the previous year. It helps to understand whether traffic is increasing, decreasing, or remaining stable.
Impact Analysis: By comparing with the previous year, stakeholders can assess the impact of new policies, economic conditions, or external events on border traffic.
Forecasting: Historical comparisons provide a basis for forecasting future trends, especially when used in conjunction with other measures like YoY Growth %.

**8. YoY Growth %**

Formula:
YoY Growth % = 
DIVIDE(
    [Total Crossings] - [Previous Year Crossings], 
    [Previous Year Crossings]
)

**Purpose:**
This measure calculates the year-over-year growth percentage in border crossings, showing how traffic has changed relative to the same period in the previous year.
Analytical Value:
Trend Identification: YoY Growth % is a key metric for identifying long-term trends. Positive growth indicates increasing traffic, while negative growth suggests a decline.
Performance Evaluation: This measure helps evaluate the effectiveness of border management strategies or policies implemented in the past year.
Decision-Making: By understanding growth trends, stakeholders can make informed decisions about where to focus resources or how to adjust strategies to manage border traffic effectively.

**9. Total Passengers**

Formula:
Total Passengers = 
CALCULATE(
    SUM('Border_Crossing_Entry_Data'[Value]), 
    'Border_Crossing_Entry_Data'[Crossing Type] IN {"Passengers", "Pedestrians", "Buses"}
)

**Purpose:**
This measure sums the total number of passenger crossings, focusing on pedestrian and bus traffic.
Analytical Value:
Passenger Flow Analysis: This measure provides a clear view of passenger traffic across borders, which is critical for understanding the movement of people rather than goods.
Operational Planning: High passenger volumes may require more processing resources, security personnel, or facilities at certain ports.
Crossing Type Comparison: By comparing passenger traffic with vehicle traffic (using the Passenger to Vehicle Ratio), stakeholders can gain insights into the balance between the two and how it changes over time.

**10. Total Vehicles**

Formula:
Total Vehicles = 
CALCULATE(
    SUM('Border_Crossing_Entry_Data'[Value]), 
    'Border_Crossing_Entry_Data'[Crossing Type] IN {"Personal Vehicles", "Trucks", "Commercial Vehicles"}
)

**Purpose:**
This measure sums the total number of vehicle crossings, focusing on personal vehicles, trucks, and commercial vehicles.
Analytical Value:
Traffic Management: Understanding the volume of vehicle traffic is crucial for managing infrastructure and resources at ports, especially for lanes and facilities designed for vehicles.
Economic Insights: Vehicle traffic, particularly commercial vehicles, is often a proxy for economic activity, especially in regions dependent on cross-border trade.
Comparative Analysis: Comparing vehicle traffic across different ports
