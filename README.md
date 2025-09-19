# Driving-Operational-Efficiency-with-Analytics-in-Broadcasting
This project applies forecasting, simulation, and optimisation to broadcasting operations using synthetic data. Analyses include call demand forecasting, subscription prediction, inventory optimisation, engineer job allocation, and queue simulation to boost efficiency and customer satisfaction.
# Project Overview
This project explores how analytics can drive improvements in the operational activities of Delta Sky Broadcasting (DSB), with a focus on data-informed decision-making across key business functions. In today’s dynamic broadcasting environment, operational efficiency, customer responsiveness, and cost optimisation are critical to maintaining a competitive edge. By applying analytical techniques to areas such as demand forecasting, inventory management, workforce allocation, and queue optimisation, DSB can gain actionable insights that support strategic planning and daily operations.

The project is structured around a series of analyses that include identifying call demand patterns, forecasting new subscription volumes, optimising inventory through simulation, allocating engineers using linear programming, and improving service queue performance via simulation. Each stage demonstrates how analytical tools and models not only reveal hidden patterns and inefficiencies but also provide evidence-based recommendations to streamline operations, reduce costs, and enhance customer satisfaction. Together, these insights offer a comprehensive blueprint for transforming DSB’s operational capabilities through data-driven decision-making. 
# Investigating New Subscription Call Demand
In a competitive industry like broadcasting, companies that succeed in attracting new consumers tend to perform better, as consumers are generally reluctant to switch between services. As such, missing out on potential new subscriptions would result in a significant opportunity loss for DSB. To effectively manage attention and allocate resources, it is essential to analyse the new subscription calls for any underlying patterns.

![Time Series Soothing using different Centred Moving Averages](https://github.com/niloy2974/Driving-Operational-Efficiency-with-Analytics-in-Broadcasting/blob/main/tables%20and%20visualisations/Time%20Series%20Soothing%20using%20different%20Centred%20Moving%20Averages.jpg)

The actual data exhibits a weekly repeating pattern with a notable spike at the end of May, indicating the presence of both trend and seasonality. To smooth the actual series and isolate the trend, various Centred Moving Average (CMA) methods were applied, including CMA-3, CMA-7, and CMA-14. CMA-3 proves too reactive, failing to eliminate seasonality, while CMA-14 overly smooths the data, diminishing the significance of the strong peak. In comparison, CMA-7 strikes an effective balance by mitigating the limitations of the other two methods and covers a single operational cycle, making it the most suitable option. The trendline reveals a peak in early June that establishes a new baseline and suggests a surge in demand likely driven by awareness campaigns.

![Detrended Time Series with the Seasonal Profile](https://github.com/niloy2974/Driving-Operational-Efficiency-with-Analytics-in-Broadcasting/blob/main/tables%20and%20visualisations/Detrended%20Time%20Series%20with%20the%20Seasonal%20Profile.jpg)

To further investigate the seasonality component, the series was decomposed using the Additive Method, as the amplitude of fluctuations remains relatively constant. After detrending, the data was plotted against respective weeks and days to identify any recurring patterns. The resulting line chart highlights that calls are consistently higher from Monday to Thursday, gradually declining on Friday and reaching their lowest point over the weekend. This pattern implies that consumers are more likely to reach out during weekdays, possibly due to the perception that DSB’s services may be unavailable during the weekend.

![Time Series Components](https://github.com/niloy2974/Driving-Operational-Efficiency-with-Analytics-in-Broadcasting/blob/main/tables%20and%20visualisations/Time%20Series%20Components.jpg)

Given these implications, DSB should allocate additional resources and maintain sufficient inventory to cater to the increased volume of calls during the weekdays. Furthermore, considering the significant rise in demand in early June, it is evident that consumer interest has reached a new level. To address both the identified seasonality and the upward trend in demand, DSB should implement a dynamic resource planning strategy that prioritises weekday operations and anticipates heightened activity during future campaign periods.
# Forecasting New Subscription Call Demand
Now that the trend and seasonality have been analysed, the next step is to forecast future calls so that DSB can make strategic operational decisions to meet upcoming demand. Several forecasting models were applied using data up to 18 June as the training set, with the remaining data reserved for testing.

![Seasonal Naive Forecasting Model and Average Index with Average Level Forecasting Model](https://github.com/niloy2974/Driving-Operational-Efficiency-with-Analytics-in-Broadcasting/blob/main/tables%20and%20visualisations/Seasonal%20Naive%20Forecasting%20Model%20and%20%20Average%20Index%20with%20Average%20Level%20Forecasting%20Model.jpg)

The Seasonal Naïve model was found to underfit the data, as it bases its forecast solely on the previous week’s values. This makes it slow to respond to fluctuations and unable to capture underlying trends. In comparison, the Seasonal Average model, which uses the Weekly Average Level and Average Index, performs slightly better by averaging seasonality across multiple periods. This approach allows for improved handling of constant seasonality. However, both models remain limited in their responsiveness to recent changes, reducing their overall effectiveness.

![Seasonal Exponential Smoothing Forecasting Model](https://github.com/niloy2974/Driving-Operational-Efficiency-with-Analytics-in-Broadcasting/blob/main/tables%20and%20visualisations/Seasonal%20Exponential%20Smoothing%20Forecasting%20Model.jpg)

The Seasonal Exponential Smoothing (SES) model offers a stronger alternative, as it effectively incorporates recent changes, making it well-suited for both seasonality and slight trends. Its flexibility lies in the ability to adjust the Level (0.30) and Season (0.35) indices, allowing emphasis on recent data and seasonality to be prioritised as needed. This adaptability makes the model more responsive to recent fluctuations.

![In-sample and Out-of-sample Error Comparison across different models](https://github.com/niloy2974/Driving-Operational-Efficiency-with-Analytics-in-Broadcasting/blob/main/tables%20and%20visualisations/In-sample%20and%20Out-of-sample%20Error%20Comparison%20across%20different%20models.jpg)

These models were evaluated based on forecasting errors, using metrics such as Mean Error (ME), Mean Absolute Error (MAE), and Root Mean Squared Error (RMSE). Each metric provides unique insights: ME indicates whether forecasts tend to over- or underestimate, although it may mask large individual errors due to offsetting positive and negative values; MAE treats all errors equally but overlooks the severity of larger deviations; RMSE, by contrast, places greater weight on large errors, making it ideal for identifying significant forecasting issues.

Based on the error comparison, the Season-Trend Exponential Smoothing ES model shows better performance in terms of in-sample Mean Error (ME), but it performs poorly in the out-of-sample forecast. This suggests that the model overpredicts, likely due to giving too much weight to the short-term trend observed in June, which was not sustained. Since this trend was not continuous, SES performs better in the out-of-sample forecast by ignoring the trend component.

While the Seasonal Averages model shows lower ME in the out-of-sample forecast, its high in-sample and MAE values suggest that errors in opposing directions are cancelling each other out, rather than indicating true accuracy. Overall, SES shows more balanced performance and performs better in out-of-sample forecasting, making it the more reliable model for future predictions.

![In-sample Error Comparison and Out-of-sample Error Comparison for Seasonal Naive, Average Index with Average Level and Seasonal Smoothing Forecasting Models](https://github.com/niloy2974/Driving-Operational-Efficiency-with-Analytics-in-Broadcasting/blob/main/tables%20and%20visualisations/In-sample%20Error%20Comparison%20and%20Out-of-sample%20Error%20Comparison%20for%20Seasonal%20Naive%2C%20Average%20Index%20with%20Average%20Level%20and%20Seasonal%20Smoothing%20Forecasting%20Models.jpg)

Upon comparing both in-sample and out-of-sample errors, the Seasonal Exponential Smoothing model emerged as the most effective, consistently yielding the lowest error values across all metrics and demonstrating higher accuracy for out-of-sample data. This highlights the model’s ability to improve as more data becomes available.
# New Subscription Installation: Inventory Simulation
DSB operates a Continuous Inventory System, where inventory levels are constantly monitored, and replenishment occurs once a specified threshold is reached. Two critical parameters in this system are the Reorder Point (ROP) and Reorder Quantity (ROQ). In the existing setup, ROP is set at 2 units, meaning a reorder is triggered when stock drops to 2 units, while ROQ is set at 10 units per order. Additional variables include the inventory level (initially 10 units), the demand, and a lead time of zero, indicating that replenishment arrives on the next working day. Apart from the operational variables, there are cost variables like ordering cost incurred during each order placement, holding cost incurred for storing inventory and backorder cost incurred due to understocking. 

This system would be well-suited for DSB as it allows reordering at any time, enabling the company to respond swiftly to shifts in customer demand and ensure higher customer satisfaction. It also provides greater control over inventory levels, helping to reduce holding costs. However, despite this flexibility, the system is limited in minimising ordering costs, as ordering in smaller quantities makes it challenging to benefit from bulk purchases or negotiate more favourable supplier contracts.

To assess cost implications and simulate real-world variability, a one-week simulation was conducted using a discrete random distribution for the number of installations and boxes. This approach allows the testing of different inventory scenarios and cost structures.

![Day-wise Inventory Levels and Cost Breakdowns for Current Inventory System (ROQ=10 units)](https://github.com/niloy2974/Driving-Operational-Efficiency-with-Analytics-in-Broadcasting/blob/main/tables%20and%20visualisations/Day-wise%20Inventory%20Levels%20and%20Cost%20Breakdowns%20for%20Current%20Inventory%20System%20(ROQ%3D10%20units).jpg)

From the simulation results, DSB had to reorder twice during the week due to stock reaching the ROP. However, despite reordering, understocking occurred in the last two days, leading to backorders. This indicates that either the ROP should be increased to trigger reorders earlier, or the ROQ should be raised to allow the business to operate longer without frequent reordering.

Cost breakdown revealed that holding costs were lower than ordering costs, suggesting that increasing the ROQ would reduce the frequency of orders and therefore lower overall ordering costs. Despite frequent reordering, backorder costs were still incurred, reinforcing the need to adjust ROP and ROQ for better cost optimisation. Managing other variables like safety stock and inventory capacity would also contribute to the cost reduction.

To optimise inventory costs, the Economic Order Quantity (EOQ) was calculated under the assumption that all variables remain constant, with a steady demand rate of 2 units per day, derived from the cumulative demand observed during the simulation week. Using the ordering and holding cost inputs, the EOQ model recommended a revised ROQ of 13 units, which showed a clear improvement over the existing setup.

![Day-wise Inventory Levels and Cost Breakdowns for EOQ-based Inventory System (ROQ=13 units)](https://github.com/niloy2974/Driving-Operational-Efficiency-with-Analytics-in-Broadcasting/blob/main/tables%20and%20visualisations/Day-wise%20Inventory%20Levels%20and%20Cost%20Breakdowns%20for%20EOQ-based%20Inventory%20System%20(ROQ%3D13%20units).jpg)

Upon implementing the updated ROQ of 13 units, DSB successfully maintained sufficient inventory levels throughout the week to meet daily demand. There were no understocking events, which meant the business avoided incurring any backorder costs. Additionally, the system required only two reorders during the week, and by the end of the period, there were still three units of stock remaining, indicating a buffer that supports operational continuity without excessive overstocking.

![Cost Breakdowns Comparison (left) and Ideal ROP for both ROQs (right)](https://github.com/niloy2974/Driving-Operational-Efficiency-with-Analytics-in-Broadcasting/blob/main/tables%20and%20visualisations/Cost%20Breakdowns%20Comparison%20and%20Ideal%20ROP%20for%20both%20ROQs.jpg)

A comparison between the original ROQ of 10 and the optimised ROQ of 13 showed a clear trade-off between holding and ordering costs. While holding costs slightly increased due to the higher inventory level, this was offset by reduced ordering frequency and the elimination of backorders. Overall, the increase in ROQ proved profitable by reducing total costs and improving service levels.

In addition to optimising ROQ, modifying the ROP can further enhance inventory performance. Although a lower ROP such as 0 may minimise holding costs, it significantly increases the risk of stockouts and customer dissatisfaction when ROQ is 10. In comparison, a ROP of 2 effectively prevented backorders under the new ROQ of 13. However, increasing the ROP beyond this point would lead to overstocking, raising holding costs unnecessarily. In conclusion, optimising key components like ROP and ROQ based on demand and cost trade-offs can help DSB minimise total costs.
# Engineer Job Allocation
To serve new subscriptions, DSB uses a greedy allocation method to assign engineers to jobs based on shortest travel distance. This approach aims to minimise the total distance covered, enabling faster service, and reducing operational costs. A sample scenario was tested where DSB expected to optimise the route, targeting a minimum total distance of 28 miles.

![Current Job Allocation Model](https://github.com/niloy2974/Driving-Operational-Efficiency-with-Analytics-in-Broadcasting/blob/main/tables%20and%20visualisations/Current%20Job%20Allocation%20Model.jpg)

At first glance, the solution may appear optimised, as each engineer is assigned to a single job and the closest distances are considered in most cases, except for Job 4. However, since engineers can be allocated to multiple jobs, a more efficient combination may exist those results in a total distance lower than 28 miles.

To determine the lowest possible distance within the given constraints, a linear programming model was developed. This approach successfully reduced the total distance to 27 miles. To understand this solution more clearly, it is important to explain the decision variables, objective function, and constraints.

In this model, the overall decision would be optimally allocating the five engineers to the five jobs in such a way that the total covered distance is minimised. The decision variable would be whether a specific engineer would be assigned to a specific job or not.

![Decision Variable Equation](https://github.com/niloy2974/Driving-Operational-Efficiency-with-Analytics-in-Broadcasting/blob/main/equations/Decision%20Variable%20Equation.jpg)

In Equation (1), 

a ∈ {1,2,3,4,5}, represents the engineers Tom, Bob, Paul, Steve, and Lucas respectively.

b ∈ {1,2,3,4,5}, represents the jobs Job 1, Job 2, Job 3, Job 4, and Job 5 respectively.

The objective function would be the total covered distance for all jobs to be minimised. To calculate the total distance the distances of the allocated jobs should be added together. To make it possible for considering all possible combinations all the job distances should be multiplied with the respective decision variable x_ab. Following objective function can be formed through this:

![Objective Function Equation](https://github.com/niloy2974/Driving-Operational-Efficiency-with-Analytics-in-Broadcasting/blob/main/equations/Objective%20Function%20Equation.jpg)

To shorten the Equation (2), summation notations for the variables can be used,

![Updated Objective Function Equation](https://github.com/niloy2974/Driving-Operational-Efficiency-with-Analytics-in-Broadcasting/blob/main/equations/Updated%20Objective%20Function%20Equation.jpg)

In Equation (3),

d_ab, represents the distance required for engineer 'a' to reach job 'b'.
There are specifically two constraints in this case. First constraint is for the engineers, stating that each engineer can be assigned to multiple jobs. So, the first constraint in equation:

![First Constraint Equation](https://github.com/niloy2974/Driving-Operational-Efficiency-with-Analytics-in-Broadcasting/blob/main/equations/First%20Constraint%20Equation.jpg)

The second constraint is for the jobs, stating that each job must be assigned to one engineer. So, the second constraint in equation:

![Second Constraint Equation](https://github.com/niloy2974/Driving-Operational-Efficiency-with-Analytics-in-Broadcasting/blob/main/equations/Second%20Constraint%20Equation.jpg)

Setting up the objective function and the constraints on excel solver, the following optimal solution was revealed:

![Objective, Variable Cells and Constraints of Solver Report, Job Allocation Comparison between the previous and new model](https://github.com/niloy2974/Driving-Operational-Efficiency-with-Analytics-in-Broadcasting/blob/main/tables%20and%20visualisations/Objective%2C%20Variable%20Cells%20and%20Constraints%20of%20Solver%20Report%2C%20Job%20Allocation%20Comparison%20between%20the%20previous%20and%20new%20model.jpg)

According to the solver report, the job allocation can be optimised with a minimum distance of 27 miles. The change is highlighted by comparing the final values with the original values. For the original solution, Bob was assigned to a job which was six miles away, but it was closer to Paul. So, in the new the model, it has been assigned to Paul, as an engineer can do multiple jobs if required.

The solution demonstrates that DSB can improve its job allocation by using linear programming models. However, whether to implement such an approach would also depend on the cost implications of each model. While this solution reduces the total distance, it also implies that Paul would need to handle multiple jobs, which could potentially increase resource costs. At the same time, Bob would remain idle, thereby lowering overall cost efficiency.
# Engineer Vehicle Servicing and Maintenance
To allocate engineers effectively, DSB operates a local service hub that requires efficient queue management to minimise service delays for customers. Optimising queue times not only helps address unexpected future demand and estimate initial costs but also demands a long-term commitment from DSB. Key performance indicators include total, average, and maximum wait and service times, as higher values in these metrics can lead to increased costs due to extended waiting space requirements and potential business losses. Prolonged wait times may also trigger varied customer reactions, resulting in dissatisfaction. These challenges can be mitigated by optimising resource allocation and improving service efficiency.

A queuing simulation was developed based on the multiple-channel, single-phase system used by DSB, where two mechanics perform a single type of task. A single-queue was maintained, operating under a first-in, first-out (FIFO) discipline. Customers were assumed to be patient, following a finite-source model due to the fixed number of vehicles. To simulate the process for 15 vehicles, a Poisson distribution (λ = 4) was used for arrival times, while service times followed an Exponential distribution (μ = 3).

![Wait Times and Mechanic-wise Idle Time Comparison](https://github.com/niloy2974/Driving-Operational-Efficiency-with-Analytics-in-Broadcasting/blob/main/tables%20and%20visualisations/Wait%20Times%20and%20Mechanic-wise%20Idle%20Time%20Comparison.jpg)

An overview of the timings shows that the total wait time was 7.45 minutes, which is equal to the maximum wait time, indicating that only one vehicle experienced a delay while the rest were served immediately. Comparing the performance of both mechanics, mechanic 2 appears more efficient, with a lower idle time of 92 minutes (45%). A deeper analysis using a Gantt chart would provide more detailed insights into the allocation and utilisation of service time.

![Gantt Chart based on the simulation output](https://github.com/niloy2974/Driving-Operational-Efficiency-with-Analytics-in-Broadcasting/blob/main/tables%20and%20visualisations/Gantt%20Chart%20based%20on%20the%20simulation%20output.jpg)

Upon closer inspection, only vehicle 7 experienced a wait time, caused by longer-than-expected servicing of vehicles 5 and 6. The relatively high individual idle times suggest that customer arrivals occurred at large intervals. This highlights an inverse relationship between idle time and wait time, where an increase in one typically leads to a decrease in the other, and vice versa.

The periods when both mechanics were idle highlight opportunities for DSB to improve operational efficiency. Overall idle time decreases as customer frequency increases, suggesting that either reducing resources or increasing customer intake could optimise utilisation. However, reducing the number of mechanics may risk losing customers, as there were instances when both mechanics were simultaneously occupied, indicating potential capacity constraints during peak periods.

To efficiently utilise DSB’s vehicle servicing system, a Demand Management Strategy can be adopted to control customer arrivals and align resources, accordingly, reducing idle time and improving cost-effectiveness. However, restricting arrivals may lead to customer dissatisfaction and unpredictable price fluctuations. This dissatisfaction could be mitigated by implementing scheduled appointments instead of walk-ins. Alternatively, DSB could focus on keeping customer intake, which may raise costs and wait times but would enhance overall work efficiency and resource utilisation.

DSB could also consider a Level Capacity Strategy, lowering service capacity to minimise excessive idle time. The current high idle time suggests that resources are set up to handle a higher-than-necessary demand level. Reducing mechanics to match a lower, more realistic demand may decrease idle time but could increase customer wait times. Ultimately, the choice depends on DSB’s prioritisation between cost efficiency and service responsiveness.

Other potential improvements include optimising resource utilisation by assigning alternative tasks during idle periods or reducing the number of service channels. Additionally, hiring part-time or subcontracted mechanics could help lower fixed resource costs, effectively reducing overall idle cost without sacrificing flexibility.
# Recommendations
Based on the analyses conducted across various operational areas of Delta Sky Broadcasting (DSB), several recommendations can be made to improve efficiency and support better decision-making. The identification of peak call times should guide shift scheduling and workforce management at the call centre to ensure adequate staffing during high-demand periods. Forecasting new subscriptions reveals future customer growth trends, allowing DSB to plan resource allocation and marketing strategies effectively. The inventory management simulation indicates that the current ordering policy may lead to overstocking or stockouts, so a more dynamic, data-driven reordering approach based on demand forecasting would help reduce costs and improve product availability. The linear programming model shows that engineer allocation can be optimised by basing it on workload forecasts, improving service coverage without increasing staffing costs. The queue simulation highlights that long wait times negatively impact customer satisfaction, so investing in additional service counters or digital self-service options during peak periods could enhance the service experience.

Implementing these approaches together would enable DSB to proactively manage demand fluctuations, allocate resources more efficiently, and improve customer satisfaction. Integrating these models into a centralised analytics dashboard would allow decision-makers to monitor key operational metrics in real time and adjust strategies dynamically. By embedding analytics into everyday operations, DSB can create a more responsive, efficient, and customer-focused organisation that is well-equipped to meet future challenges.
