# **Learning Notes: Urban Traffic Anomaly Detection**

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

Figure 2 systematically illustrates the process of identifying traffic anomalies in urban environments. It begins with Urban Sensing and Data Acquisition, where sensors and GPS devices capture traffic flow data, represented by a sensor icon. This data is then organized in the Data Collection phase, symbolized by a database icon, ensuring it's structured according to specific applications such as flow values. The third phase, Outlier Detection, uses statistical, similarity-based, or pattern-mining methods to analyze the data for outliers, indicated by a magnifying glass icon. Finally, the Output phase, marked with a checkmark icon, categorizes the detected outliers into three types: single-flow outliers, congested roads, and interactions between road outliers, each uniquely illustrated by corresponding icons.

### **2.1 Research Challenges**
- **High-dimensionality**: Traffic flow data is often large-scale and multidimensional, making it difficult to efficiently detect anomalies.
- **Spatial-temporal complexity**: Traffic data has strong dependencies across time and space, complicating the detection of dynamic anomalies.
- **Data noise**: Sensor failures or inaccuracies can introduce noise into the data, leading to false positives or missed anomalies.

### **2.2 Methods**

| **Method**           | **Research Object**     | **Authors**                  | **Model Name**                         | **Description**                                                                                                                                                        | **Strengths and Weaknesses**                                                                                                                                         |
|----------------------|-------------------------|------------------------------|----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Statistical**      | Multivariate            | Ngan et al. [^2]              | Dirichlet Process Mixture Model (DPMM) | Traffic flow data is projected into a higher-dimensional space, reduced to 2D using PCA, and outliers are detected using Dirichlet process clustering.                    | Strengths: Handles high-dimensional data and clusters automatically. Weaknesses: Relies on PCA, which might lose some variance and might not capture complex patterns.|
| **Statistical**      | Univariate              | Lam et al. [^3]               | Kernel Smoothing Naive Bayes (KSNB)    | Traffic data errors and abnormalities are detected by assuming inlier flow values follow a kernel smooth distribution.                                                   | Strengths: Automatically determines inliers. Weaknesses: Assumes normal distribution, which might not fit all real-world data.                                       |
| **Statistical**      | Multivariate            | Turochy and Smith [^4]        | Multivariate Statistical Quality Control (MSQC) | Uses multiple traffic variables (speed, occupancy) and F-distribution to detect congestion outliers.                                                                 | Strengths: Considers multiple variables. Weaknesses: Might struggle with high variability in traffic data.                                                           |
| **Statistical**      | Multivariate            | Park et al. [^5]              | Multiple Blocks on MSQC (MB-MSQC)      | Enhances MSQC by dividing traffic flow data into different time blocks and applying MSQC on each block separately.                                                      | Strengths: Addresses daily traffic pattern variability. Weaknesses: Requires correct block definitions for effective performance.                                    |
| **Similarity**       | Distance                | Dang et al. [^6]              | kNN-based approach (kNN-F)             | Calculates distances between flow values and detects outliers by exceeding a threshold in kNN algorithm.                                                               | Strengths: Simple and interpretable. Weaknesses: Distance calculation might be computationally expensive with large datasets.                                         |
| **Similarity**       | Density                 | Tang and Ngan [^7]            | Density-based Bounded LOF (BLOF)       | Calculates the local reachability density (lrd) for each flow and uses it to identify outliers based on a bounded LOF algorithm.                                        | Strengths: Adapted for density variations. Weaknesses: Sensitive to parameter settings (e.g., number of neighbors).                                                  |
| **Similarity**       | Distance                | Munoz-Organero et al. [^8]    | Center of Sliding Window (CSW)         | Filters out outliers caused by random traffic conditions (like jams) by sliding window approach focusing on driving locations and conditions.                          | Strengths: Effective for filtering noise. Weaknesses: May miss rare but significant outliers outside sliding windows.                                                |
| **Similarity**       | Spatiotemporal          | Shi et al. [^9]               | Dynamic Spatial and Temporal Flows (DSTF) | Uses spatiotemporal similarity between road segments to detect anomalies, considering direction and flow dynamics.                                                   | Strengths: Accounts for spatiotemporal data. Weaknesses: Computationally intensive for large road networks.                                                          |
| **Similarity**       | Univariate              | Cheng et al. [^10]             | Self-Organizing Map for Road Flows (SOM-RF) | Clusters traffic flow time series data using SOM algorithm to detect outliers.                                                                                          | Strengths: Handles time series clustering. Weaknesses: SOM might require extensive tuning for optimal results.                                                       |
| **Pattern Mining**   | Causal Interaction      | Liu et al. [^11]               | Causal Interaction in Outlier Flows (CIOF) | Discovers causal interactions among detected outliers in urban traffic flows by analyzing flow segments and distortions between them.                                  | Strengths: Identifies causal relationships. Weaknesses: High computational cost for large networks.                                                                  |
| **Pattern Mining**   | Spatiotemporal          | Pang et al. [^12]              | Pattern Mining for Spatio-Temporal Outlier Detection (PM-STOD) | Identifies outlier regions by analyzing emerging and persistent patterns in spatiotemporal data using a grid-based approach.                                           | Strengths: Detects emerging and persistent outliers. Weaknesses: Grid-based methods might miss outliers at boundaries.                                                |
| **Pattern Mining**   | Congested pattern       | Chawla et al. [^13]            | DM-TF                                 | Analyzes traffic between regions using pattern mining on segment-route matrices to detect anomalous behaviors.                                                         | Strengths: Focuses on regional patterns. Weaknesses: May overlook isolated segment anomalies.                                                                        |
| **Pattern Mining**   | Congested pattern       | Inoue et al. [^14]             | FP-growth for Congested Patterns (FP-growth-CP) | Uses FP-growth algorithm to discover frequent patterns in congested traffic data by treating it as a transactional database.                                           | Strengths: Efficient in identifying congestion patterns. Weaknesses: Requires extensive data preprocessing to be effective.                                          |
| **Pattern Mining**   | Spatiotemporal          | Nguyen et al. [^15]            | STCTree                                | Predicts frequent spatiotemporal congested sites and their causal relationships using a tree structure for traffic data streams.                                        | Strengths: Handles large datasets and streaming data. Weaknesses: Complexity in tree combination and pattern identification processes.                               |

### ** 2.3 Flow Outlier Detection Algorithms: Detailed Explanation

![Figure 3: Flow outlier detection illustration. (a) kNN-F illustration. (b) CI-OF illustration.](https://github.com/user-attachments/assets/849ee38b-a562-4cb5-b09e-9473f189fe5a)

Figure 3 illustrates two algorithms for flow outlier detection in traffic data: **kNN-F** (K-Nearest Neighbor for Flow) and **CI-OF** (Complex Interaction Outlier Framework). These methods focus on identifying anomalies in traffic flows, either in single flow patterns or through interactions between different road segments.

#### (a) **kNN-F Algorithm for Single-Flow Outliers**

The upper part of the diagram (labeled as (a)) demonstrates the kNN-F algorithm, which is designed to detect single-flow outliers based on nearest neighbor calculations.

1. **Flow Data Input**:  
   The algorithm begins with a sequence of flow data over time, represented as \( f_1, f_2, ..., f_{10} \). Each flow instance corresponds to a specific time and a recorded flow value, such as 2, 25, 28, etc.

2. **Nearest Neighbor Search**:  
   For each flow \( f_i \), the algorithm calculates the nearest neighbors in the flow sequence using a predefined distance metric (e.g., Euclidean distance). In the diagram, the nearest neighbors for each flow are shown, such as \( f_1 \) having neighbors \( f_6 \) and \( f_2 \), and so on.

3. **Distance Calculation**:  
   After identifying the nearest neighbors, the algorithm calculates the distance between the current flow and its nearest neighbors. This distance is crucial for determining how different or anomalous a flow might be from its surrounding flows. For example, the distance between \( f_1 \) and its neighbors is calculated to be 23, while for \( f_2 \) it is 3.

4. **Outlier Detection**:  
   The outlier detection step involves applying a threshold value (denoted as \( \epsilon \)) to the computed distances. If the distance exceeds the threshold, the flow is classified as an outlier. In this case, \( \epsilon \) is set to 5, and any flow with a distance greater than 5 is marked as an outlier. For example, flows \( f_1 \) and \( f_7 \) are detected as outliers due to their distances (23 and 70, respectively) exceeding the threshold.

This algorithm effectively detects **single-flow outliers** by analyzing the distance between individual flow values and their closest neighbors, flagging those flows that deviate significantly from typical patterns.

#### (b) **CI-OF Algorithm for Complex Interaction Outliers**

The lower part of the diagram (labeled as (b)) presents the CI-OF algorithm, which detects outliers by considering interactions between different road segments. This method focuses on identifying **complex interaction outliers**, where unusual patterns arise from the relationships between flows across multiple segments.

1. **Flow Graph Construction**:  
   For each time window (denoted as \( W_1, W_2, W_3 \)), a directed flow graph is constructed. In these graphs, nodes represent road segments, and directed edges represent the flow between segments. The edge weights indicate the magnitude of flow between two segments. For example, the flow from segment \( a \) to \( b \) is represented with a weight of 2, indicating the flow volume.

2. **Segment Outlier Detection**:  
   Using these graphs, the algorithm identifies **segment outliers**, which are road segments exhibiting abnormal flow patterns. These segments deviate from expected behavior based on the relationships between connected segments. For instance, segments \( a \rightarrow b \) and \( b \rightarrow g \) might show unexpected flow behavior, potentially indicating abnormal traffic conditions.

3. **Transactional Representation**:  
   The flow interactions are transformed into a **transactional form**, where the sequence of flows between segments is represented as transactions. This transformation allows the algorithm to apply frequent pattern mining techniques to identify abnormal sequences. The diagram shows the process of converting segment interactions into tree structures, such as \( Tree_1 \) and \( Tree_2 \), which represent different flow sequences.

4. **Frequent Segment Outliers**:  
   Finally, the algorithm detects **frequent segment outliers**—those flow sequences that exhibit abnormal patterns across multiple time windows. These recurring patterns of unusual interactions between road segments are flagged as outliers. For example, the sequence \( a \rightarrow b \rightarrow d \) might be identified as a frequent segment outlier due to its repeated occurrence with abnormal flow behavior.

The CI-OF algorithm captures **complex interactions** between road segments, allowing for the detection of outliers not only in individual flows but also in the relationships between different segments. This approach is particularly effective for identifying abnormal patterns in traffic networks where interactions between segments are key to understanding outliers.

### Summary

In summary, the diagram provides a detailed view of two flow outlier detection algorithms:

- **kNN-F Algorithm**: Detects **single-flow outliers** by comparing individual flow values to their nearest neighbors, flagging those that significantly deviate from the norm based on a predefined distance threshold.
- **CI-OF Algorithm**: Detects **complex interaction outliers** by analyzing interactions between road segments, transforming these interactions into transactional form, and identifying recurring abnormal flow patterns across multiple time windows.

These algorithms are highly applicable in traffic management and urban planning, enabling the early detection of anomalies that may indicate traffic congestion, road blockages, or other disruptions in traffic flow.

---

## **3. Trajectory Outlier Detection**

Trajectory outlier detection identifies anomalies in the movement patterns of individual vehicles or groups of vehicles. It is often used in GPS-based systems, where vehicle trajectories are tracked over time.

![Figure 4: Trajectory outlier detection framework.](https://github.com/user-attachments/assets/a4fb283a-7ca6-40f9-942f-121cd0baf8d8)

Figure 4 presents the overall framework of the existing trajectory outlier detection algorithms. Generally, the trajectory outlier detection algorithms are decomposed into three main steps: i) Preprocessing: Preprocessing is aimed at collecting the trajectories database and information related to the road network of the urban city. The mapping function allows the generation of the mapped trajectory database. ii) Outlier detection: The mapped trajectory database is entered to the outlier detection algorithms, including clustering, density, and distance approaches, to find trajectory outliers. iii) Output visualization: After finding outliers, visualizing tools are needed to show the trajectory outliers to the user. In this context, two kinds of outliers are discovered. Only the trajectory outliers could be detected by applying offline processing. However, by applying online processing, the sub-trajectory that caused outlierness may be identified. 


### **3.1 Research Challenges**
- **High Dimensionality**: Trajectories are represented by sequences of spatial and temporal data points, which are high-dimensional and complex.
- **Complex Behaviors**: Vehicles exhibit diverse behaviors, making it difficult to define what constitutes abnormal behavior.
- **Contextual Information**: The same trajectory could be anomalous in one context (e.g., speed limit violations) but normal in another.

### **3.2 Commonly Used Methods**

| Method             | Outlier Detection Algorithms  | Authors                  | Model Name     | Description                                                                                  | Strengths and Weaknesses                                               |
| ------------------ | ----------------------------- | ------------------------ | -------------- | -------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| **OFFLINE PROCESSING** | **Clustering**             | Zhang [^16]              | MoNavTT        | Detects two-level outliers in taxi trips using NAVTEQ street map and MoNav algorithm.        | Strength: Effective in large networks; Weakness: High computational cost. |
|                    | **Clustering**                | Kumar et al. [^17]        | ClustiVAT      | An improved trajectory clustering algorithm using iVAT for identifying trajectory outliers.  | Strength: Improved accuracy in clustering; Weakness: Sensitive to noise.  |
|                    | **Density**                   | Zhang et al. [^18]        | iBAT           | An isolation-based anomaly trajectory detection algorithm using the "few and different" property. | Strength: High detection rate; Weakness: Limited scalability.            |
|                    | **Clustering**                | Lv et al. [^19]           | PBOTD          | Prototype-based outlier detection using medoids to identify anomalous taxi trajectories.     | Strength: Accurate prototype representation; Weakness: High memory usage.|
|                    | **Distance**                  | Zhou et al. [^20]         | OTD-UTF        | Identifies outliers in taxi fraud detection by matching trajectory and meter databases.       | Strength: Practical for real-world fraud detection; Weakness: Requires detailed data. |
|                    | **Density**                   | Kong et al. [^21]         | LoTAD          | Long-term traffic anomaly detection including TS-segments creation and anomaly index calculation. | Strength: Long-term trend analysis; Weakness: Less effective for short-term anomalies. |
| **ONLINE PROCESSING** | **Distance**               | Jie et al. [^22]          | TPRO           | Time-dependent popular route detection for trajectory anomaly by comparing with representative routes. | Strength: Real-time detection; Weakness: Requires large historical data. |
|                    | **Clustering**                | Chen et al. [^23]         | iBOAT          | Real-time anomaly detection in taxi sub-trajectories for unnecessary detours by greedy drivers. | Strength: Fast processing; Weakness: May miss subtle anomalies.          |
|                    | **Distance**                  | Lee et al. [^24]          | TRAOD          | Angle-based trajectory outlier detection using a segmental detection strategy.               | Strength: Handles direction deviations; Weakness: May not detect all outlier types. |
|                    | **Density**                   | Yu et al. [^25]           | PN-Outlier     | Sub-trajectory anomaly detection based on point neighbor principle, scoring each sub-trajectory. | Strength: Fine-grained detection; Weakness: High computational complexity. |
|                    | **Clustering**                | Yu et al. [^26]           | TN-Outlier     | Sub-trajectory anomaly detection based on trajectory neighbor principle, scoring each sub-trajectory. | Strength: Context-aware detection; Weakness: Sensitive to clustering parameters. |
|                    | **Density**                   | Wu et al. [^27]           | DB-TOD         | Trajectory anomaly detection based on driving behavior, using a probabilistic learning model to estimate costs. | Strength: Behavior-aware detection; Weakness: Model complexity.         |

### ** 3.3 Trajectory Outlier Detection in Traffic Data: iBAT and TROAD Methods

![Figure 5: Trajectory outlier detection illustration. (a) iBAT illustration. (b) TRAOD illustration.](https://github.com/user-attachments/assets/e3068ec8-7020-434d-92a7-9050762134f6)

Figure 5 provides a visual representation of two distinct algorithms—**iBAT** and **TROAD**—designed for anomaly detection in traffic trajectory data. Each method applies a different approach to identify outliers in trajectories, serving specific processing needs: offline and online detection.

#### 1. **iBAT (Isolation-Based Anomalous Trajectory Detection)**
- **Approach**: iBAT is an **offline processing method**. It detects anomalous trajectories by organizing trajectory data into an isolation tree, as shown in part (a) of the diagram.
- **Process**: 
  1. The input is a **trajectory database**, where each trajectory is represented as a sequence of location points (e.g., \(t_1 = (p_1, p_2, p_3, p_4, \dots)\)).
  2. An **isolation tree** is constructed by recursively partitioning the trajectory dataset based on shared commonalities between trajectory segments.
  3. **Trajectories** that share similar paths are grouped together in the tree nodes, with each node representing a subset of similar trajectories.
  4. **Outliers** are identified as those trajectories that deviate significantly from the common patterns, typically located deeper in the tree with fewer similar counterparts.

- **Objective**: The isolation-based mechanism highlights rare trajectories, which are treated as anomalous by their isolation from the majority of the data.

#### 2. **TROAD (Trajectory Outlier Detection for Real-Time Data)**
- **Approach**: TROAD is an **online processing method**. It focuses on identifying **sub-trajectory outliers** within real-time traffic data, as illustrated in part (b) of the diagram.
- **Process**:
  1. **Trajectory database**: Similar to iBAT, TROAD starts with a database of trajectories, each divided into several sub-segments, or **t-partitions**.
  2. The trajectories are split into sub-segments, and each sub-trajectory (denoted as \(L_i^j\)) is analyzed separately.
  3. A **density-based analysis** is performed for each sub-segment. The algorithm calculates the density of trajectories passing through specific partitions, as shown in the table.
  4. **Outliers** are detected based on their density values. Sub-trajectories with significantly lower density than the norm are flagged as outliers, visualized in **red** in the diagram.
  5. TROAD’s real-time capability allows it to continuously monitor incoming trajectory data and detect anomalies as they occur, making it highly suitable for dynamic environments.

- **Objective**: The method aims to **dynamically detect outliers** at the sub-trajectory level, providing more granular anomaly detection in real-time traffic systems.

### Summary
Both **iBAT** and **TROAD** are pivotal in traffic trajectory anomaly detection, but they differ in their focus. iBAT offers robust offline processing to detect trajectory-level outliers, ideal for historical analysis. In contrast, TROAD excels in real-time monitoring, detecting anomalies at the sub-trajectory level, enabling proactive responses to irregularities in urban traffic networks.

---

### **4. Conclusion**
Urban traffic anomaly detection is a rapidly evolving field that leverages advanced machine learning, deep learning, and statistical techniques to identify unusual patterns in both traffic flow and vehicle trajectories. Despite significant progress, several challenges remain, including real-time detection, scalability, and handling of complex and dynamic data.

Researchers continue to develop more sophisticated algorithms that can adapt to the unique challenges of urban environments, aiming to improve urban mobility, safety, and efficiency.

---

## References

[^1]: Djenouri, Y., Belhadi, A., Lin, J. C.-W., Djenouri, D., & Cano, A. (2019). A Survey on Urban Traffic Anomalies Detection Algorithms. IEEE Access, 12192–12205. [https://doi.org/10.1109/access.2019.2893124](https://doi.org/10.1109/access.2019.2893124)
[^2]: Ngan, H. Y. T., Yung, N. H. C., & Yeh, A. G. O. (2015). Outlier detection in traffic data based on the Dirichlet process mixture model. IET Intelligent Transport Systems, 773–781. [https://doi.org/10.1049/iet-its.2014.0063](https://doi.org/10.1049/iet-its.2014.0063)
[^3]: Lam, P., Wang, L., Ngan, H. Y. T., Yung, N. H. C., & Yeh, A. G. O. (2017). Outlier Detection in Large-Scale Traffic Data by Na&amp;#xEF;ve Bayes Method and Gaussian Mixture Model Method. Electronic Imaging, 73–78. [https://doi.org/10.2352/issn.2470-1173.2017.9.iriacv-272](https://doi.org/10.2352/issn.2470-1173.2017.9.iriacv-272)
[^4]: Turochy, R. E., & Smith, B. L. (2002). Applying quality control to traffic condition monitoring. ITSC2000. 2000 IEEE Intelligent Transportation Systems. Proceedings (Cat. No.00TH8493). Presented at the 2000 IEEE Intelligent Transportation Systems. Proceedings, Dearborn, MI, USA. [https://doi.org/10.1109/itsc.2000.881011](https://doi.org/10.1109/itsc.2000.881011)
[^5]: Park, E. S., Turner, S., & Spiegelman, C. H. (2003). Empirical Approaches to Outlier Detection in Intelligent Transportation Systems Data. Transportation Research Record: Journal of the Transportation Research Board, 21–30. [https://doi.org/10.3141/1840-03](https://doi.org/10.3141/1840-03)
[^6]: Dang, T. T., Ngan, H. Y. T., & Liu, W. (2015). Distance-based k-nearest neighbors outlier detection method in large-scale traffic data. 2015 IEEE International Conference on Digital Signal Processing (DSP). Presented at the 2015 IEEE International Conference on Digital Signal Processing (DSP), Singapore, Singapore. [https://doi.org/10.1109/icdsp.2015.7251924](https://doi.org/10.1109/icdsp.2015.7251924)
[^7]: Tang, J., & Ngan, HenryY. T. (2016). Traffic outlier detection by density-based bounded local outlier factors. Information Technology in Industry,Information Technology in Industry. [https://doi.org/10.17762/itii.v4i1.38](https://doi.org/10.17762/itii.v4i1.38)
[^8]: Munoz-Organero, M., Ruiz-Blaquez, R., & Sánchez-Fernández, L. (2018). Automatic detection of traffic lights, street crossings and urban roundabouts combining outlier detection and deep learning classification techniques based on GPS traces while driving. Computers, Environment and Urban Systems, 1–8. [https://doi.org/10.1016/j.compenvurbsys.2017.09.005](https://doi.org/10.1016/j.compenvurbsys.2017.09.005)
[^9]: Shi, Y., Deng, M., Yang, X., & Gong, J. (2018). Detecting anomalies in spatio-temporal flow data by constructing dynamic neighbourhoods. Computers, Environment and Urban Systems, 67, 80-96. [https://doi.org/10.1016/j.compenvurbsys.2017.08.010](https://doi.org/10.1016/j.compenvurbsys.2017.08.010)
[^10]: Cheng, Y., Zhang, Y., Hu, J., & Li, L. (2007). Mining for Similarities in Urban Traffic Flow Using Wavelets. 2007 IEEE Intelligent Transportation Systems Conference. Presented at the 2007 IEEE Intelligent Transportation Systems Conference, Bellevue, WA, USA. [https://doi.org/10.1109/itsc.2007.4357769](https://doi.org/10.1109/itsc.2007.4357769)
[^11]: Liu, W., Zheng, Y., Chawla, S., Yuan, J., & Xing, X. (2011). Discovering spatio-temporal causal interactions in traffic data streams. Proceedings of the 17th ACM SIGKDD International Conference on Knowledge Discovery and Data Mining. Presented at the KDD ’11: The 17th ACM SIGKDD International Conference on Knowledge Discovery and Data Mining, San Diego California USA. [https://doi.org/10.1145/2020408.2020571](https://doi.org/10.1145/2020408.2020571)
[^12]: Pang, L. X., Chawla, S., Liu, W., & Zheng, Y. (2011). On Mining Anomalous Patterns in Road Traffic Streams. In Lecture Notes in Computer Science,Advanced Data Mining and Applications (pp. 237–251). [https://doi.org/10.1007/978-3-642-25856-5_18](https://doi.org/10.1007/978-3-642-25856-5_18)
[^13]: Chawla, S., Zheng, Y., & Hu, J. (2012). Inferring the Root Cause in Road Traffic Anomalies. 2012 IEEE 12th International Conference on Data Mining. Presented at the 2012 IEEE 12th International Conference on Data Mining (ICDM), Brussels, Belgium. [https://doi.org/10.1109/icdm.2012.104](https://doi.org/10.1109/icdm.2012.104)
[^14]: Inoue, R., Miyashita, A., & Sugita, M. (2016). Mining spatio-temporal patterns of congested traffic in urban areas from traffic sensor data. 2016 IEEE 19th International Conference on Intelligent Transportation Systems (ITSC). Presented at the 2016 IEEE 19th International Conference on Intelligent Transportation Systems (ITSC), Rio de Janeiro, Brazil. [https://doi.org/10.1109/itsc.2016.7795635](https://doi.org/10.1109/itsc.2016.7795635)
[^15]: Nguyen, H., Liu, W., & Chen, F. (2017). Discovering Congestion Propagation Patterns in Spatio-Temporal Traffic Data. IEEE Transactions on Big Data, 169–180. [https://doi.org/10.1109/tbdata.2016.2587669](https://doi.org/10.1109/tbdata.2016.2587669)
[^16]: Zhang, J. (2012). Smarter outlier detection and deeper understanding of large-scale taxi trip records. Proceedings of the ACM SIGKDD International Workshop on Urban Computing. Presented at the KDD ’12: The 18th ACM SIGKDD International Conference on Knowledge Discovery and Data Mining, Beijing China. [https://doi.org/10.1145/2346496.2346521](https://doi.org/10.1145/2346496.2346521)
[^17]: Kumar, D., Bezdek, J. C., Rajasegarar, S., Leckie, C., & Palaniswami, M. (2017). A visual-numeric approach to clustering and anomaly detection for trajectory data. The Visual Computer, 33(3), 265–281. [https://doi.org/10.1007/s00371-015-1192-x](https://doi.org/10.1007/s00371-015-1192-x)
[^18]: Zhang, D., Li, N., Zhou, Z.-H., Chen, C., Sun, L., & Li, S. (2011). iBAT. Proceedings of the 13th International Conference on Ubiquitous Computing. Presented at the Ubicomp ’11: The 2011 ACM Conference on Ubiquitous Computing, Beijing China. [https://doi.org/10.1145/2030112.2030127](https://doi.org/10.1145/2030112.2030127)
[^19]: Lv, Z., Xu, J., Zhao, P., Liu, G., Zhao, L., & Zhou, X. (2017). Outlier Trajectory Detection: A Trajectory Analytics Based Approach. In Lecture Notes in Computer Science,Database Systems for Advanced Applications (pp. 231–246). [https://doi.org/10.1007/978-3-319-55753-3_15](https://doi.org/10.1007/978-3-319-55753-3_15)
[^20]: Zhou, X., Ding, Y., Peng, F., Luo, Q., & Ni, L. M. (2017). Detecting unmetered taxi rides from trajectory data. 2017 IEEE International Conference on Big Data (Big Data). Presented at the 2017 IEEE International Conference on Big Data (Big Data), Boston, MA. [https://doi.org/10.1109/bigdata.2017.8257968](https://doi.org/10.1109/bigdata.2017.8257968)
[^21]: Kong, X., Song, X., Xia, F., Guo, H., Wang, J., & Tolba, A. (2018). LoTAD: long-term traffic anomaly detection based on crowdsourced bus trajectory data. World Wide Web, 825–847. [https://doi.org/10.1007/s11280-017-0487-4](https://doi.org/10.1007/s11280-017-0487-4)
[^22]: Zhu, J., Jiang, W., Liu, A., Liu, G., & Zhao, L. (2015). Time-Dependent Popular Routes Based Trajectory Outlier Detection. In Lecture Notes in Computer Science,Web Information Systems Engineering – WISE 2015 (pp. 16–30). [https://doi.org/10.1007/978-3-319-26190-4_2](https://doi.org/10.1007/978-3-319-26190-4_2)
[^23]: Chen, C., Zhang, D., Castro, P. S., Li, N., Sun, L., Li, S., & Wang, Z. (2013). iBOAT: Isolation-Based Online Anomalous Trajectory Detection. IEEE Transactions on Intelligent Transportation Systems, 806–818. [https://doi.org/10.1109/tits.2013.2238531](https://doi.org/10.1109/tits.2013.2238531)
[^24]: Lee, J.-G., Han, J., & Li, X. (2008). Trajectory Outlier Detection: A Partition-and-Detect Framework. 2008 IEEE 24th International Conference on Data Engineering. Presented at the 2008 IEEE 24th International Conference on Data Engineering (ICDE 2008), Cancun, Mexico. [https://doi.org/10.1109/icde.2008.4497422](https://doi.org/10.1109/icde.2008.4497422)
[^25]: Yu, Y., Cao, L., Rundensteiner, E. A., & Wang, Q. (2014). Detecting moving object outliers in massive-scale trajectory streams. Proceedings of the 20th ACM SIGKDD International Conference on Knowledge Discovery and Data Mining. Presented at the KDD ’14: The 20th ACM SIGKDD International Conference on Knowledge Discovery and Data Mining, New York New York USA. [https://doi.org/10.1145/2623330.2623735](https://doi.org/10.1145/2623330.2623735)
[^26]: Yu, Y., Cao, L., Rundensteiner, E. A., & Wang, Q. (2017). Outlier Detection over Massive-Scale Trajectory Streams. ACM Transactions on Database Systems, 1–33. [https://doi.org/10.1145/3013527](https://doi.org/10.1145/3013527)
[^27]: Wu, H., Sun, W., & Zheng, B. (2017). A Fast Trajectory Outlier Detection Approach via Driving Behavior Modeling. Proceedings of the 2017 ACM on Conference on Information and Knowledge Management. Presented at the CIKM ’17: ACM Conference on Information and Knowledge Management, Singapore Singapore. [https://doi.org/10.1145/3132847.3132933](https://doi.org/10.1145/3132847.3132933)

