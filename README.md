# Dialect Classification

In many cases for speech classification, audio -> text -> text classification is a valid workflow, and avoids a lot of the awkwardness of working with audio. However, in some cases salient information is lost in the process of transcription. One class of these cases is when we want to investigate a certain manner of speaking.

In this project, I have trained a wav2vec2 model to classify Spanish speakers based on their accents by where they are from. The data we use contains 5 locales of Latin American Spanish, + Basque to further investigate how much easier we can expect language discrimination to be compared to dialect discrimination.

# The Data

The high-quality audio clips of Spanish were introduced in a 2020 ACL Anthology paper available [here](https://aclanthology.org/2020.lrec-1.801/).

<img width="693" alt="spanish dialects dataset info" src="https://user-images.githubusercontent.com/46304188/204838210-5680d0cf-6e33-47c2-9cfc-88ecca5d7633.png">

For each language, the dataset contains a variety of speakers with different speaker profiles. This is favorable to some other datasets with 1 or very few speakers because our model is more likely to learn identifiers of dialects than individual voices / prosodies.

The data for Basque comes from a similar campaign described [here](https://aclanthology.org/2020.sltu-1.3/), only with a focus on minority Western European languages rather than Latin American Spanish.

# The Model

For this task, I used a wav2vec2 as a base before fine tuning. This model is introduced [here.](https://arxiv.org/abs/2006.11477)

A quick rundown of the model architecture (image from original paper).

<img width="346" alt="wav2vec2 architecture" src="https://user-images.githubusercontent.com/46304188/204836253-bf8a6445-cbcf-43c5-af8c-13b711e384bc.png">

The zoomed-out view of this model is we use a CNN on a normalized waveform to extract features. From the features we use a transformer network to learn a contextualized representation, but also use a discretized representation that helps our model identify distinct speech units. Both the discretized and contextualized representations are passed forward in the network.

# Fine Tuning

The actual training process for this model was relatively light weight, only taking about an hour (and possibly starting to overfit within that time).

<img width="281" alt="image" src="https://user-images.githubusercontent.com/46304188/204837681-42a15453-28b9-479a-ad3d-5b2eea15166f.png">


# Results

![image](https://user-images.githubusercontent.com/46304188/204847009-c2683496-b7e6-48fc-8452-94be8d373729.png)

Accuracy = 0.89

# Analysis

Our model achieves good results, but because of the number of speakers in the dataset, it is likely that the model, at least to some degree, learned which voices belong to which class. Unfortunately, the speaker information isn't labeled in the public dataset, so it is difficult to split our train-test data on different speakers. The fact that there are misclassifications though is paradoxically reassuring because it hints that our model is not doing completely voice-matching, where we would expect near-perfect results.

The distribution of the confusion matrix offers some interesting insight into the relationship between dialects. We would expect Basque to be the most easily identified because it is a different language than the rest of them, but while it is close, it actually seems to be less identifiable than Peruvian Spanish. 

The most often-mistaked dialects are Venezualan and Colombian, which is a good sign we are learning information based on accents. Although I personally am not super familiar with how different Latin American Spanish accents sound, Colombia and Venezuala share a border and have relatively high population exchange.
