# Dialect Classification

In many cases for speech classification, audio -> text -> text classification is a valid workflow, and avoids a lot of the awkwardness of working with audio. However, in some cases salient information is lost in the process of transcription. One class of these cases is when we want to investigate a certain manner of speaking.

In this project, I have trained a wav2vec2 model to classify Spanish speakers based on their accents by where they are from. The data we use contains 5 locales of Latin American Spanish, + Basque to further investigate how much easier we can expect language discrimination to be compared to dialect discrimination.

# The Data

The high-quality audio clips of Spanish were introduced in a 2020 ACL Anthology paper available [here](https://aclanthology.org/2020.lrec-1.801/).

For each language, the dataset contains a variety of speakers with different speaker profiles. This is favorable to some other datasets with 1 or very few speakers because our model is more likely to learn identifiers of dialects than individual voices / prosodies.

The data for Basque comes from a similar campaign described [here](https://aclanthology.org/2020.sltu-1.3/), only with a focus on minority Western European languages rather than Latin American Spanish.

# The Model

For this task, I used a wav2vec2 as a base before fine tuning. This model is introduced [here.](https://arxiv.org/abs/2006.11477)

A quick rundown of the model architecture (image from original paper).

<img width="346" alt="wav2vec2 architecture" src="https://user-images.githubusercontent.com/46304188/204836253-bf8a6445-cbcf-43c5-af8c-13b711e384bc.png">

# Fine Tuning

The actual training process for this model was relatively light weight, only taking about an hour (and possibly starting to overfit within that time).

# Results

![image](https://user-images.githubusercontent.com/46304188/204835858-8a18777b-8f26-4c97-86ab-c01f86356876.png)
