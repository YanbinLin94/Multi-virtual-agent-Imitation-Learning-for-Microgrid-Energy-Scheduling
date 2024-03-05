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

Assume we discrete the SOC state into 51 sizes, and the interrupted time is from 10 to 13 (T=24, a day for 24 hours).

```Run the MATLAB code in the file MAIL/GM 2024_Matlab/GM 2024_Matlab.m```

Then you will get a **interrupt10-13_dp51.xls** file and some output figures. This is one expert demonstration generated by the Dynamic Programming method in the scenario of extreme weather-related events happen at time 10-13 (totally 4 hours).

![output](https://github.com/YanbinLin94/Multi-virtual-agent-Imitation-Learning-for-Microgrid-Energy-Scheduling/assets/97860537/0fecf82d-a24e-4fe4-83ba-39a8898d6d29)

To implement our proposed MAIL method, you still need to generate other scenarios' expert demonstrations, since there will be several virtual agents to solve different cases in MAIL method. For example, we consider three virtual environments that the power supply interruptions happen at the time-period $10^{th}$-$13^{th}$ hours, $11^{th}$-$14^{th}$ hours, and $12^{th}$-$15^{th}$ hours. Three local virtual agents are applied to imitate the corresponding virtual environment's actions.
