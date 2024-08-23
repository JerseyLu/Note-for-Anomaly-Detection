# **Learning Notes: Urban Traffic Anomaly Detection**

This learning note focuses on the key methodologies, applications, and challenges in urban traffic anomaly detection, as surveyed in recent literature. The main objective is to understand the diverse approaches used for detecting anomalies in traffic flow and trajectory data in urban environments. Individuals who are interested in it are welcome to discuss and study together. PS: Only used for personal study!

---

## **1. Overview**
Urban traffic anomaly detection plays a crucial role in intelligent transportation systems (ITS), aiming to identify unusual patterns or events that deviate from normal traffic conditions[^1]. These anomalies can represent traffic congestion, accidents, unusual vehicle behavior, or special events.

### **1.1 Key Definitions**
- **Anomaly**: Any observation or pattern in the data that deviates significantly from the expected behavior.
- **Traffic Anomalies**: Irregular patterns in traffic flow or vehicle trajectories that are inconsistent with historical data or expected behavior.

### **1.2 Types of Anomalies**
1. **Point Anomalies**: Single data points that are significantly different from the rest of the data.
2. **Contextual Anomalies**: Data points that are anomalous in a particular context but appear normal in another.
3. **Collective Anomalies**: A collection of data points that deviate from expected behavior as a group, even if individual points appear normal.

![Figure 1: Taxonomy of urban traffic outlier detection algorithms](https://github.com/user-attachments/assets/0e300130-cae3-4d62-9d80-3dc847892f09)

Figure 1 outlines the taxonomy of the urban traffic outlier detection algorithms. They are separated into two categories. Flow outlier detection aims to identify outliers from urban flow data, including 1) statistical, 2) similarity, and 3) pattern mining methods. The second category is trajectory outlier detection where clustering and similarity approaches are used to derive outliers. This includes trajectory outliers using offline processing and sub-trajectory outliers using online processing.(Reference[^1] for the image source) 

---

## **2. Flow Outlier Detection**

Flow outlier detection focuses on identifying unusual patterns in traffic flow data, which may include changes in vehicle density, speed, or direction at specific locations or times.


![Figure 2: Flow Outlier Detection Framework](https://github.com/user-attachments/assets/7e62c562-0d75-45ca-bf24-678f84b6ddec)

Figure 2 systematically illustrates the process of identifying traffic anomalies in urban environments. It begins with Urban Sensing and Data Acquisition, where sensors and GPS devices capture traffic flow data, represented by a sensor icon. This data is then organized in the Data Collection phase, symbolized by a database icon, ensuring it's structured according to specific applications such as flow values. The third phase, Outlier Detection, uses statistical, similarity-based, or pattern mining methods to analyze the data for outliers, indicated by a magnifying glass icon. Finally, the Output phase, marked with a checkmark icon, categorizes the detected outliers into three types: single flow outliers, congested roads, and interactions between road outliers, each uniquely illustrated by corresponding icons.

### **2.1 Research Challenges**
- **High-dimensionality**: Traffic flow data is often large-scale and multidimensional, making it difficult to efficiently detect anomalies.
- **Spatial-temporal complexity**: Traffic data has strong dependencies across time and space, complicating the detection of dynamic anomalies.
- **Data noise**: Sensor failures or inaccuracies can introduce noise into the data, leading to false positives or missed anomalies.

### **2.2 Methods**

| **Method**           | **Research Object**     | **Authors**                  | **Model Name**                         | **Description**                                                                                                                                                        | **Strengths and Weaknesses**                                                                                                                                         |
|----------------------|-------------------------|------------------------------|----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Statistical**      | Multivariate            | Ngan et al. [22]              | Dirichlet Process Mixture Model (DPMM) | Traffic flow data is projected into a higher-dimensional space, reduced to 2D using PCA, and outliers are detected using Dirichlet process clustering.                    | Strengths: Handles high-dimensional data and clusters automatically. Weaknesses: Relies on PCA, which might lose some variance and might not capture complex patterns.|
| **Statistical**      | Univariate              | Lam et al. [23]               | Kernel Smoothing Naive Bayes (KSNB)    | Traffic data errors and abnormalities are detected by assuming inlier flow values follow a kernel smooth distribution.                                                   | Strengths: Automatically determines inliers. Weaknesses: Assumes normal distribution, which might not fit all real-world data.                                       |
| **Statistical**      | Multivariate            | Turochy and Smith [25]        | Multivariate Statistical Quality Control (MSQC) | Uses multiple traffic variables (speed, occupancy) and F-distribution to detect congestion outliers.                                                                 | Strengths: Considers multiple variables. Weaknesses: Might struggle with high variability in traffic data.                                                           |
| **Statistical**      | Multivariate            | Park et al. [27]              | Multiple Blocks on MSQC (MB-MSQC)      | Enhances MSQC by dividing traffic flow data into different time blocks and applying MSQC on each block separately.                                                      | Strengths: Addresses daily traffic pattern variability. Weaknesses: Requires correct block definitions for effective performance.                                    |
| **Similarity**       | Distance                | Dang et al. [28]              | kNN-based approach (kNN-F)             | Calculates distances between flow values and detects outliers by exceeding a threshold in kNN algorithm.                                                               | Strengths: Simple and interpretable. Weaknesses: Distance calculation might be computationally expensive with large datasets.                                         |
| **Similarity**       | Density                 | Tang and Ngan [29]            | Density-based Bounded LOF (BLOF)       | Calculates the local reachability density (lrd) for each flow and uses it to identify outliers based on a bounded LOF algorithm.                                        | Strengths: Adapted for density variations. Weaknesses: Sensitive to parameter settings (e.g., number of neighbors).                                                  |
| **Similarity**       | Distance                | Munoz-Organero et al. [30]    | Center of Sliding Window (CSW)         | Filters out outliers caused by random traffic conditions (like jams) by sliding window approach focusing on driving locations and conditions.                          | Strengths: Effective for filtering noise. Weaknesses: May miss rare but significant outliers outside sliding windows.                                                |
| **Similarity**       | Spatiotemporal          | Shi et al. [31]               | Dynamic Spatial and Temporal Flows (DSTF) | Uses spatiotemporal similarity between road segments to detect anomalies, considering direction and flow dynamics.                                                   | Strengths: Accounts for spatiotemporal data. Weaknesses: Computationally intensive for large road networks.                                                          |
| **Similarity**       | Univariate              | Cheng et al. [32]             | Self-Organizing Map for Road Flows (SOM-RF) | Clusters traffic flow time series data using SOM algorithm to detect outliers.                                                                                          | Strengths: Handles time series clustering. Weaknesses: SOM might require extensive tuning for optimal results.                                                       |
| **Pattern Mining**   | Causal Interaction      | Liu et al. [37]               | Causal Interaction in Outlier Flows (CIOF) | Discovers causal interactions among detected outliers in urban traffic flows by analyzing flow segments and distortions between them.                                  | Strengths: Identifies causal relationships. Weaknesses: High computational cost for large networks.                                                                  |
| **Pattern Mining**   | Spatiotemporal          | Pang et al. [38]              | Pattern Mining for Spatio-Temporal Outlier Detection (PM-STOD) | Identifies outlier regions by analyzing emerging and persistent patterns in spatiotemporal data using a grid-based approach.                                           | Strengths: Detects emerging and persistent outliers. Weaknesses: Grid-based methods might miss outliers at boundaries.                                                |
| **Pattern Mining**   | Congested pattern       | Chawla et al. [40]            | DM-TF                                 | Analyzes traffic between regions using pattern mining on segment-route matrices to detect anomalous behaviors.                                                         | Strengths: Focuses on regional patterns. Weaknesses: May overlook isolated segment anomalies.                                                                        |
| **Pattern Mining**   | Congested pattern       | Inoue et al. [42]             | FP-growth for Congested Patterns (FP-growth-CP) | Uses FP-growth algorithm to discover frequent patterns in congested traffic data by treating it as a transactional database.                                           | Strengths: Efficient in identifying congestion patterns. Weaknesses: Requires extensive data preprocessing to be effective.                                          |
| **Pattern Mining**   | Spatiotemporal          | Nguyen et al. [43]            | STCTree                                | Predicts frequent spatiotemporal congested sites and their causal relationships using a tree structure for traffic data streams.                                        | Strengths: Handles large datasets and streaming data. Weaknesses: Complexity in tree combination and pattern identification processes.                               |

---

## **3. Trajectory Outlier Detection**

Trajectory outlier detection identifies anomalies in the movement patterns of individual vehicles or groups of vehicles. It is often used in GPS-based systems, where vehicle trajectories are tracked over time.

### **3.1 Research Challenges**
- **High Dimensionality**: Trajectories are represented by sequences of spatial and temporal data points, which are high-dimensional and complex.
- **Complex Behaviors**: Vehicles exhibit diverse behaviors, making it difficult to define what constitutes abnormal behavior.
- **Contextual Information**: The same trajectory could be anomalous in one context (e.g., speed limit violations) but normal in another.

### **3.2 Commonly Used Methods**

| **Method**           | **Description**                                                   | **Applications**                                                     | **Strengths and Weaknesses**                                   |
|----------------------|-------------------------------------------------------------------|---------------------------------------------------------------------|----------------------------------------------------------------|
| **Clustering**        | Groups trajectories based on similarity to identify outliers.     | K-means and DBSCAN to identify unusual vehicle routes.               | Simple but sensitive to initial parameters.                    |
| **Density-based**     | Identifies outliers based on the density of trajectories.         | Local Outlier Factor (LOF) used to detect deviations in crowded areas.| Robust to noise but computationally expensive.                  |
| **Distance-based**    | Measures similarity between trajectories using distance metrics.  | Dynamic Time Warping (DTW) compares temporal sequences.               | Effective for temporal patterns but has high computational costs.|
| **Deep Learning**     | LSTM and RNNs model complex trajectory sequences.                 | Detects abnormal driving patterns or unexpected route changes.       | Powerful for sequence data but requires extensive training.     |

### **3.3 Applications**
- **Anomaly Detection in Autonomous Driving**: Detects abnormal driving behaviors such as sudden lane changes or unexpected stops.
- **Traffic Flow Analysis**: Identifies disruptions in traffic patterns caused by construction, accidents, or other anomalies.
- **Urban Planning**: Helps in understanding and managing urban mobility by analyzing common and rare driving patterns.

---

## **4. Research Challenges in Urban Traffic Anomaly Detection**
While many methods have been developed for anomaly detection in urban traffic, several challenges remain:
1. **Data Sparsity and Imbalance**: Anomalies are rare, and many datasets contain significantly more normal examples, leading to imbalanced training sets.
2. **Scalability**: As cities become increasingly connected, the volume of data to be processed grows exponentially, posing challenges for both storage and computation.
3. **Real-time Processing**: Many systems struggle to provide real-time anomaly detection due to high computational costs or slow data collection methods.
4. **Dynamic Environments**: Traffic patterns are constantly changing, and algorithms must be able to adapt to new normal behaviors as the environment evolves.
5. **Privacy Concerns**: With the increasing use of GPS data and cameras, privacy concerns regarding data collection and use are becoming more prominent.

---

### **5. Summary**
Urban traffic anomaly detection is a rapidly evolving field that leverages advanced machine learning, deep learning, and statistical techniques to identify unusual patterns in both traffic flow and vehicle trajectories. Despite significant progress, several challenges remain, including real-time detection, scalability, and handling of complex and dynamic data.

Researchers continue to develop more sophisticated algorithms that can adapt to the unique challenges of urban environments, aiming to improve urban mobility, safety, and efficiency.

---

## References

[^1]: Djenouri, Y., Belhadi, A., Lin, J. C.-W., Djenouri, D., & Cano, A. (2019). A Survey on Urban Traffic Anomalies Detection Algorithms. IEEE Access, 12192–12205. [https://doi.org/10.1109/access.2019.2893124](https://doi.org/10.1109/access.2019.2893124)
