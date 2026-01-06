# network-intrusion-detection-ml

<img width="600" height="400" alt="image" src="https://github.com/user-attachments/assets/1d5c8107-2551-4888-be80-9bc0c1b01b4b" />

**Overview**
Network Intrusion Detection Systems (IDS) are essential for monitoring network traffic and identifying malicious activities that threaten system security. Traditional IDS rely on predefined rules and signatures, which limits their ability to detect novel and evolving attacks. Machine Learning–based IDS address this challenge by learning patterns from network traffic data, enabling accurate detection of anomalies and attack types while improving adaptability and reducing false positives.

This repository presents an end-to-end Machine Learning–based Intrusion Detection System (IDS) designed to detect malicious network activities and classify different types of cyber attacks. The system leverages supervised learning techniques to analyze network traffic, perform feature selection, and build robust classification models for both binary (benign vs attack) and multi-class attack detection. By combining scalable ML pipelines, optimized feature engineering, and comprehensive evaluation metrics, this project demonstrates a practical and data-driven approach to modern network security.



**Dataset: CICIDS 2017**

For training and evaluating the Machine Learning models in this project, we use the CICIDS 2017 dataset.

The CICIDS 2017 dataset (Canadian Institute for Cybersecurity) is a realistic network traffic dataset that captures both benign and malicious activities over several days. The traffic reflects typical user behavior and includes a wide range of attack types such as Brute-Force, DoS & DDoS, Web Attacks, Botnets, and more. It contains flow-based features extracted from network traffic, making it ideal for machine learning–based intrusion detection research.

The dataset can be dowloaded via below link:
  https://www.unb.ca/cic/datasets/ids-2017.html


  **Data Preparation**
  The raw PCAP files from Monday to Friday are processed and aggregated into a single consolidated DataFrame, which serves as the base dataset for all subsequent data preprocessing steps. The resulting dataset contains 2,830,743         network flow records, indexed from 0 to 2,830,742, with a total of 79 feature columns representing flow-level network characteristics and traffic attributes.

  **Data cleaning**
  The aggregated dataset undergoes multiple data cleaning steps to ensure consistency and reliability for machine learning:
  Duplicate records are removed.
  Missing values are identified and handled appropriately.
  Infinite values are converted to NaN to maintain numerical stability.
  All NaN values are imputed using the median of their respective columns to minimize the impact of outliers.
  During this process, only the Flow Bytes/s and Flow Packets/s columns were found to contain 1,564 missing values (approximately 0.6% of the dataset). These values were successfully replaced with the median of each column. After        these steps, the dataset is clean and ready for feature engineering and model training.

  **Label Definition and Attack Categorization**
  The original CICIDS 2017 dataset contains a large number of fine-grained attack labels. To simplify the classification task and improve model interpretability, related attack labels are grouped into broader attack categories.
  A mapping dictionary is created to consolidate similar attack types under common labels. Finally just 9 attacks types are defined in our label column , attack type-->BENIGN, DoS, DDoS, Port Scan, Brute Force, Web Attack, Bot,          Infiltration, Heartbleed
  
**Data Preprocessing**
 The Attack Type column is label-encoded, converting categorical attack labels into numeric values, as machine learning models can only interpret numerical inputs.

**Feature Engineering and Reduction**
Zero-variance features are first removed, as they provide no useful information for modeling.
Highly correlated features are dropped to avoid redundancy, reducing the total number of columns from 47 to 23.
Next, XGBoost is used to evaluate feature importance and select the most relevant features for classification.
After these steps, the dataset is reduced to 24 informative features, a balanced number that helps the model learn effectively without being overwhelmed by too many variables, reducing the risk of overfitting or “hallucinations” caused by irrelevant or noisy features.

**Binary classification**-->  For binary classification, the dataset was divided into normal (BENIGN) vs attack (ATTACK) traffic. Two models were implemented to compare performance: Logistic Regression and Random Forest.
Random Forest significantly outperformed Logistic Regression in terms of accuracy, precision, and recall, particularly for the attack class, demonstrating its ability to capture complex patterns in network traffic.

Multiclass classification-
For multi-class classification, the goal was to classify traffic into one of the specific attack types. Both Logistic Regression and Random Forest were initially applied. However:
Logistic Regression was too simple to separate overlapping classes effectively.
Random Forest achieved high overall accuracy, but performance was dominated by the majority (BENIGN) class, leading to poor recall and precision for minority attack classes.

To address this, XGboost was implemented, resulting in a balanced accuracy of 0.9313, with significant improvement in recall and precision for the attack classes.
The confusion matrix is used to visualize the performance of each class. Finally, a feature importance bar chart generated using XGBoost highlights the most influential features contributing to the classification, providing insight into the key drivers of network traffic anomalies.
  

  
  

