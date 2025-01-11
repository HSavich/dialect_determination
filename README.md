# Dialect Classification

A common method of handling speech audio in language tasks is to transcribe the audio into text, and then perform the task on the transcribed text. However, in some cases salient information is lost in the process of transcription. One class of these cases is when we want to investigate a certain manner of speaking.

This project trains a wav2vec2 model to classify speech based on the accent of the speaker. The dataset contains five dialects of Latin American Spanish, each accent represented by multiple speakers. Basque is added to the dataset to further investigate how much easier we can expect language determination to be compared to accent determination.

# The Data

The high-quality audio clips of Spanish were introduced in a 2020 ACL Anthology paper available [here](https://aclanthology.org/2020.lrec-1.801/).

<img width="693" alt="spanish dialects dataset info" src="https://user-images.githubusercontent.com/46304188/204838210-5680d0cf-6e33-47c2-9cfc-88ecca5d7633.png">

For each language, the dataset contains audio clips from various speakers with different speaker profiles. Using multiple speakers for each accent is necessary for the model to identify accents based on regional variations, rather than using individual voices and prosodies.

The data for Basque comes from a similar campaign described [here](https://aclanthology.org/2020.sltu-1.3/), only with a focus on minority Western European languages rather than Latin American Spanish.

# The Model

A fine-tuned wav2vec2 model is used for this tasked. This architecture of this model was introduced in a [paper from Facebook AI.](https://arxiv.org/abs/2006.11477)

The image below, pulled from the original paper, provides a visual overview of the model's high-level architecture.

<img width="346" alt="wav2vec2 architecture" src="https://user-images.githubusercontent.com/46304188/204836253-bf8a6445-cbcf-43c5-af8c-13b711e384bc.png">

Wav2vec2 use a CNN on a normalized waveform to extract features. From the features, a transformer network learns a contextualized representation as well as a discretized representation that helps our model identify distinct speech units. Both the discretized and contextualized representations are passed forward in the network.

# Fine Tuning

The training process for this model is lightweight, taking only half an hour before starting to overfit.

<img width="327" alt="image" src="https://user-images.githubusercontent.com/46304188/204849159-683f5e9b-bafb-46bb-862b-33aad2971b03.png">

# Results

![image](https://user-images.githubusercontent.com/46304188/206084896-0de8f6a2-17ef-433b-be8f-0c106e74c058.png)

Accuracy = 0.980

# Analysis

Our model achieves excellent accuracy on the testing set. This is interesting in light of the fact that we split on speakers, so the training objective could have been satisfied by learning to identify individual voices, which would have led to low testing accuracy. Instead, we find that the wav2vec2 model learns to classify accents **before** it tries to "cheat" by identifying voices. This suggests that accent classification may be an easier task than voice classification.

The confusion matrix shows that Basque is the most easily distinguished, which should be expected as it is the only language that isn't Spanish. Puerto Rican was the hardest to identify in the testing set, which is likely due to Puerto Rican Spanish having the lowest number of audio clips in the dataset. 

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


