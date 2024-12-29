# Dialect Classification

A common method of handling speech audio in language tasks is to transcribe the audio into text, and then perform the task on the transcribed text. However, in some cases salient information is lost in the process of transcription. One class of these cases is when we want to investigate a certain manner of speaking.

In this project, I have trained a wav2vec2 model to classify Spanish speakers based on their accents by where they are from. The data we use contains five locales of Latin American Spanish, as well as Basque to further investigate how much easier we can expect language discrimination to be compared to dialect discrimination.

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

The actual training process for this model was relatively light weight, only taking about half an hour (and starting to overfit within that time).

<img width="327" alt="image" src="https://user-images.githubusercontent.com/46304188/204849159-683f5e9b-bafb-46bb-862b-33aad2971b03.png">

# Results

![image](https://user-images.githubusercontent.com/46304188/206084896-0de8f6a2-17ef-433b-be8f-0c106e74c058.png)

Accuracy = 0.980

# Analysis

Even splitting on speakers, our model achieves excellent accuracy on the testing set. This is interesting because it indicates that accent classification, at least at this granularity, is an easier task than voice identification, which could have just as easily met the training objective.

The confusion matrix shows that Basque is the most easily distinguished, which should be expecting as it is the only language that isn't Spanish. Puerto Rican was the hardest to identify in the testing set, but I think this is more having to do with PR having the least data moreso than something about the accent itself.

I think if this same size of dataset was used for this same experiment, but there were more speakers (and so not as much fitting on individual voices), we could expect near perfect accuracy.

# References

[Crowdsourcing Latin American Spanish for Low-Resource Text-to-Speech](https://aclanthology.org/2020.lrec-1.801) (Guevara-Rukoz et al., LREC 2020)

[Open-Source High Quality Speech Datasets for Basque, Catalan and Galician](https://aclanthology.org/2020.sltu-1.3) (Kjartansson et al., SLTU 2020)

[wav2vec 2.0: A Framework for Self-Supervised Learning of Speech Representations](https://arxiv.org/abs/2006.11477v3) (Baveski et al., Facebook AI 2020)

[Huggingface Audio Classification Tutorial](https://huggingface.co/docs/transformers/tasks/audio_classification)

# Further Reading

[Towards Data Science Blog about wav2vec2.0](https://towardsdatascience.com/wav2vec-2-0-a-framework-for-self-supervised-learning-of-speech-representations-7d3728688cae)
This blog posts offers some easy reading for understanding the wav2vec2.0 model, as well as how it relates to and improves upon models before it.

[Classifying Accents from Spectograms](https://medium.com/analytics-vidhya/using-machine-learning-to-identify-accents-in-spectrograms-of-speech-5db91c191b6b)
This blog post describes a computer-vision approach to classifying accents by using spectrograms rather than raw waveforms.

[What's a Language Anyways?](https://www.theatlantic.com/international/archive/2016/01/difference-between-language-dialect/424704/)
Outside the machine-learning realm, this The Atlantic article discusses the nuances of classifying languages, dialects, accents, etc. These nuances describe what's achievable with a dialect identifier like the one I trained. Sidenote: this author, John McWhorter, has a great podcast about linguistics!

# This Model on Huggingface Hub

https://huggingface.co/hhsavich/accent_determinator


