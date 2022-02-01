# adaptive-detection-molecules-attacker
This project consisted of two problems: 
1. Network Intrusion Detection problem in which you will perform binary class classification and
predict whether there is an attack or not.
2. IOT Botnet Attack Detection problem in which you will perform multiclass classification and
predict is the case is normal or a specific type of attack. <br/>
**For both the problems, you will have 2 sources of data:**
- An Initial CSV file which you can use to train an initial model.
- A data stream (accessed from the Kafka Server) which will be used to evaluate the static
solution and the solution that adapts with time.
## 1. Binary problem
### 1.1. Algorithm’s description
after load dataset I applied simple preprocessing to drop NAN, Inf values and repair initial spaces in the column name.
#### 1.1.1. Static Part
In this problem I trayed 3 different algorithms with All default parameters and fixed “random state” and get the following result 

|Algorithms | Accuracy|
|--|--|
|Decision tree |  99.84% |
|Random forest|  99.76% |
| SVM | 93.03% |

#### 1.1.2. Adapted Part
In Adapted Part I used forgetting Strategies (fixed window method) with 20,000 row window size and get new 2,000 records using Kafka API and loop for fifty times. Each iteration adds the new records in the bottom of my DataFrame and deletes the same size for the top of DataFrame to be sure that after 10 iteration all old records were changed.
In this part I used two Models based on decision tree. <br/>
**“static_model”** that trained (fitted) on given dataset. and during iteration and sliding the fixed window predict each new data on it without fitting and append its value to list.<br/>
 **“Adapted_model”** that trained (fitted) on the fixed window data 20,000 and train (fit) and during iteration and sliding the fixed window predict each new data and append its value to list. and train on the new window. Finally, I got two prediction lists from the two models.
### 1.2. Algorithm evaluation
 from the two prediction lists that I mentioned before I applied “Accuracy”, “F1_Score” as evaluation metric and print Classification report for each iteration. As shown in fig. 1<br/>
 ![1 terminal output of iteration no. 46 for problem 1](https://github.com/eslamahmed235/adaptive-detection-molecules-attacker/blob/d7d708afaa1c789991b420c15023170098fc89f8/data/image.png)<br/>
 After get Accuracy and F1_Score and classification report I think best way to show F1_Score result to plot both line in same figure as shown in fig 2 <br/>
 ![-line chart between accuracy and no of iteration for problem 1](https://github.com/eslamahmed235/adaptive-detection-molecules-attacker/blob/4ae689ac8717528501eaae66a56e2fc6984631eb/data/problem%201%20result.png)


## 2. Multiclass Problem
### 2.1 Algorithm’s description
#### 2.1.1 Static Part
in this part I trayed 3 different algorithms with All default parameters and fixed “random state” and get the following result <br/>

|Algorithms | Accuracy|
|--|--|
|Decision tree | 99. 90 % |
|Random forest| 99.88 % |
| SVM | 87, 34 % |

#### 2.1.2 Adapted Part
In Adapted Part I used forgetting Strategies (fixed window method) with 20,000 row window size and get new 1,000 records using Kafka API and loop for 100 times. Each iteration adds the new records In the bottom of my DataFrame and delete the same size for the top of DataFrame to be sure that after 20 iteration all old records was changed.

### 2.2 Algorithm evaluation
from the two prediction lists that I mentioned before I applied “Accuracy”, “F1_Score” as evaluation metric and print Classification report for each iteration. As shown in fig. 3 <br/>
![Classification report for iteration no. 46](https://github.com/eslamahmed235/adaptive-detection-molecules-attacker/blob/main/data/problem%202%20calssification%20report.png) <br/>
After get Accuracy and F1_Score and classification report I think best way to show final result to plot both line in same figure as shown in fig 4 <br/>
![-line chart between accuracy and no of iteration for problem 2](https://github.com/eslamahmed235/adaptive-detection-molecules-attacker/blob/819e963bc089f82a53e103b1fa39c501625f66cd/data/problem%202%20result.png) <br/>
