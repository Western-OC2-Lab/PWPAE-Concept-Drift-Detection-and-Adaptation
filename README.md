# PWPAE-Concept-Drift-Detection-and-Adaptation

This is the code for the paper entitled "**[PWPAE: An Ensemble Framework for Concept Drift Adaptation in IoT Data Streams](https://arxiv.org/pdf/2104.10529.pdf)**" published in **2021 IEEE Global Communications Conference (GLOBECOM)**.  
Authors: Li Yang, Dimitrios Michael Manias, and Abdallah Shami  
Organization: The Optimized Computing and Communications (OC2) Lab, ECE Department, Western University

<p float="left">
  <img src="https://github.com/Western-OC2-Lab/PWPAE-Concept-Drift-Detection-and-Adaptation/blob/main/IoTID20.png" width="470" />
  <img src="https://github.com/Western-OC2-Lab/PWPAE-Concept-Drift-Detection-and-Adaptation/blob/main/CICIDS2017.png" width="470" /> 
</p>

## Abstract
As the number of Internet of Things (IoT) devices and systems have surged, IoT data analytics techniques have been developed to detect malicious cyber-attacks and secure IoT systems; however, concept drift issues often occur in IoT data analytics, as IoT data is often dynamic data streams that change over time, causing model degradation and attack detection failure. This is because traditional data analytics models are static models that cannot adapt to data distribution changes. In this paper, we propose a Performance Weighted Probability Averaging Ensemble (PWPAE) framework for drift adaptive IoT anomaly detection through IoT data stream analytics. Experiments on two public datasets show the effectiveness of our proposed PWPAE method compared against state-of-the-art methods.

## Implementation 
### Dataset 
1. IoTID20 dataset, a novel IoT botnet dataset
   * Publicly available at: https://sites.google.com/view/iot-network-intrusion-dataset/home

2. CICIDS2017 dataset, a popular network traffic dataset for intrusion detection problems
   * Publicly available at: https://www.unb.ca/cic/datasets/ids-2017.html  

For the purpose of displaying the experimental results in Jupyter Notebook, the sampled subsets of the two datasets are used in the sample code. The subsets are in the "data" folder.

### Code  
* [globecom2021_PWPAE_IoTID20.ipynb](https://github.com/Western-OC2-Lab/PWPAE-Concept-Drift-Detection-and-Adaptation/blob/main/globecom2021_PWPAE_IoTID20.ipynb): code for the sampled IoTID20 dataset.  
* [globecom2021_PWPAE_CICIDS2017.ipynb](https://github.com/Western-OC2-Lab/PWPAE-Concept-Drift-Detection-and-Adaptation/blob/main/globecom2021_PWPAE_CICIDS2017.ipynb): code for the sampled CICIDS2017 dataset.

### Requirements & Libraries  
* Python 3.6+
* [scikit-learn](https://scikit-learn.org/stable/)  
* [LightGBM](https://lightgbm.readthedocs.io/en/latest/)
* [river](https://riverml.xyz/dev/)

## Contact-Info
Please feel free to contact us for any questions or cooperation opportunities. We will be happy to help.
* Email: [liyanghart@gmail.com](mailto:liyanghart@gmail.com) or [Abdallah.Shami@uwo.ca](mailto:Abdallah.Shami@uwo.ca)
* GitHub: [LiYangHart](https://github.com/LiYangHart) and [Western OC2 Lab](https://github.com/Western-OC2-Lab/)
* LinkedIn: [Li Yang](https://www.linkedin.com/in/li-yang-65a190176/)  
* Google Scholar: [Li Yang](https://scholar.google.com.eg/citations?user=XEfM7bIAAAAJ&hl=en) and [OC2 Lab](https://scholar.google.com.eg/citations?user=oiebNboAAAAJ&hl=en)

## Citation
If you find this repository useful in your research, please cite this article as:  

L. Yang, D. M. Manias, and A. Shami, “PWPAE: An Ensemble Framework for Concept Drift Adaptation in IoT Data Streams,” in *2021 IEEE Glob. Commun. Conf. (GLOBECOM)*, Madrid, Spain, Dec. 2021.

```
@INPROCEEDINGS{1570723427,
  author={Yang, Li and Manias, Dimitrios Michael and Shami, Abdallah},
  booktitle={2021 IEEE Global Communications Conference (GLOBECOM)}, 
  title={PWPAE: An Ensemble Framework for Concept Drift Adaptation in IoT Data Streams}, 
  year={2021},
  pages={1-6},
  doi={}
  }
```
