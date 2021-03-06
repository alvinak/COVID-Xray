Overview
----
This repository is an implementation of the paper "[COVID-MobileXpert: On-Device COVID-19 Patient Triage and Follow-up using Chest X-rays](https://arxiv.org/pdf/2004.03042.pdf)".


Android App
-------
Our Android App is available [here](https://drive.google.com/file/d/1yqNsVHkrrCoo_XYedOOqSUJRnzc0vjIB/view?usp=sharing). Please feel free to have a try. You can directly download the images provided in "Test Cases for App" for testing (click on the "View code").

Here are the steps to test with your own CXR images (click on the "View all of README.md"):
* Download and install our App [here](https://drive.google.com/file/d/1yqNsVHkrrCoo_XYedOOqSUJRnzc0vjIB/view?usp=sharing).
* Download and install Microsoft Office Lens [here](https://play.google.com/store/apps/details?id=com.microsoft.office.officelen).
* Display a CXR image on your screen (PACS system or PC).
* Open Microsoft Office Lens and take a snapshot of the CXR image under the "DOCUMENT" mode (adjust border if needed).
* Choose the "GRAYSCALE" filter and save the snapshot.
* Open our App, load the snapshot from the gallery and make the prediction (check the following live demo).


Live demo
------
[<img src="Demo/CXR_Images_test.png" width="300">](https://www.youtube.com/watch?v=zcU6x0nTlp0)
[<img src="Demo/Snapshots_test.png" width="300">](https://www.youtube.com/watch?v=GWRhMH-N9Tc)


Triage Datasets
-----
* Pre-training Data (108,948 CXR Images)
  * [ChestX-ray8: Hospital-scale Chest X-ray Database and Benchmarks on Weakly-Supervised Classification and Localization of Common Thorax Diseases.](https://nihcc.app.box.com/v/ChestXray-NIHCC)  
* Fine-tuning Data (537 CXR Images)
  * [COVID-19 Image Data Collection.](https://github.com/ieee8023/covid-chestxray-dataset)  
  * [RSNA Pneumonia Detection Challenge.](https://www.kaggle.com/c/rsna-pneumonia-detection-challenge)  
* Fine-tuning data is split into training/validation/testing sets with 125/18/36 images for each class.

Follow-up Dataset
------
<p><img src="readme/label.png" alt="test" width="600"></p>

We first assign a opacity score S for each COVID-19 positive CXR image in [COVID-19 Image Data Collection](https://github.com/ieee8023/covid-chestxray-dataset) using the scoring system provided by this [paper](https://arxiv.org/abs/2005.11856). The figure shows an example of how we generate CXR image sequences and assign corresponding radiological trajectory labels (i.e., "Worse", "Stable", "Improved"). We collect a total of 159 CXR image squences, the data is split it into training/validation/testing sets with 111/16/32 samples.


COVID-MobileXpert Architecture with Three-player KTD Training
----
<p><img src="readme/model.png" alt="test" width="850"></p>


Model Training and Evaluation
----
Codes and learned model parameters are available in the Main folder. Here are the steps for training and testing:

Triage Model:
* Put the CXR images in the Dataset folder as the following structure:


```
Dataset
   train
      clean
      covid
      pneumonia
   test
      clean
      covid
      pneumonia
   validation
      clean
      covid
      pneumonia
```
* Download the pre-trained model [here](https://drive.google.com/file/d/1lgaGtAfdnjnziHSZ0TaYKisYGjwLxebU/view?usp=sharing) and save it into RF_model folder.
* Run the .ipynb file for triage model training and testing.  

Follow-up Model:
* Put the COVID-19 CXR images and metadata.csv from [COVID-19 Image Data Collection](https://github.com/ieee8023/covid-chestxray-dataset) in the Dataset_FollowUp folder.
* Run the [code](https://github.com/mlmed/covid-severity) of severity scoring system to assign opacity scores to CXR images and save them to metadata.csv.
* Run the feature.ipynb to extract and save features from CXR images.
* Run the data_generation.ipynb to generate CXR image sequences and assign corresponding radiological trajectory labels.
* Run the model_FollowUp.ipynb for follow-up model training and testing.


On-device Deployment of the COVID-MobileXpert
------
<p><img src="readme/android.png"  align="middle" alt="test" width="800"></p>

We provide the source code for deployment with [Pytorch Mobile](https://drive.google.com/file/d/1lgaGtAfdnjnziHSZ0TaYKisYGjwLxebU/view?usp=sharing) and [Android Studio](https://developer.android.com/studio), which is developed based on this [repository](https://github.com/johnolafenwa/PytorchMobile). The source code contains an example model, if you want to deploy other models, here are the steps:
* Download the pre-trained models.
* Use the script "TorchScript_converter.py" to convert the model to TorchScript (.pt).
* Put the model under "src/main/assets" folder
* Change the path in 'MainActivity.java' to the current .pt file.
* Build and test.

Results
----
Evaluation of COVID-19 Patient Triage and Follow-up Performance.
<p><img src="readme/AUC.png" alt="test" width="800"></p>

Dependencies
-----
* Python 3.7
* Pytorch 1.3

Citation
------
```
Xin Li, Chengyin Li and Dongxiao Zhu
COVID-MobileXpert: COVID-MobileXpert: On-Device COVID-19 Patient Triage and Follow-up using Chest X-rays, arXiv:2004.03042, 2020  
https://github.com/xinli0928/COVID-Xray
```

```
@misc{li2020covidmobilexpert,
    title={COVID-MobileXpert: On-Device COVID-19 Patient Triage and Follow-up using Chest X-rays},
    author={Xin Li and Chengyin Li and Dongxiao Zhu},
    year={2020},
    eprint={2004.03042},
    archivePrefix={arXiv},
    primaryClass={eess.IV}
}
```
