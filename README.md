# FitnessData_statReport
##Statistician report on Fitness tracker data

**_Topic:_** Accounting analytics on trainings of groups based on fitness-bracelets results

**_Purpose:_** Creating an automatic PivotTable(Excel) report with connecting to remote server, pointing out the group with the best results within certain period of time

### Input data
Under consideration was taken a table with aggregated data of the following structure:

* sportsman ID;
* active mode;
* date;
* number of steps;
* distance travelled;
* number of floors have been climbed;
* number of calories;
* number of active clories burnt during trainings;
* training time;
* sportsman group;

The table is organised as an Excel sheet;

### Data analysis and working on missing values
#### Sport tracker operation

In regular activity mode the device monitors a number of steps completed (Steps), sleep time, walking upstairs (Floors). It is possible due to integral accelerometer and other algorythms developed by Garmin, which help to distinguish a step from just a pretend movement in the process of washing up. After the distance traveled (Distance) there is burnt calories information (Calories). The counting is completed due to optical cardial rythm detector. Moreover, we use complex approach of counting calories burnt a day including calories burnt in the process of metabolism, i.e. those which are needed to provide vital functions. In addition, the minutes of intensive activity are also taken into account (Active minutes). This index is computed using recommended number of minutes of 'moderate physical exercises' a week 5х30 minutes, i.e. 150 minutes. This index can be tuned manually.

Generally all the devices have only one mode during trainings: jogging. The following data to track trainings can be chosen: time, distance, calories (active calories), heart rhythm (heartbeats per minute) and training zone (determined by pulse). 

Fitness-bracelets results: 
* _«Steps»_ can be excluded from the control index, as it counts a number of steps per day, not per training;
* _«Distance»_, _«Steps»_, _«Floors»_ and _«Calories»_ provide results per day;

To choose the evaluated connection factor, the following work has been done:
* data was divided into groups as a basic factor and participants' activity;
* correlation tables have been formed to identify multiple relations of variables;

Data hasn't been checked to be normal, so to evaluate correlation between it we used nonparametric ranking criteria of Spearman. 

![the average for the group for the entire period](http://i9.pixs.ru/storage/5/4/9/1png_6938305_25292549.png)

![changes in average for dates](http://i12.pixs.ru/storage/7/8/4/2png_9555860_25292784.png)

![averages data for all group](http://i12.pixs.ru/storage/7/9/6/3png_5547286_25292796.png)

_«Active calories»_ gets data about burnt calories during training when a person activates the training
mode on the bracelet. This mode cannot be activated manually. All the data received from people who
activated the training mode when they really needed. «Active calories» data can be taken as a potential
control index of the most active sportsgroup.

_«Active minutes»_ will be excluded from the control, as this index can be activated manually by each
person.

**Conclusion:** Active calories data is used as an independant control missing values index of the group
training.

### Working with missing values
Input data has missing values in two options:
* all the numeric values are missing;
* only values for Floors are missing.

In the first case the rows are deleted, because it is supposed to use zero filling methods, average filling methods or interpolation method. When filled in with 0, we deteriorate team index what is unacceptable. When filled in with average values or interpolation method average team index does not change, i.e. When deleting rows, the result does not change.

In the second case we checked the affect of Floors on the active calories, because it is the index we use to choose the best group. We made the following steps:
1)	Chart of interrelation of active calories and steps of Floors index equals 0, it does not affectactive calories:  

![graph of active calories from steps for Floors = 0](http://i12.pixs.ru/storage/8/4/1/4png_8419439_25292841.png)

2) Making the same chart using input data:

![graph of active calories from steps for Floors = NA](http://i9.pixs.ru/storage/8/4/3/5png_1555149_25292843.png)

As we see, charts are almost the same, i.e. It means that no matter if Floors index values equal zero or filled with values, their correlation does not change.

3)	Determining Spearman correlation coefficient (_r_) for sample of the general population:

| Correlation method="spearman"     | Step        | Distance       | Floors | Calories | ActiveMinutes |
|:------:|:--------:|:--------:|:--------:|:--------:|:--------:|
| Active Calories  | 0,83      |0,84     | 0,38 | 0,88 | 0,6 |

We may conclude that active calories are interrelated with number of steps more than with otherfactors. And value r for Floors to the active calories equals 0.38.

Multiple regression equation **ActiveCalories ~ Steps +Floors**, for the following analytical evaluation affect on Floors values.

#### Calculation of active calories based on regression equation

![table of sample](http://i12.pixs.ru/storage/9/2/1/6png_2997314_25292921.png)

1) Regression equation evaluation.

Matrix X with variables Xj is added to one column with the values 1:

![table of steps and floors](http://i12.pixs.ru/storage/9/4/3/7png_8606358_25292943.png)

Matrix Y is considered to be _«Active Calories»_ column.
2) Matrix X is transposed to Xt
3) Matrixes (Xt, X) are multiplied
    
    Xt * X

| 40     | 354608        | 101       |
|:------:|:--------:|:--------:|
| 354608  | 3616518878      |968903     |
| 101   | 968903   |433    |

4) Matrixes (Xt, Y) are multiplied

    Xt * Y

| 40     | 
|:------:|
| 354608  | 
| 101   |

5) Inverse matrix (XtX)-1 is obtained

    Xt * X - 1

| 0,199     | -1,80E-05        | -0,00688       |
|:------:|:--------:|:--------:|
| -1,80E-05  | 0      |-1,00E-06     |
| -0,00688   | -1,00E-06   |0,006    |

6) Vector regression coefficient is defined:

![table of regression](http://i12.pixs.ru/storage/9/9/0/8png_7355463_25292990.png)

Regression equation (regression equation evaluation):			

    Y(ActiveCalories) = 164,114 + 0.106 X1(Steps) + 10.042 X2(Floors)

![shedule active ca;ories_1](http://i12.pixs.ru/storage/3/6/7/9png_9853753_25297367.png)

![shedule active ca;ories_2](http://i12.pixs.ru/storage/4/0/5/10png_4076429_25297405.png)

Plots shows that at NA values of Floors the coefficients of regression equation don't change significantly. Thus, we may conclude that missing values can be omitted and equated to 0.

### Analytics

Regression equation for _ActiveCalories ~ Step_ looks like:

    Y(ActiveCalories) = 175,629 + 0.108 X(Steps)

Regression equation for ActiveCalories ~ Steps & Floors looks like:

    Y(ActiveCalories) = 164,114 + 0.106 X1(Steps) + 10.042 X2(Floors)

Calculation table based on our data sample is composed:

| Inintial ActivityCalories    | According to steam regression equation        | According to multiple regression equation |
|:------:|:--------:|:--------:|
| 692 |	847 |	826 |
| 732 |	879 |	908 |
| 810 |	891 |	920 |
| 822 |	949 |	967 |
| 853 |	967 |	995 |
| 855 |	976 |	953 |
| 864 |	999 |	1016 |
| 780 |	1153 |	1158 |
| 133 |	282 |	279 |
| 473 |	490 |	474 |
| 595 |	526 |	519 |
| 588 |	649 |	641 |
| 713 |	652 |	634 |
| 766 |	728 |	708 |
| 696 |	771 |	751 |
| 807 |	781 |	801 |
| 942 |	903 |	901 |
| 1187 |	987 |	1013 |
| 1424 |	1061 |	1087 |
| 1236 |	1073 |	1059 |
| 1081 |	1149 |	1133 |
| 1271 |	1259 |	1231 |
| 1965 |	1263 |	1276 |
| 1350 |	1398 |	1389 |
| 1592 |	1567 |	1575 |
| 1565 |	1807 |	1792 |
| 1644 |	1847 |	1832 |
| 1180 |	1151 |	1156 |
| 1085 |	1188 |	1172 |
| 1487 |	1281 |	1284 |
| 1465 |	1296 |	1298 |
| 1300 |	1309 |	1321 |
| 1416 |	1342 |	1324 |
| 1387 |	1378 |	1359 |
| 1620 |	1401 |	1402 |
| 1524 |	1445 |	1445 |
| 1446 |	1517 |	1587 |
| 1528 |	1523 | 	1522 |
| 1500 |	1633 |	1651 |
| 1807 |	1861 |	1825 |

Variances of samples are obtained from steam regression and multiple regression equation and equal 140781.6 and 140343.1 accordingly, sd – 374.62 and 374.21, mean value – 1129.475 and 1129.6. Spearman rank correlation coefficient r = 0,998.

When using two-sample Kolmogorov-Smirnov test p-value=1 > 0.05, null hypothesis is rejected and we conclude that two sampes is drawn by different empirical distribution functions.

| X1i    | X2i      | ni     | F * (X1i) | F * (X2i) |
|:------:|:--------:|:--------:|:--------:|:--------:|
| 282 | 279 | 1 | 0,00 | 0,00 |
| 490 | 474 | 1 | 0,03 | 0,025 |
| 526 | 519 | 1 | 0,05 | 0,05 |
| 649 | 634 | 1 | 0,08 | 0,075 |
| 652 | 641 | 1 | 0,10 | 0,1 |
| 728 | 708 | 1 | 0,13 | 0,125 |
| 771 | 751 | 1 | 0,15 | 0,15 |
| 781 | 801 | 1 | 0,18 | 0,175 |
| 847 | 826 | 1 | 0,20 | 0,2 |
| 879 | 901 | 1 | 0,23 | 0,225 |
| 891 | 908 | 1 | 0,25 | 0,25 |
| 903 | 920 | 1 | 0,28 | 0,275 |
| 949 | 953 | 1 | 0,30 | 0,3 |
| 967 | 967 | 1 | 0,33 | 0,325 |
| 976 | 995 | 1 | 0,35 | 0,35 |
| 987 | 1013 | 1 | 0,38 | 0,375 |
| 999 | 1016 | 1 | 0,40 | 0,4 |
| 1061 | 1059 | 1 | 0,43 | 0,425 |
| 1073 | 1087 | 1 | 0,45 | 0,45 |
| 1149 | 1133 | 1 | 0,48 | 0,475 |
| 1151 | 1156 | 1 | 0,50 | 0,5 |
| 1153 | 1158 | 1 | 0,53 | 0,525 |
| 1188 | 1172 | 1 | 0,55 | 0,55 |
| 1259 | 1231 | 1 | 0,58 | 0,575 |
| 1263 | 1276 | 1 | 0,60 | 0,6 |
| 1281 | 1284 | 1 | 0,63 | 0,625 |
| 1296 | 1298 | 1 | 0,65 | 0,65 |
| 1309 | 1321 | 1 | 0,68 | 0,675 |
| 1342 | 1324 | 1 | 0,70 | 0,7 |
| 1378 | 1359 | 1 | 0,73 | 0,725 |
| 1398 | 1389 | 1 | 0,75 | 0,75 |
| 1401 | 1402 | 1 | 0,78 | 0,775 |
| 1445 | 1445 | 1 | 0,80 | 0,8 |
| 1517 | 1522 | 1 | 0,83 | 0,825 |
| 1523 | 1575 | 1 | 0,85 | 0,85 |
| 1567 | 1587 | 1 | 0,88 | 0,875 |
| 1633 | 1651 | 1 | 0,90 | 0,9 |
| 1807 | 1792 | 1 | 0,93 | 0,925 |
| 1847 | 1825 | 1 | 0,95 | 0,95 |
| 1861 | 1832 | 1 | 0,98 | 0,975 |

**Conclusion:** according to this data, Floors values don't affect on ActiveCalories values , that is why we can fill missing Floors values with 0 values.

### Client
Client side - PivotTable (Excel)

Development language: VBA

Implementation: Macro

Macro functionality:
- Creating remote server connection to the Database with ODBC connection
- Creating SQL request to get sample without missing values
- Creating summary table based on sample
- Descending sorting by amount of active caories burnt within 3 weeks
- Creating Barchart of active calories burnt by each group.
- Working with filters by «Date» and «User ID»
- Showing the result of the best group
- Providing data protection

### Server
ODBC connection is used for hardware server which works in Linux system.

Query processing is conducted by phpMyAdmin, which allows to transform our SQL BD.

Number of simultaneous connections - **100**

The customer gets an account to work (add new dat to tables, create additional databases etc.) To authorize account the client receives login and password.

Outcome of the programm:

![view of programm](http://i12.pixs.ru/storage/0/7/4/11png_9857456_25293074.png)
