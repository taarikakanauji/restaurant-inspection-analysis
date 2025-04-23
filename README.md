# Restaurant Inspection Analysis - Power BI Service 

![Power BI Service H](https://github.com/taarikakanauji/restaurant-inspection-analysis/raw/main/images/Power-BI-Service-H.jpg)

## Problem Statement

California’s food safety regulators face challenges in:

`Uneven Inspection Coverage`: Some regions have significantly fewer inspections than others

`Recurring Violations`: High-frequency issues persist across regions

`Compliance Gaps`: Restaurants score Grade B or lower, risking public health.

`Resource Allocation`: Lack of granular insights leads to inefficient inspector deployment.

This dashboard helps health department officials, restaurant owners, and food safety stakeholders track and analyze inspection results across regions, violation types, and time periods. It provides data-driven insights to identify problem areas, optimize inspection resources, and improve overall food safety performance. 
The Key Stakeholders are:

  - Health Department Administrators
  - Food Safety Inspectors
  - Restaurant Owners/Operators
  - City Planning Officials
  - Public Health Researcher


## Objective

- Monitor inspection compliance by tracking key metrics including total inspections, violation rates, and sanitation grades
- Analyze and compare violation trends across different regions to prioritize inspection resources
- Identify common violation types to target for education and prevention
- Track progress toward food safety goals across the country
- Pinpoint geographical areas needing additional oversight or support


## Key Questions

    Q : Which regions have the highest violation rates and need focused attention?

    Q : Which areas are meeting or exceeding food safety goals?

    Q : Where should we allocate additional inspection resources?

    Q : What are the most common types of violations occurring?

    Q : Which regions have the lowest inspection rates?

    Q : Do violations spike seasonally?

    Q : Which zip codes have the highest % of Grade B/C restaurants? 

    Q : Are violations correlated with inspection frequency?

## Steps followed 

As a Business Analyst, I led the development of an interactive Restaurant Inspection Dashboard designed to provide health regulators, inspectors, and policymakers with a centralized, data-driven view of food safety compliance, violation trends, and inspection efficiency across all regions.

The primary goal was to transform raw inspection data into actionable insights, enabling stakeholders to:
Target high-risk violations, Optimize inspector deployment to underserved regions, Improve public health outcomes by reducing Grade B/C restaurants , Align resources with city goals.

Below is my step-by-step approach to create complete dashboard in Power BI Service, without Power BI Desktop:

- Step 1 : I have **installed Data Gateway standard mode** and did setup with credentials and path.

To begin, I navigated to Settings > Manage Connections & Gateway within the Power BI service. Under the On-premises data gateway tab, I verified the availability of our gateway cluster by checking key details such as the cluster name, associated email, user info, and ensuring the status was online.

Once verified, I proceeded to create new data connections by selecting + New. Here, I chose the On-premises option, selected the gateway cluster from the dropdown, assigned a connection name, specified the connection type as ‘File’, provided the file path, and configured Windows authentication. After clicking Create, the connection was successfully established. I validated all connections by checking the file paths under the Check Connections tab.

- Step 2:  Creating a **Dataflow (Gen 1)** for Data Transformation
With the connections in place, I created a Gen 1 Dataflow—Power Query in the cloud, which mirrors the functionality of Power BI Desktop. In the Get Data window, I selected Text/CSV as the data source and used the existing on-premises gateway to access files via the provided paths.

After loading the files, I clicked on Transform Data, which launched Power Query. I imported and transformed the following tables:

calendar_lookup – Added calculated columns for Year, Quarter, Month, Start of Year, Start of Month, and Start of Week.

city_lookup

Inspector_lookup

Region_lookup

restaurant_inspection_data

restaurant_name_lookup – Applied promoted headers.

sanitary_grade_lookup – Applied promoted headers.

Once all transformations were completed, I saved and closed the dataflow and triggered a refresh to ensure data readiness for the next step.

- Step 3: Building the **Semantic Model in Power BI Desktop**
Next, I moved to Power BI Desktop to create the semantic model. I imported the dataflow (Gen 1) by choosing Get Data > Dataflows and connected to the published dataflow in the Power BI service.

In the model view, I established all necessary relationships between tables using primary and foreign keys, ensuring referential integrity and enabling seamless reporting. After validating the model structure, I published it to the appropriate Power BI workspace along with a blank report, laying the foundation for the upcoming analysis.

- Step 4: **Registering the Semantic Model and Blank Report**
Post-publication, I confirmed that the newly created semantic model and associated blank report were visible in the Power BI service under the designated workspace.
To ensure workflow visibility, I also verified its status in the task flow window, confirming successful registration and availability for report building.


- Step 5: **Creating Visualizations in Power BI Service**
With the data model in place and the report structure ready, I began the process of developing insightful visuals within Power BI Service.

- Step 6: **Designing KPI Cards for Key Metrics** – To give stakeholders a quick and clear snapshot of overall performance, I designed KPI cards that showcase critical engagement metrics. These KPIs deliver real-time insights into business activity, enabling swift and informed decision-making.

  The total number of restaurant inspections conducted in the current month by comparing it to previous month, helps assess whether health departments are meeting inspection targets

  `Fields Used:`
  
  Value: Total Inspections
  Trend Axis: Start of month
  Target: Previous Month Inspection

  The mean inspection grade across all restaurants inspected in current month by comparing it to previous month, Indicates strong/weak compliance with food safety standards.

  `Fields Used:`
  
  Value: Avg sanitary grade
  Trend Axis: Start of month
  Target: Previous Month sanitory grade
      
      Total Inspections = COUNTROWS(VALUES(R_Restaurant_Inspection_Data[record_id]))

      Total violations = COUNT(R_Restaurant_Inspection_Data[violation_code]) -COUNTBLANK(R_Restaurant_Inspection_Data[violation_code])

      Previous month sanitary grade = CALCULATE([Avg sanitary grade],DATEADD(R_Calendar_Lookup[Transaction_Date],-1,MONTH))

      Previous month Inspections = CALCULATE([Total Inspections],DATEADD(R_Calendar_Lookup[Transaction_Date],-1,MONTH ))

      All Violations = CALCULATE([Total violations],ALL(R_Restaurant_Inspection_Data ))

      All inspections = CALCULATE([Total Inspections],ALL(R_Restaurant_Inspection_Data ))

      % of all Inspections = DIVIDE([Total Inspections],[All inspections])

      % of all violations = DIVIDE([Total violations],[All Violations])

      Avg sanitary grade = AVERAGE(R_Restaurant_Inspection_Data[score])

      Grade A = CALCULATE( COUNT(R_Restaurant_Inspection_Data[grade]),R_Restaurant_Inspection_Data[grade] = "A")

      Grade B = CALCULATE(COUNT(R_Restaurant_Inspection_Data[grade]),R_Restaurant_Inspection_Data[grade] = "B")

      Grade C = CALCULATE(COUNT(R_Restaurant_Inspection_Data[grade]),R_Restaurant_Inspection_Data[grade] = "C")


  All KPI cards were formatted using currency symbols where applicable, enhancing visual clarity and ensuring executives can quickly interpret them without needing to explore detailed datasets.

   ![KPI](https://github.com/taarikakanauji/restaurant-inspection-analysis/raw/main/images/kpi.jpg)

           
- Step 7 : **Inspection by Region & Restaurant Name** – I incorporated an interactive stacked bar chart to effectively represent inspection by region and name using drill up and drill down mode. 

        Fields Used:

        Y axis : Facility_region, facility_name

        X-axis: Total Inspections


   Los Angeles have the highest inspection volumes, suggesting either higher restaurant density. Central LA, Eastside, Northeast LA has the lowest inspection volume—could indicate underserved areas or fewer restaurants

  High-volume regions may strain inspector bandwidth, leading to rushed inspections.   Lower-volume regions might lack consistent oversight, masking recurring violations.

  For High-Volume Regions: Deploy additional inspectors or automate scheduling to reduce backlogs. For High-Violation Regions, Launch targeted training for common violations

  Use predictive analytics to shift inspectors to regions with rising violation trends


  ![Inspection Region Name](https://github.com/taarikakanauji/restaurant-inspection-analysis/raw/main/images/Inspection-Region-Name.jpg)

- Step 8 : **Violations by Region & Type** – I implemented a decomposition tree graph to illustrate violations by region and type. 

        Fields used:

        Analyze: total violations

        explain by: facility region, violation desciption

        tooltip: % of the violations

   Los Angeles has the highest total violations with floor/wall maintenance being the most frequent, likely due to higher restaurant density.

   High-violation regions may need more inspectors, but budget constraints could limit coverage. Repeat violations indicate poor adherence to standards despite inspections. Advocate for stricter penalties for repeat offenders in high-violation regions.


  ![Violation Region Type](https://github.com/taarikakanauji/restaurant-inspection-analysis/raw/main/images/Violation-region-type.jpg)

- Step 9 :  **Violation Trend** – I created a stacked column chart to showcase the monthly violation trend. This gives us a high-level picture of how the violation are performing monthly.

        Fields used:

        X-axis: Start of month
        Y- axis: Total violations 

  From the data, Violations peak in may, June, august months, possibly due to higher restaurant activity or perishable food risks and lowest in July month.

High-violation regions (e.g., LA) may need more inspectors, while low-volume areas could be underserved. Violation spikes in certain months may correlate with seasonal factors (e.g., summer crowds, holiday rushes).


  ![Violation Trend](https://github.com/taarikakanauji/restaurant-inspection-analysis/raw/main/images/violation-trend.jpg)


- Step 10 : **Sanitation Grade** - This donut chart describes the Grade A, B, C grades.

        Fields used:

        Legend: sanitation grade

        Values: grade A, B, C

  From the graph, we can pinpoint that 88.46% Grade A: Indicates strong overall compliance, but 10.48% Grade B and any Grade C restaurants still pose risks.

  San Gabriel Valley and San Fernando Valley have high inspection volumes, but their violation trends suggest maintenance challenges. Grade B/C restaurants: Mandate corrective action plans. Low inspection frequency in some regions could mask hidden risks. Reward top performers (e.g., "A+ Certification" for consistent Grade A restaurants)


    ![Sanitation Grade](https://github.com/taarikakanauji/restaurant-inspection-analysis/raw/main/images/sanitation-grade.jpg)

- Step 11 : **Restaurant Zip Code** - This is represented using a map that shows the zip code by total violations.

        Fields used:

        Location: Facility_city, facility_zip
        bubble size: total violations

  From this map, we can concur that Zip codes with the big bubble size have the most violations, suggesting areas needing urgent intervention.

Zip codes with high violations likely correlate with lower sanitation grades (B/C). Busy restaurants may struggle with consistent compliance. Aging equipment/facilities may lead to more violations. Mandatory staff training for restaurants with repeat violations.
Proactive maintenance programs for older restaurants and Public health campaigns in high-risk zones to educate owners & customers

    
![Map](https://github.com/taarikakanauji/restaurant-inspection-analysis/raw/main/images/zip-map.jpg)


- Step 12: **Adding Filters to the Dashboard** - To enhance user experience and interactivity, I incorporated 3 slicers that allow users to tailor the data view based on their preferences. 

  I added slicers for:

      Date 

      Inspector Name 

      Violation Code

    ![Slicer Pane](https://github.com/taarikakanauji/restaurant-inspection-analysis/raw/main/images/slicer-pane.jpg)

- Step 13: **Restaurant Inspection Detailed Analysis**

  ![Power BI Service D](https://github.com/taarikakanauji/restaurant-inspection-analysis/raw/main/images/Power-BI-Service-D.jpg)

This provides a geographical detailed analysis of restaurant health code violations across different regions, zip codes, and time periods. To Track violation trends, Identify high-risk areas, Allocate inspection resources effectively.

Monitor Violation Hotspots – Pinpoint zip codes with the highest violations.

Regional Comparison – Compare performance across regions

**Insights**

Geographic visualization of violations, with data point cluster with heatmap indicating higher violations in region.

High-risk zip codes likely have older restaurants or high density, Low-risk zip codes may reflect better compliance.

Seasonal spikes (e.g., summer months due to higher dining activity).

Root Cause -Are violations due to negligence, outdated facilities, or insufficient training?

Priority inspections in top-violation zip codes and training for restaurants

Stricter penalties for repeat offenders.

Incentives for consistent "A" grade restaurants.

Integrate customer complaint data for a fuller risk assessment.

- Step 14:  To ensure data security, I implemented both **static and dynamic RLS** through Manage Roles in Power BI Service

For static RLS, I manually defined roles with DAX filters based on specific field values (e.g., Los Angeles ).

For dynamic RLS, I used user-based filtering with the USERPRINCIPALNAME() function to ensure each user sees only their relevant data.

I navigated to semantic model > Security 
To validate, I used test As Role to test different user perspectives.

- Step 15: **Mobile Layout Optimization**
In Power BI Service, I selected Mobile Layout.

I rearranged the visuals by dragging and resizing them to fit a mobile screen format. This ensured a seamless viewing experience for users accessing the report on mobile devices.

- Step 16: Pinning Report to **Dashboard** & **Setting Data Alerts**

 I clicked Pin Live to a new dashboard. I created Data Alerts by hovering over KPI tiles in the dashboard and selecting the bell icon.
Here, I set thresholds (e.g., alert when value > 80), defined alert titles, and toggled email notifications.

- Step 17: **Subscription** Setup for Report Tracking
To ensure stakeholders receive timely updates:

I clicked on the report, selected Subscribe, and created a new subscription.I filled in details like Name, Start Date/Time, Frequency (daily/weekly), and output format (attached as PDF and PPT). This helped keep the team informed even without logging into the service.

- Step 18: Data **Lineage view** Check
Before scheduling refresh, I used the Lineage View to review the end-to-end data flow:
I went to the Workspace > Lineage view tab.

This helped me verify the connections between dataflows, semantic models, reports, and dashboards, ensuring there were no broken links.

- Step 19: **Schedule Refresh** Configuration

To automate data updates: I opened the Semantic model > Settings > Scheduled Refresh. I enabled refresh, selected the frequency (e.g., daily), and set the time of day. I also ensured the Dataflow was configured similarly by going into Dataflow > Settings. This ensured fresh data was always available for analysis.

- Step 20: Naming Each Layer in the **Task Flow**
For clarity in the lineage and navigation:
I gave meaningful names to each component:
Dataflow – collect data
Semantic Model – store data
Report – data visualizations
Dashboard – track data
This clear naming convention helped streamline communication within the team.

- Step 21: Publishing the **App**
To make the report accessible to end-users:
I went to Workspace > Create > App.
I configured app details like name, description, audience access, and selected the content (report + dashboard) to include.
Finally, I clicked Publish to share the app with users.

- Step 22: Final Checks Post-Publishing
After publishing:
I **reviewed the task flow**, verifying that:

**Next refresh** timing was displayed correctly

Components showed **Endorsement: Promoted**

The app was marked as **Included in App: Yes**

This gave me confidence that the deployment was clean and all dependencies were in place.

Throughout this journey, I focused not just on building a functional report, but also on making it accessible, secure, and insightful for end users. Every click and configuration was done with intent, ensuring a high-quality Power BI solution from development to delivery.

# Snapshot of Report (Power BI Service)

![Power BI Service H Full](https://github.com/taarikakanauji/restaurant-inspection-analysis/raw/main/images/Power-BI-Service-H-full.jpg)

# **Project Resources**  

### **Detailed Report (PDF)**  
[Insights of Restaurant Inspection Analysis](https://github.com/taarikakanauji/restaurant-inspection-analysis/blob/main/Restaurant%20Inspection%20Analysis.pdf)  

### **Power BI Service Demo**  
[Power BI Service Demo (MKV)](https://github.com/taarikakanauji/restaurant-inspection-analysis/blob/main/Power-Service-Demo.mkv)  

### **Project README (Setup Guide)**  
[README.md](https://github.com/taarikakanauji/restaurant-inspection-analysis/blob/main/README.md)  

# Conclusion

By leveraging the insights and data-driven strategies from this dashboard, stakeholders can expect:

- Targeted Inspections: Focus resources on high-violation regions (e.g., Los Angeles, San Fernando Valley) and common violations
- Higher Sanitation Grades: Reduce Grade B/C restaurants by addressing recurring violations
- Trend-Based : Use violation trends (seasonal spikes) to deploy inspectors or training programs.
- Reduced Violations: Proactively address frequent issues to lower violation rates MoM
- Accountability: Clear regional benchmarks help track progress and justify resource allocation.
- Staff & Budget Prioritization: Redirect efforts to regions with rising violations or low inspection volume.

As a result, A safer, more efficient, and transparent food safety ecosystem, where: Restaurants meet higher cleanliness standards, boosting public trust. Health departments optimize inspections, saving time and costs. Communities benefit from reduced foodborne illness risks and equitable oversight. By acting on these insights, stakeholders will create a data-driven, proactive, and high-compliance food safety environment across California



