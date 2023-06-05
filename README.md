# PWPAE-Concept-Drift-Detection-and-Adaptation

This is the code for the paper entitled "**[PWPAE: An Ensemble Framework for Concept Drift Adaptation in IoT Data Streams](https://arxiv.org/pdf/2109.05013.pdf)**" published in **2021 IEEE Global Communications Conference (GLOBECOM)**, doi: [10.1109/GLOBECOM46510.2021.9685338](https://ieeexplore.ieee.org/document/9685338).  
Authors: Li Yang, Dimitrios Michael Manias, and Abdallah Shami  
Organization: The Optimized Computing and Communications (OC2) Lab, ECE Department, Western University

This repository also introduces **concept drift definitions** and **online machine learning methods** for **data stream analytics** using the **[River](https://riverml.xyz/dev/)** library.  

A complete **tutorial code** for the comprehensive and complete pipeline for **concept drift, online machine learning, and data stream analytics**, including dynamic data pre-processing, drift-based dynamic feature selection, dynamic model learning & selection, and online ensemble models, can be found in: [MSANA-Online-Data-Stream-Analytics-And-Concept-Drift-Adaptation](https://github.com/Western-OC2-Lab/MSANA-Online-Data-Stream-Analytics-And-Concept-Drift-Adaptation)

Another **tutorial code** for **concept drift, online machine learning, and data stream analytics** can be found in: [OASW-Concept-Drift-Detection-and-Adaptation](https://github.com/Western-OC2-Lab/OASW-Concept-Drift-Detection-and-Adaptation)

<p float="left">
  <img src="https://github.com/Western-OC2-Lab/PWPAE-Concept-Drift-Detection-and-Adaptation/blob/main/IoTID20.png" width="470" />
  <img src="https://github.com/Western-OC2-Lab/PWPAE-Concept-Drift-Detection-and-Adaptation/blob/main/CICIDS2017.png" width="470" /> 
</p>

## Concept Drift
In non-stationary and dynamical environments, such as IoT environments, the distribution of input data often changes over time, known as concept drift. The occurrence of concept drift will result in the performance degradation of the current trained data analytics model. Traditional offline machine learning (ML) models cannot deal with concept drift, making it necessary to develop online adaptive analytics models that can adapt to the predictable and unpredictable changes in data streams. 

To address concept drift, effective methods should be able to detect concept drift and adapt to the changes accordingly. Therefore, concept drift detection and adaptation are the two major steps for online learning on data streams.

### Drift Detection
* [Adaptive Windowing (ADWIN)](https://riverml.xyz/dev/api/drift/ADWIN/) is a distribution-based method that uses an adaptive sliding window to detect concept drift based on data distribution changes. ADWIN identifies concept drift by calculating and analyzing the average of certain statistics over the two sub-windows of the adaptive window. The occurrence of concept drift is indicated by a large difference between the averages of the two sub-windows. Once a drift point is detected, all the old data samples before that drift time point are discarded.

   * *Albert Bifet and Ricard Gavalda. "Learning from time-changing data with adaptive windowing." In Proceedings of the 2007 SIAM international conference on data mining, pp. 443-448. Society for Industrial and Applied Mathematics, 2007.*

  ```python
  from river.drift import ADWIN
  adwin = ADWIN()
  ```

* [Drift Detection Method (DDM)](https://riverml.xyz/dev/api/drift/DDM/) is a popular model performance-based method that defines two thresholds, a warning level and a drift level, to monitor model's error rate and standard deviation changes for drift detection.

   * *João Gama, Pedro Medas, Gladys Castillo, Pedro Pereira Rodrigues: Learning with Drift Detection. SBIA 2004: 286-295*

  ```python
  from river.drift import DDM
  ddm = DDM()
  ```

### Drift Adaptation
* [Hoeffding tree (HT)](https://riverml.xyz/dev/api/tree/HoeffdingTreeClassifier/) is a type of decision tree (DT) that uses the Hoeffding bound to incrementally adapt to data streams. Compared to a DT that chooses the best split, the HT uses the Hoeffding bound to calculate the number of necessary samples to select the split node. Thus, the HT can update its node to adapt to newly incoming samples.
  
   * *G. Hulten, L. Spencer, and P. Domingos. Mining time-changing data streams. In KDD’01, pages 97–106, San Francisco, CA, 2001. ACM Press.*
  
  ```python
  from river import tree
  model = tree.HoeffdingTreeClassifier(
       grace_period=100,
       split_confidence=1e-5,
       ...
  )
  ```

* [Extremely Fast Decision Tree (EFDT)](https://riverml.xyz/dev/api/tree/ExtremelyFastDecisionTreeClassifier/), also named Hoeffding Anytime Tree (HATT), is an improved version of the HT that splits nodes as soon as it reaches the confidence level instead of detecting the best split in the HT.
  
   * *C. Manapragada, G. Webb, and M. Salehi. Extremely Fast Decision Tree. In Proceedings of the 24th ACM SIGKDD International Conference on Knowledge Discovery & Data Mining (KDD '18). ACM, New York, NY, USA, 1953-1962, 2018.*
  
  ```python
  from river import tree
  model = tree.ExtremelyFastDecisionTreeClassifier(
       grace_period=100,
       split_confidence=1e-5,
       min_samples_reevaluate=100,
       ...
   )
  ```

* [Adaptive random forest (ARF)](https://riverml.xyz/dev/api/ensemble/AdaptiveRandomForestClassifier/) algorithm uses HTs as base learners and ADWIN as the drift detector for each tree to address concept drift. Through the drift detection process, the poor-performing base trees are replaced by new trees to fit the new concept.
  
   * *Heitor Murilo Gomes, Albert Bifet, Jesse Read, Jean Paul Barddal, Fabricio Enembreck, Bernhard Pfharinger, Geoff Holmes, Talel Abdessalem. Adaptive random forests for evolving data stream classification. In Machine Learning, DOI: 10.1007/s10994-017-5642-8, Springer, 2017.*
  
  ```python
  from river import ensemble
  model = ensemble.AdaptiveRandomForestClassifier(
       n_models=3,
       drift_detector=ADWIN(),
       ...
   )
  ```

* [Streaming Random Patches (SRP)](https://riverml.xyz/dev/api/ensemble/SRPClassifier/) uses the similar technology of ARF, but it uses the global subspace randomization strategy, instead of the local subspace randomization technique used by ARF. The global subspace randomization is a more flexible method that improves the diversity of base learners.
  
   * *Heitor Murilo Gomes, Jesse Read, Albert Bifet. Streaming Random Patches for Evolving Data Stream Classification. IEEE International Conference on Data Mining (ICDM), 2019.*
  
  ```python
  from river import ensemble
  base_model = tree.HoeffdingTreeClassifier(
     grace_period=50, split_confidence=0.01,
     ...
   )
  model = ensemble.SRPClassifier(
     model=base_model, n_models=3, drift_detector=ADWIN(),
     ...
  )
  ```

* [Leverage bagging (LB)](https://riverml.xyz/dev/api/ensemble/LeveragingBaggingClassifier/) is another popular online ensemble that uses bootstrap samples to construct base learners. It uses Poisson distribution to increase the data diversity and leverage the bagging performance.
  
   * *Bifet A., Holmes G., Pfahringer B. (2010) Leveraging Bagging for Evolving Data Streams. In: Balcázar J.L., Bonchi F., Gionis A., Sebag M. (eds) Machine Learning and Knowledge Discovery in Databases. ECML PKDD 2010. Lecture Notes in Computer Science, vol 6321. Springer, Berlin, Heidelberg.*
  
  ```python
  from river import ensemble
  from river import linear_model
  from river import preprocessing
  model = ensemble.LeveragingBaggingClassifier(
     model=(
         preprocessing.StandardScaler() |
         linear_model.LogisticRegression()
     ),
     n_models=3,
     ...
  )
  ```


## Abstract of The Paper
As the number of Internet of Things (IoT) devices and systems have surged, IoT data analytics techniques have been developed to detect malicious cyber-attacks and secure IoT systems; however, concept drift issues often occur in IoT data analytics, as IoT data is often dynamic data streams that change over time, causing model degradation and attack detection failure. This is because traditional data analytics models are static models that cannot adapt to data distribution changes. In this paper, we propose a Performance Weighted Probability Averaging Ensemble (PWPAE) framework for drift adaptive IoT anomaly detection through IoT data stream analytics. Experiments on two public datasets show the effectiveness of our proposed PWPAE method compared against state-of-the-art methods.

<p float="left">
  <img src="https://github.com/Western-OC2-Lab/PWPAE-Concept-Drift-Detection-and-Adaptation/blob/main/framework.jpg" width="350" />
</p>

## Implementation 
### Online Learning/Concept Drift Adaptation Algorithms  
* Adaptive Random Forest (ARF)
* Streaming Random Patches (SRP)
* Extremely Fast Decision Tree (EFDT)
* Hoeffding Tree (HT)
* Leveraging Bagging (LB)
* Performance Weighted Probability Averaging Ensemble (PWPAE)
  * Proposed Method

### Drift Detection Algorithms
* Adaptive Windowing (ADWIN)
* Drift Detection Method (DDM)

### Dataset 
1. IoTID20 dataset, a novel IoT botnet dataset
   * Publicly available at: https://sites.google.com/view/iot-network-intrusion-dataset/home

2. CICIDS2017 dataset, a popular network traffic dataset for intrusion detection problems
   * Publicly available at: https://www.unb.ca/cic/datasets/ids-2017.html  

For the purpose of displaying the experimental results in Jupyter Notebook, the sampled subsets of the two datasets are used in the sample code. The subsets are in the "[data](https://github.com/Western-OC2-Lab/PWPAE-Concept-Drift-Detection-and-Adaptation/tree/main/data)" folder.

### Code  
* [globecom2021_PWPAE_IoTID20.ipynb](https://github.com/Western-OC2-Lab/PWPAE-Concept-Drift-Detection-and-Adaptation/blob/main/globecom2021_PWPAE_IoTID20.ipynb): code for the sampled IoTID20 dataset.  
* [globecom2021_PWPAE_CICIDS2017.ipynb](https://github.com/Western-OC2-Lab/PWPAE-Concept-Drift-Detection-and-Adaptation/blob/main/globecom2021_PWPAE_CICIDS2017.ipynb): code for the sampled CICIDS2017 dataset.

### Requirements & Libraries  
* Python 3.6+
* [Scikit-learn](https://scikit-learn.org/stable/)  
* [LightGBM](https://lightgbm.readthedocs.io/en/latest/)
* [River](https://riverml.xyz/dev/) 0.1.0, this old version is available at: [River 0.1.0](https://github.com/Western-OC2-Lab/PWPAE-Concept-Drift-Detection-and-Adaptation/tree/main/libraries)

## Contact-Info
Please feel free to contact us for any questions or cooperation opportunities. We will be happy to help.
* Email: [liyanghart@gmail.com](mailto:liyanghart@gmail.com) or [Abdallah.Shami@uwo.ca](mailto:Abdallah.Shami@uwo.ca)
* GitHub: [LiYangHart](https://github.com/LiYangHart) and [Western OC2 Lab](https://github.com/Western-OC2-Lab/)
* LinkedIn: [Li Yang](https://www.linkedin.com/in/li-yang-phd-65a190176/)  
* Google Scholar: [Li Yang](https://scholar.google.com.eg/citations?user=XEfM7bIAAAAJ&hl=en) and [OC2 Lab](https://scholar.google.com.eg/citations?user=oiebNboAAAAJ&hl=en)

## Citation
If you find this repository useful in your research, please cite this article as:  

L. Yang, D. M. Manias and A. Shami, "PWPAE: An Ensemble Framework for Concept Drift Adaptation in IoT Data Streams," 2021 IEEE Global Communications Conference (GLOBECOM), 2021, pp. 1-6, doi: 10.1109/GLOBECOM46510.2021.9685338.

```
@INPROCEEDINGS{9685338,
  author={Yang, Li and Manias, Dimitrios Michael and Shami, Abdallah},
  booktitle={2021 IEEE Global Communications Conference (GLOBECOM)}, 
  title={PWPAE: An Ensemble Framework for Concept Drift Adaptation in IoT Data Streams}, 
  year={2021},
  pages={1-6},
  doi={10.1109/GLOBECOM46510.2021.9685338}
  }
```
