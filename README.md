# Multi-virtual-agent-Imitation-Learning-for-Microgrid-Energy-Scheduling (MAIL)
This is an open-code for our PESGM 2024 paper: An Imitation Learning Method with Multi-Virtual Agents for Microgrid Energy Optimization under Interrupted Periods.

## **Set Up**

**This code requires:**

•	MATLAB

•	Python

•	sklearn/tensorflow/keras/numpy/xlrd/pandas libraries

## **Data Preparation**

To investigate the impact of power supply interruptions on the microgrid operations, **we assume during extreme weather-related events, the utility grid goes down for a certain period, and the microgrid operates in an isolated mode during this period**. The **RG outputs** are taken from the system advisory model by **the National Renewable Energy Laboratory for the city of Phoenix, AZ (https://sam.nrel.gov/)**. For the load-demand, **a small residential community load-demand data** is collected from **https://openei.org/wiki/Data**.

### Example of Data Generation

Here is an example to generate data for the latter implementation of our proposed **MAIL** method.

Assume we discrete the SOC state into 51 sizes, and the interrupted time is from $10^{th}$ to $13^{th}$ hour (T=24, a day for 24 hours).

```Run the MATLAB code in the file MAIL/GM 2024_Matlab/mainDP_Interrupted.m```

Then you will get a **interrupt10-13_dp51.xls** file and some output figures. This is one expert demonstration generated by the Dynamic Programming method in the scenario that extreme weather-related events happen at time $10^{th}-13^{th}$ (totally 4 hours).

![output](https://github.com/YanbinLin94/Multi-virtual-agent-Imitation-Learning-for-Microgrid-Energy-Scheduling/assets/97860537/0fecf82d-a24e-4fe4-83ba-39a8898d6d29)

To implement our proposed MAIL method, you still need to generate other scenarios' expert demonstrations, since there will be several virtual agents to solve different cases in MAIL method. 

For example, we consider three virtual environments that the power supply interruptions happen at the time-period $10^{th}-13^{th}$ hours, $11^{th}-14^{th}$ hours, and $12^{th}-15^{th}$ hours. Three local virtual agents are applied to imitate the corresponding virtual environment's actions. So you need to change **fault_time = [10 11 12 13]** to **fault_time = [11 12 13 14]**  in the **mainDP_Interrupted.m** file to generate **interrupt11-14_dp51.xls**, and change **fault_time = [10 11 12 13]** to **fault_time = [12 13 14 15]** to generate **interrupt12-15_dp51.xls**. 

These files are already included in the MAIL/GM 2024_Python file now.

## **Implematation of MAIL Method**
### Generate Learner Policy and Output Results

Once you have the expert demonstrations for different cases, you can start to implement the MAIL method in the MAIL/GM 2024_Python file.

```Run the main.py file```

You will get the result of MAIL method: policy10-15_max3_dp11.csv. This is a policy learn for the scenario that extreme weather-related events happen at time $10^{th}-15^{th}$ (totally 6 hours), which are different from expert scenarios.

```Run the calculate_cost.py file```

You will get the cost of this learned policy and a "result_of_interruption10-15_max_dp51.csv" file including 7 rows of  output results: Battery_soc, power from grid,	power to grid,	battery discharged power,	battery charged power,	and dg output.

### Plot Figures

Copy "result_of_interruption10-15_max_dp51.csv" to MAIL/GM 2024_Matlab file.

```Run the plot_result.m file```

You will get the output figure of the proposed MAIL method.

![result](https://github.com/YanbinLin94/Multi-virtual-agent-Imitation-Learning-for-Microgrid-Energy-Scheduling/assets/97860537/ef855c07-7db0-4040-b697-fc4d2c7a38af)





