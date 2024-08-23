# Note-for-Anomaly-Detection
This repository contains my personal notes in the field of Anomaly Detection, especially in Transportation. Individuals who are interested in it are welcome to discuss and study together. PS: Only used for personal study!


## 1. **Introduction**
- **Background**: Anomaly detection plays a crucial role across numerous domains, including intrusion detection, fraud detection, fault detection, monitoring systems, and healthcare diagnostics. Anomalies represent rare but significant events that may indicate critical issues[^1].
- **Importance**: Early anomaly detection helps prevent damage, improve system reliability, and safeguard sensitive data. With the advent of big data, the challenge of detecting anomalies has grown, making this area vital in machine learning and data mining.

## 2. **Definition and Types of Anomalies**
- **Definition**: Anomalies (or outliers) refer to data instances that deviate significantly from the majority of the data. These deviations may represent noise, errors, or actual anomalous behaviors.
- **Types of Anomalies**:
  - **Point Anomalies**: Single data instances significantly different from the rest.  
    _Example_: A sudden spike in temperature readings.
  - **Contextual Anomalies** [^2]: Data points that are anomalous only in a specific context (e.g., temporal or spatial).  
    _Example_: A high temperature is normal during summer but abnormal in winter.
  - **Collective Anomalies**: A group of related data points that deviate from the norm collectively.  
    _Example_: A set of data points showing abnormal fluctuations in system performance over a short period.

    ![Figure1: Three main types of anomalies: Point Anomaly, Contextual Anomaly, and Collective Anomaly](https://github.com/user-attachments/assets/e2f2b90f-b997-4eba-bbf0-0e879226d254)

Figure 1 illustrates the three main types of anomalies: Point Anomaly, Contextual Anomaly, and Collective Anomaly. Point anomalies are easy to detect because they are single, isolated outliers that diverge from the rest of the data. Contextual anomalies, on the other hand, are only considered abnormal within a specific context, such as an unusual spike in a time series. Collective anomalies require analyzing data in groups, where individual points may seem normal but the pattern they form deviates from expected behavior.
    


## 3. **Classification of Anomaly Detection Techniques**

### 3.1 **Supervised Learning**
- **Definition**: Supervised methods require labeled data containing both normal and anomalous instances to train a model. The challenge lies in the scarcity and imbalance of anomaly labels.
- **Key Approaches**:
  - **Classification Algorithms**: Decision trees, support vector machines (SVMs), and neural networks classify new data points as normal or anomalous.
  - **Limitations**: Anomalies are rare and labels are difficult to obtain, which may hinder performance.

### 3.2 **Semi-Supervised Learning**
- **Definition**: Semi-supervised methods are trained only on normal data, assuming that anomalies deviate significantly from learned patterns.
- **Key Approaches**:
  - **Autoencoders**: Neural networks designed to encode input data into a lower-dimensional space and reconstruct the data. Anomalies typically exhibit high reconstruction errors.
  - **One-Class SVM**: Creates a boundary around normal data, flagging instances outside the boundary as anomalies.
  - **Limitations**: Dependence on the representativeness of the normal data may cause some anomalies to go undetected.

### 3.3 **Unsupervised Learning**
- **Definition**: Unsupervised methods do not require labeled data and rely on the inherent structure of the data to detect anomalies.
- **Key Approaches**:
  - **Clustering Algorithms**: Methods like K-means and DBSCAN identify outliers that do not belong to any cluster or appear in small clusters.
  - **Distance-Based Methods**: K-nearest neighbors (KNN) detect anomalies based on large distances from their neighbors.
  - **Density-Based Methods**: Local Outlier Factor (LOF) identifies anomalies based on their local density compared to surrounding data points.
  - **Limitations**: Unsupervised methods may have high false positives if the data distribution is complex.

## 4. **Statistical Methods in Anomaly Detection**
- **Foundations**: Statistical techniques assume that normal data follows a specific distribution, with deviations being considered anomalies.
- **Common Techniques**:
  - **Z-Score**: Measures how far a data point is from the mean, in terms of standard deviations. Points with extreme z-scores (> 3 or < -3) are treated as anomalies.
  - **Grubbs' Test**: Identifies one outlier in a dataset by computing the largest deviation from the mean.
  - **Gaussian Mixture Model (GMM)**: Models data as a mixture of Gaussian distributions. Points with low probabilities are anomalies.

**Example**:  
Grubbs' Test identifies single extreme values by comparing them to the rest of the data's distribution.

## 5. **Machine Learning Approaches for Anomaly Detection**

### 5.1 **Classification Methods**
- **Support Vector Machine (SVM)**: Separates normal and anomalous data by maximizing the margin between them. Effective in high-dimensional data but can struggle with imbalanced datasets.
- **Decision Trees**: Tree-based classifiers recursively split data based on feature values. It is easy to interpret but may require ensemble methods like Random Forests to detect complex anomalies.

### 5.2 **Clustering Methods**
- **K-Means**: Partitions data into clusters. Points far from their cluster centroid are treated as outliers.
- **DBSCAN**: Detects dense regions of data and flags isolated points as noise (anomalies).

### 5.3 **Neural Networks**
- **Autoencoders (AE)**: Learn compressed representations of data and attempt to reconstruct it. High reconstruction errors signify anomalies.
- **Variational Autoencoders (VAE)**: VAE is widely used in image generation, data completion and other fields, and is particularly suitable for dealing with abnormal generation problems. The distribution of latent variables is modeled through the generative model to generate abnormal samples.
- **Generative Adversarial Networks (GANs)**: The generator generates normal data samples, and the discriminator learns to distinguish normal data from generated data, and detects anomalies based on the adversarial nature between the generator and the discriminator. (e.g., images, videos, and text data)
- **Recurrent Neural Networks (RNNs)**: Capture temporal dependencies in time-series data. Used to detect anomalies in systems where temporal patterns are important (e.g., stock markets, health monitoring).
- **Convolutional Neural Networks (CNNs)**: In image and video anomaly detection, CNN detects abnormal samples by learning the spatial features of normal data. (e.g., CV, security monitoring, medical images)

## 6. **Applications of Anomaly Detection**
- **Cybersecurity** [^3][^4]: Detects unusual network activity that could indicate intrusions or malware.
- **Financial Fraud Detection** [^5][^6][^7][^8][^9]: Identifies fraudulent transactions by detecting abnormal patterns in financial data.
- **Healthcare** [^10][^11]: Detects anomalous medical records or symptoms to help diagnose diseases.
- **Industrial Systems** [^12][^13]: Monitors equipment for abnormal performance metrics, enabling early fault detection and reducing downtime.
- **Image and Video Surveillance** [^14][^15]: Identifies abnormal behaviors or events in security footage.

## 7. **Challenges in Anomaly Detection**
- **High Dimensionality**: Analyzing data with many features increases complexity, which makes it harder to detect meaningful patterns. Dimensionality reduction techniques (e.g., PCA) can help.
- **Data Imbalance**: Anomalies are rare, making it difficult to train models. Synthetic data generation and resampling techniques are used to address the imbalance.
- **Labeling Difficulties**: Obtaining labeled anomalies is expensive and often infeasible, driving the need for unsupervised and semi-supervised approaches.
- **Concept Drift**: In dynamic environments, normal and anomalous behavior may change over time. Adaptive models that handle concept drift are needed.

## 8. **Future Research Directions**
- **Ensemble Methods**: Combining multiple approaches (statistical, machine learning, and deep learning) to improve robustness and accuracy.
- **Time-Series Anomaly Detection**: As IoT and smart cities expand, detecting anomalies in real-time time-series data becomes critical.
- **Deep Learning**: Advanced architectures like Generative Adversarial Networks (GANs) and Variational Autoencoders (VAEs) can model complex anomalies in high-dimensional data.
- **Domain-Specific Solutions**: Integrating domain knowledge to improve anomaly detection accuracy in specific fields (e.g., healthcare, finance).

![Figure2: Key components associated with deep learning-based anomaly detection technique.](https://github.com/user-attachments/assets/ffa30efb-b589-43b5-91ba-356b2cbc90cd)

The essential elements of deep learning-based anomaly detection methods are methodically arranged into three primary categories by the framework shown in Figure 2: **Applications**, **Type of Anomaly**, and **Type of Model**. This framework contains all of the essential components required to comprehend and create deep learning models for anomaly detection in a variety of areas and anomaly kinds. (Reference [^16] for the image source)



## References

[^1]: Chandola, V., Banerjee, A., & Kumar, V. (2009). Anomaly detection: A survey. ACM computing surveys (CSUR), 41(3), 1-58. [https://doi.org/10.1145/1541880.1541882](https://doi.org/10.1145/1541880.1541882)
[^2]: Song, X., Wu, M., Jermaine, C., & Ranka, S. (2007). Conditional Anomaly Detection. IEEE Transactions on Knowledge and Data Engineering, 631–645. [https://doi.org/10.1109/tkde.2007.1009](https://doi.org/10.1109/tkde.2007.1009)
[^3]: Hofmeyr, S. A., Forrest, S., & Somayaji, A. (1998). Intrusion detection using sequences of system calls. Journal of Computer Security, 151–180. [https://doi.org/10.3233/jcs-980109](https://doi.org/10.3233/jcs-980109)
[^4]: González, F. A., & Dasgupta, D. (2003). Anomaly Detection Using Real-Valued Negative Selection. Genetic Programming and Evolvable Machines, 383–403. [https://doi.org/10.1023/a:1026195112518] (https://doi.org/10.1023/a:1026195112518)
[^5]: Fawcett, T., & Provost, F. (1999). Activity monitoring. Proceedings of the Fifth ACM SIGKDD International Conference on Knowledge Discovery and Data Mining. Presented at the KDD99: The First Annual International Conference on Knowledge Discovery in Data, San Diego California USA. [https://doi.org/10.1145/312129.312195](https://doi.org/10.1145/312129.312195)
[^6]: Ghosh, S., & Reilly, DouglasL. (1994). Credit Card Fraud Detection with a Neural-Network. Hawaii International Conference on System Sciences, Hawaii International Conference on System Sciences. [10.1109/HICSS.1994.323314](https://ieeexplore.ieee.org/document/323314)
[^7]: Burge, P., & Shawe-Taylor, J. (2001). An Unsupervised Neural Network Approach to Profiling the Behavior of Mobile Phone Users for Use in Fraud Detection. Journal of Parallel and Distributed Computing, 61(7), 915–925. [https://doi.org/10.1006/jpdc.2000.1720](https://doi.org/10.1006/jpdc.2000.1720)
[^8]: Brockett, P. L., Xia, X., & Derrig, R. A. (1998). Using Kohonen’s Self-Organizing Feature Map to Uncover Automobile Bodily Injury Claims Fraud. The Journal of Risk and Insurance, 65(2), 245. [https://doi.org/10.2307/253535](https://doi.org/10.2307/253535)
[^9]: Aggarwal, C. C. (2005). On Abnormality Detection in Spuriously Populated Data Streams. Proceedings of the 2005 SIAM International Conference on Data Mining. Presented at the Proceedings of the 2005 SIAM International Conference on Data Mining. [https://doi.org/10.1137/1.9781611972757.8](https://doi.org/10.1137/1.9781611972757.8)
[^10]: Wong, W.-K., Moore, AndrewW., Cooper, GregoryF., & Wagner, MichaelM. (2003). Bayesian network anomaly pattern detection for disease outbreaks. International Conference on Machine Learning,International Conference on Machine Learning.
[^11]: Lin, J., Keogh, E., Fu, A., & Van Herle, H. (2005). Approximations to Magic: Finding Unusual Medical Time Series. 18th IEEE Symposium on Computer-Based Medical Systems (CBMS’05). Presented at the 18th IEEE Symposium on Computer-Based Medical Systems (CBMS’05), Dublin, Ireland. [https://doi.org/10.1109/cbms.2005.34](https://doi.org/10.1109/cbms.2005.34)
[^12]: Keogh, E., Lonardi, S., & Chiu, B. “Yuan-chi.” (2002). Finding surprising patterns in a time series database in linear time and space. Proceedings of the Eighth ACM SIGKDD International Conference on Knowledge Discovery and Data Mining. Presented at the KDD02: The Eighth ACM SIGKDD International Conference on Knowledge Discovery and Data Mining, Edmonton Alberta Canada. [https://doi.org/10.1145/775047.775128](https://doi.org/10.1145/775047.775128)
[^13]: Worden, K. (1997). STRUCTURAL FAULT DETECTION USING A NOVELTY MEASURE. Journal of Sound and Vibration, 201(1), 85–101. [https://doi.org/10.1006/jsvi.1996.0747](https://doi.org/10.1006/jsvi.1996.0747)
[^14]: Byers, S., & Raftery, A. E. (1998). Nearest-Neighbor Clutter Removal for Estimating Features in Spatial Point Processes. Journal of the American Statistical Association, 577–584. [https://doi.org/10.1080/01621459.1998.10473711](https://doi.org/10.1080/01621459.1998.10473711)
[^15]: Singh, S., & Markou, M. (2004). An approach to novelty detection applied to the classification of image regions. IEEE Transactions on Knowledge and Data Engineering, 16(4), 396–406. [https://doi.org/10.1109/tkde.2004.1269665](https://doi.org/10.1109/tkde.2004.1269665)
[^16]: Sydney, R., (CMCRC)), C., (QCRI), S., & HBKU), H. (2019). Deep Learning for Anomaly Detection: A Survey. arXiv Preprint arXiv:1901.03407. [https://doi.org/10.48550/arXiv.1901.03407](
https://doi.org/10.48550/arXiv.1901.03407)


