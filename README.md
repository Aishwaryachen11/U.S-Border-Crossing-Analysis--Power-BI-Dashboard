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


### **Visualizations and Interactions**

1. Sankey Diagram

<img src="https://github.com/Aishwaryachen11/U.S-Border-Crossing-Analysis-Power-BI-Dashboard/blob/main/Images/Sankey%20Chart.png" alt="Description" width="400"/>
 
**Purpose:**
The Sankey Diagram is used to visualize the flow of border crossings from different crossing types to specific borders (U.S.-Mexico and U.S.-Canada). It highlights the distribution and volume of traffic across different categories, providing a clear picture of how traffic is segmented by crossing type and border.

**Insights:**
Flow of Traffic: The Sankey Diagram illustrates the flow of traffic, showing which types of crossings (e.g., trucks, buses, pedestrians) dominate at each border.
Distribution Patterns: By examining the flow, users can identify which crossing types contribute the most to traffic at each border. For example, it might reveal that personal vehicles dominate crossings at the U.S.-Mexico border, while pedestrians are more prevalent at certain U.S.-Canada border ports.
Port Strategy: The diagram can inform border management strategies by showing where resources should be allocated based on the volume and type of crossings at each border.

**2. Decomposition Tree**

<img src="https://github.com/Aishwaryachen11/U.S-Border-Crossing-Analysis-Power-BI-Dashboard/blob/main/Images/Decomposition%20Tree.png" alt="Description" width="400"/>

**Purpose:**
The Decomposition Tree is a powerful tool for breaking down the factors that contribute to total border crossings. It allows users to explore the data interactively, drilling down into various dimensions such as border, crossing type, and state to understand what drives traffic volumes.

**Insights:**
Root Cause Analysis: The Decomposition Tree helps identify the key drivers behind trends in border traffic. For example, users can drill down to see which states contribute most to U.S.-Mexico crossings or which crossing types are most prevalent in a particular region.
Multi-Level Analysis: The tree allows for a hierarchical exploration of the data, enabling users to start with a broad analysis (e.g., total crossings) and drill down into specific details (e.g., the performance of specific ports within a state).
Comparative Insights: By comparing different branches of the tree, users can quickly identify differences in performance or trends across various categories, such as comparing how different states manage pedestrian traffic versus vehicle traffic.

**3. Line and Clustered Column Chart (Combo Chart)**

<img src="https://github.com/Aishwaryachen11/U.S-Border-Crossing-Analysis-Power-BI-Dashboard/blob/main/Images/Line%20and%20Clustered%20Column%20Chart.png" alt="Description" width="400"/>

**Purpose:**
The Line and Clustered Column Chart combines trend analysis with actual crossing volumes, providing a comprehensive view of port performance over time. This visualization is particularly useful for identifying seasonal patterns, trends, and anomalies in crossing data at specific ports.

**Insights:**
Trend Analysis: The line component of the chart represents the moving average of crossings, smoothing out short-term fluctuations to reveal underlying trends. This helps in identifying long-term growth or decline at specific ports.
Volume Comparison: The clustered columns show the actual number of crossings, allowing users to compare volumes across different years or ports. This can highlight which ports have experienced significant changes in traffic.
Seasonal Patterns: By observing the combination of trends and actual volumes, users can identify seasonal patterns or anomalies, such as a sudden drop in traffic that may correlate with external events (e.g., policy changes, economic factors).

**4. Time Series Analysis: Line Chart**

<img src="https://github.com/Aishwaryachen11/U.S-Border-Crossing-Analysis-Power-BI-Dashboard/blob/main/Images/Line%20Chart.png" alt="Description" width="400"/>

**Purpose:**
The Time Series Line Chart is used to visualize the overall trend in border crossings over time, focusing on how traffic volumes have changed year over year. This chart provides a clear and straightforward view of long-term trends in border activity.

**Insights:**
Historical Trends: The line chart shows the progression of border traffic over the years, helping users understand how volumes have evolved. This is particularly useful for identifying periods of growth, decline, or stability.
Impact of External Factors: The chart can highlight how external factors (such as new regulations, economic downturns, or global events like pandemics) have impacted border traffic. Spikes or dips in the line can be linked to specific events or periods.
Comparative Analysis: By overlaying data from different years, users can compare how traffic has changed year over year, providing insights into the effectiveness of policies or changes in border management practices.

**5. Donut Chart: Passenger and Vehicle Ratio Analysis**

<img src="https://github.com/Aishwaryachen11/U.S-Border-Crossing-Analysis-Power-BI-Dashboard/blob/main/Images/Donut%20Chart.png" alt="Description" width="400"/>

**Purpose:**
The Donut Chart is designed to compare the ratio of passenger crossings to vehicle crossings. It provides a visual representation of how different types of crossings contribute to the overall border traffic.

**Insights:**
Crossing Type Distribution: The Donut Chart visually displays the proportion of crossings that are passengers versus vehicles. This can reveal whether certain ports or borders are more focused on passenger traffic or vehicle traffic.
Resource Allocation: By understanding the dominant crossing type, stakeholders can allocate resources more effectively. For example, a port with a high vehicle crossing ratio might need more infrastructure to handle large volumes of trucks.
Comparative Analysis: The chart also allows for quick comparisons between different ports or borders, showing which locations have a higher proportion of passenger traffic relative to vehicles.

**6. Geographical Analysis: Map Visualization**

<img src="https://github.com/Aishwaryachen11/U.S-Border-Crossing-Analysis-Power-BI-Dashboard/blob/main/Images/Map%20Visualization.png" alt="Description" width="400"/>

**Purpose:**
The Map Visualization provides a geographical overview of border crossing activity, showing the number of crossings at each port of entry along the U.S.-Canada and U.S.-Mexico borders. This visualization helps users understand the spatial distribution of traffic.

**Insights:**
Regional Traffic Patterns: The map reveals which regions and ports experience the most traffic, helping users identify high-traffic areas and potential bottlenecks.
Cross-Border Comparisons: By comparing the size of bubbles (which represent the volume of crossings), users can quickly see the differences in traffic between U.S.-Canada and U.S.-Mexico borders.
Operational Efficiency: The map can inform decisions about where to allocate resources or make improvements, such as upgrading infrastructure at high-traffic ports or enhancing security measures at key locations.

**7. Matrix Visualization**

<img src="https://github.com/Aishwaryachen11/U.S-Border-Crossing-Analysis-Power-BI-Dashboard/blob/main/Images/Matrix.png" alt="Description" width="200"/>

**Purpose:**
The Matrix Visualization is used to list the top ports with high traffic, along with their respective crossing percentages. It provides a tabular view of the data, enhanced with data bars for better visual comparison.

**Insights:**
Port Ranking: The matrix highlights the top-performing ports in terms of crossing percentages, helping users quickly identify which ports handle the most traffic.
Visual Comparison: The data bars make it easy to compare the crossing percentages across different ports, showing at a glance which ports dominate the traffic landscape.
Detailed Analysis: The matrix format allows for a detailed, granular analysis of crossing data, supporting decision-making at both strategic and operational levels.

**8. Multi-Row Card Visualization**

<img src="https://github.com/Aishwaryachen11/U.S-Border-Crossing-Analysis-Power-BI-Dashboard/blob/main/Images/Multi-Row%20Card.png" alt="Description" width="200"/>

**Purpose:**
The Multi-Row Card is designed to display key metrics for high-traffic ports, providing a quick snapshot of total crossings for each port. This visualization is particularly useful for highlighting the most

### **Recommendations**
Based on the comprehensive analysis conducted through the U.S. Border Crossing Analysis Dashboard, several key recommendations emerge that can guide stakeholders in optimizing border operations, enhancing security, and improving the flow of people and goods across borders.

1. Optimize Resource Allocation Based on Port Performance
Recommendation: Allocate resources dynamically based on the traffic volume and type at each port. Ports identified as high-traffic locations, particularly those with a significant proportion of commercial vehicle crossings, should be prioritized for infrastructure upgrades, staffing increases, and enhanced security measures.
Rationale: The data reveals that certain ports handle a disproportionate share of traffic. By focusing resources on these high-traffic ports, authorities can reduce bottlenecks, improve processing times, and enhance overall operational efficiency.

2. Enhance Infrastructure for Passenger and Pedestrian Crossings
Recommendation: Ports with a high passenger-to-vehicle ratio should see investments in pedestrian processing facilities, including dedicated lanes, shelters, and security screening areas.
Rationale: The analysis indicates that some ports have a higher ratio of passenger traffic, especially on the U.S.-Mexico border. Improving infrastructure for pedestrians will not only enhance the user experience but also increase the capacity to handle larger volumes of people efficiently.

3. Implement Data-Driven Border Management Policies
Recommendation: Utilize the insights from the dashboard to inform and adjust border management policies. For example, policies that impact trade or travel should consider the year-over-year growth trends and the types of crossings most affected.
Rationale: The YoY Growth % measure provides clear indications of how traffic is changing. Policymakers can use this information to anticipate the impacts of new regulations, border closures, or security measures, ensuring that policies are responsive to real-world trends.

4. Prepare for Seasonal Fluctuations
Recommendation: Develop and implement strategies to manage seasonal peaks in border crossings, such as deploying additional temporary staff during high-traffic periods or adjusting hours of operation.
Rationale: The time series analysis reveals seasonal patterns in border crossings, particularly in regions with high tourist activity. Being prepared for these fluctuations can prevent congestion and maintain smooth operations during peak periods.

5. Focus on High-Impact Ports for Security Enhancements
Recommendation: Prioritize security upgrades at ports that handle a significant volume of crossings but are also identified as critical points in the flow of high-value or sensitive goods.
Rationale: Ports with high crossing percentages and significant commercial vehicle traffic may be more vulnerable to smuggling or other security threats. By focusing security enhancements at these critical points, authorities can better safeguard against illicit activities while maintaining efficient trade flows.

6. Regularly Review and Update Border Management Strategies
Recommendation: Establish a process for regular review and adjustment of border management strategies using the dashboard’s data. This should include quarterly reviews of port performance, crossing types, and trends.
Rationale: The border crossing environment is dynamic, with changes driven by economic conditions, geopolitical events, and regulatory adjustments. Regular reviews ensure that strategies remain effective and responsive to these changes.

### **Conclusion**
The U.S. Border Crossing Analysis Dashboard provides a powerful and comprehensive tool for understanding the complexities of border traffic at the nation’s busiest points of entry. Through detailed visualizations and advanced DAX calculations, the dashboard delivers critical insights into the flow of people and goods across borders, the performance of individual ports, and the impacts of external factors on crossing trends.

The analysis highlights several key findings:

**Significant Port Disparities:** Certain ports, particularly those along the U.S.-Mexico border, handle a disproportionate share of traffic, indicating the need for targeted resource allocation.

**Variability in Crossing Types:** The dashboard reveals distinct differences in the types of crossings across various ports and borders, with some ports serving primarily as passenger entry points, while others are dominated by commercial vehicle traffic.

**Impact of External Factors:** Time series analysis and year-over-year comparisons highlight the significant impact of external factors, such as policy changes, economic conditions, and global events, on border traffic volumes.
These insights underscore the importance of data-driven decision-making in border management. By leveraging the dashboard’s capabilities, stakeholders can make informed decisions that enhance operational efficiency, improve security, and facilitate smoother cross-border movements. Moreover, the interactive nature of the dashboard ensures that it can be continuously updated and refined, keeping pace with the evolving landscape of border crossings.

In conclusion, the U.S. Border Crossing Analysis Dashboard not only serves as a valuable tool for current operations but also lays the groundwork for future strategic planning. By following the recommendations provided, border authorities can optimize their management practices, respond effectively to emerging trends, and ensure that the U.S. border remains secure and efficient in the face of changing challenges.



