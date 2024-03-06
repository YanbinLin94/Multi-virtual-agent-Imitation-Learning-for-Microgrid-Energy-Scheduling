# (PESGM 2024) Multi-Virtual-Agent Imitation Learning for Microgrid Energy Scheduling (MAIL)
This is an open code for our PESGM 2024 paper: 
[An Imitation Learning Method with Multi virtual Agents for Microgrid Energy Optimization under Interrupted Periods.pdf](https://github.com/YanbinLin94/Multi-virtual-agent-Imitation-Learning-for-Microgrid-Energy-Scheduling/files/14512270/An.Imitation.Learning.Method.with.Multi.virtual.Agents.for.Microgrid.Energy.Optimization.under.Interrupted.Periods.pdf)


## **Citation**
**If you find this research useful, please cite these papers when you use this code.**
```
@INPROCEEDINGS{Lin_MAIL,
  author={Lin, Yanbin and Ni, Zhen and Tang, Yufei},
  booktitle={2024 IEEE Power & Energy Society General Meeting (PESGM)}, 
  title={An Imitation Learning Method with Multi-Virtual Agents for Microgrid Energy Optimization under Interrupted Periods}, 
  year={2024},
  pages={1-5},
  keywords={Imitation learning, interrupted power supply, deep neural networks, machine learning, multi-virtual agents, and microgrid energy scheduling},
  doi={}}
```

```
@INPROCEEDINGS{10252933,
  author={Lin, Yanbin and Das, Avijit and Ni, Zhen},
  booktitle={2023 IEEE Power & Energy Society General Meeting (PESGM)}, 
  title={A Modified Maximum Entropy Inverse Reinforcement Learning Approach for Microgrid Energy Scheduling}, 
  year={2023},
  volume={},
  number={},
  pages={1-5},
  keywords={Learning systems;Q-learning;Processor scheduling;Microgrids;Feature extraction;Entropy;Scheduling;Distributed energy resources;reinforcement learning;maximum entropy inverse reinforcement learning;microgrid energy scheduling;and operation optimization},
  doi={10.1109/PESGM52003.2023.10252933}}
```

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

```Run MAIL/GM 2024_Matlab/mainDP_Interrupted.m```

Then you will get a **interrupt10-13_dp51.xls** file and some output figures. This is one expert demonstration generated by the Dynamic Programming method in the scenario that extreme weather-related events happen at time $10^{th}-13^{th}$ (total 4 hours).

<img src="https://github.com/YanbinLin94/Multi-virtual-agent-Imitation-Learning-for-Microgrid-Energy-Scheduling/assets/97860537/0fecf82d-a24e-4fe4-83ba-39a8898d6d29" width="600" height="400">
<br/><br/>
To implement our proposed MAIL method, you still need to generate other scenarios' expert demonstrations, since there will be several virtual agents to solve different cases in the MAIL method. 

For example, we consider three virtual environments that the power supply interruptions happen at the time-period $10^{th}-13^{th}$ hours, $11^{th}-14^{th}$ hours, and $12^{th}-15^{th}$ hours. Three local virtual agents are applied to imitate the corresponding virtual environment's actions. So you need to change **fault_time = [10 11 12 13]** to **fault_time = [11 12 13 14]**  in the **mainDP_Interrupted.m** file to generate **interrupt11-14_dp51.xls**, and change **fault_time = [10 11 12 13]** to **fault_time = [12 13 14 15]** to generate **interrupt12-15_dp51.xls**. 

These files are already included in the MAIL/GM 2024_Python file folder now.

## **Implementation of MAIL Method**
### Generation of Learner Policy and Results

Once you have the expert demonstrations for different cases, you can implement the MAIL method in the MAIL/GM 2024_Python file folder.

```Python3 main.py```

You will get the result of the MAIL method: policy10-15_max3_dp11.csv. This is a policy learned for the scenario that extreme weather-related events happen at time $10^{th}-15^{th}$ (total 6 hours), which is different from expert scenarios.

```Run the calculate_cost.py file```

You will get the cost of this learned policy and a "result_of_interruption10-15_max_dp51.csv" file including 7 rows of output results: Battery_soc, power from grid,	power to grid,	battery discharged power,	battery charged power,	and dg output.

### Generation of Figures

Copy "result_of_interruption10-15_max_dp51.csv" to MAIL/GM 2024_Matlab filefolder.

```Run the plot_result.m file```

You will get the output figure of the proposed MAIL method.

<img src="https://github.com/YanbinLin94/Multi-virtual-agent-Imitation-Learning-for-Microgrid-Energy-Scheduling/assets/97860537/ef855c07-7db0-4040-b697-fc4d2c7a38af" width="600" height="400">
<br/><br/>
You can also use plot_batt_pol.m and plot_cost.m files to compare the results of different methods: such as the Aggregated Q-learning method and DP method (These two methods' implementations are also included in our MAIL/GM 2024_Matlab filefolder: Cooperative_system.m and mainDP_Interrupted.m). If you want to compare different methods' results, you need to copy their result to comparative_batt_pol_soc51.csv and comparative_cost_soc51.csv before you run plot_batt_pol.m and plot_cost.m files (They are already included in the file folders now).

The figures are shown below.
<br/><br/>
<img src="https://github.com/YanbinLin94/Multi-virtual-agent-Imitation-Learning-for-Microgrid-Energy-Scheduling/assets/97860537/e5dc3739-8739-403d-b87e-29c29e7e24e5" width="600" height="400">
<br/><br/>
<img src="https://github.com/YanbinLin94/Multi-virtual-agent-Imitation-Learning-for-Microgrid-Energy-Scheduling/assets/97860537/42bc5a23-9c6c-4361-8ce3-6759c4c6a7f1" width="600" height="400">
<br/><br/>
<img src="https://github.com/YanbinLin94/Multi-virtual-agent-Imitation-Learning-for-Microgrid-Energy-Scheduling/assets/97860537/a4781326-95a5-4e54-ba07-7f148b7669aa" width="600" height="400">
<br/><br/>
## Case Studies
### Case 1: Change of the Discreted Size of SOC
Change the value of N in the MAIL/GM 2024_Matlab/mainDP_Interrupted.m to change the discrete size. For example, if you want to discrete the SOC to 11 states, change N=11-1=10; if you want to discrete the SOC to 1001 states, change N=1001-1=1000.

### Case 2: Change of the Interrupted Time Period
Change **fault_time = [10 11 12 13]** in the MAIL/GM 2024_Matlab/mainDP_Interrupted.m to any time slot you want to try. Remember, the current code only supports time slots equal to 4 hours. If you want to implement the scenario that the interrupted time slot doesn't equal 4 hours, such as [10 11 12 13 14 15] (6 hours), you need the sentence in the MAIL/GM 2024_Matlab/F_costval_DP_FLT.m and MAIL/GM 2024_Matlab/F_costval_RL_FLT.m files.
```
if time == fault_time(1) || time == fault_time(2) || time == fault_time(3) || time == fault_time(4)
    sig = 1;
```
to
```
if time == fault_time(1) || time == fault_time(2) || time == fault_time(3) || time == fault_time(4) || time == fault_time(5) || time == fault_time(6)
   sig = 1;
```

### Case 3: Change of the Loss Function in the MAIL Method
Three kinds of loss functions can be used in the MAIL method.
You can use either

```loss_value = (loss11+loss12+loss13+loss21+loss22+loss23+loss31+loss32+loss33)/9```

or

```loss_value = (loss1 + loss2 + loss3)/3```

or

```loss_value = (loss11 + loss22 + loss33)/3```

Currently, we use the second one.

The relationship of these losses is listed in the table below.
<br/><br/>
![Picture1](https://github.com/YanbinLin94/Multi-virtual-agent-Imitation-Learning-for-Microgrid-Energy-Scheduling/assets/97860537/ed3ab8b0-d6f1-48b5-ad9d-fc6fb3162554)












