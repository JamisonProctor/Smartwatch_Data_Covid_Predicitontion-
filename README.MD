# Team Ceres
This describes Team Ceres contribution to the TU Munich TUM.ai Makeathon 2022 (https://makeathon.tum-ai.com)

Our team had 5 members

Altay Kacan - Deep Learning Engineer
Amr Abuzer - Deep Learning Engineer 
Anna Didovych - Business Development / Marketing
Ihor Shybka - Front End Engineer
Jamison Proctor - Data Science / Product Management

Our team came together as a result of the team finding exercises prior to the event. 

## Our Challenge:

We were provided historical heart rate and steps data taken from users of both the Apple Watch and FitBit. 
We were to use this data to predict the onset of illness related to Covid-19 or similar viral infections 
before symptoms were reported by the user.

This challenge was supported by AIforMedicine (https://ai4medicine.com)

## Our Approach:

### Prediction
There were two available sources of data, a phase-1 study and a phase-2 study. They are linked below with the corresponding papers.

Based on our research we found it was best to use anomaly detection algorithms. There were two classes of approaches we've found: 
1. Deterministic (NightSignal algorithm - Finite State Machine)
2. Learning based (LSTM Autoencoders)

The NightSignal algorithm can be found in this GitHub repository : https://github.com/StanfordBioinformatics/wearable-infection. It consists of a finite state machine that compares resting heart rate measurements for a given night with the user history to create yellow or red alerts. The file "FSMmodel.py" and the previously linked repository explains the working of this algorithm. This approach is not personalized and can be applied to all users. Furthermore it requires no training. Also since it's hand-crafted by the authors it might not generalize to different users.

The LSTM Autoencoder approach (LAAD-LSTM autoencoder anomaly detection) is a deep learning method that learns to replicate the time-series data of the users and predict future time points. Once the model is trained, if the error between the measured data and the predicted value is over a learned threshold, the algorithm classifies that point as an anomaly. Ideally each user would have their own model trained on their heart rate data when they were healthy. This makes it hard to onboard new users as they wouldn't have enough data, resulting in a situation similar to the "cold start" problem for recommender systems. 

Due to time limitations we were only able to generate predictions using the finite state machine but we strongly believe that an ensemble method as described below would be useful.

In parallel, we did exploratory data analysis on some of the user’s individual datasets. Jamison tried to create a heart rate baseline by finding the minimum heart rate for each day and taking the mean of the neighboring heart rate data and comparing that day to day. 

Local Outlier Factor / LOF (https://pyod.readthedocs.io/en/latest/_modules/pyod/models/lof.html#LOF), 
Median Absolute Deviation / MAD (https://pyod.readthedocs.io/en/latest/_modules/pyod/models/mad.html) and 
Isolation Forest/IForest (https://pyod.readthedocs.io/en/latest/_modules/pyod/models/iforest.html#IForest) algos from PyOD 

were used to identify outlier data, but it was only able to identify once symptoms were already present. 

We pivoted our approach to re-using the pretrained models provided. We clustered all of the users according to their HR data, then extrapolated which models best represented which cluster. Our plan was to then use an ensemble of the most representative models to make predictions for any user which did not have a pre-trained model. But we ran out of time. 


### UX and UI

We created a UI which shows the user a cute animal as a representation of their health. If our heart rate based prediction were to identify a potential risk for illness the UI would update the animal’s appearance with images which depicted progressively worse health of the animal. Our hope is that the user would feel more guilty not taking rest when it would affect not only their own health, but the health of their cute animal friend. 

Other data metrics were also added as UI elements.  

### Technologies
* Python
* Scikit-Learn
* Pyod
* Pandas, jupyter
* JavaScript
* Tensorflow

## Sources
Phase-1 study: https://www.nature.com/articles/s41551-020-00640-6

LAAD applied to our usecase: https://www.medrxiv.org/content/10.1101/2021.01.08.21249474v1

GitHub repo for the LAAD implementation we wanted to use: https://github.com/gireeshkbogu/LAAD

LAAD in general: https://arxiv.org/abs/1607.00148

Phase-2 study, where the NightSignal algorithm is from: https://www.nature.com/articles/s41591-021-01593-2


