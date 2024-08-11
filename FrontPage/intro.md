# Stylometric Intelligent Archive (SIA)

The Stylometry Intelligent Archive (SIA) provides researchers with a toolkit for stylometry. Stylometry applies statistical measures to literary style. It is founded on the discovery by John Burrows in the 1970s that the frequencies of very common words like ‘the’ and ‘you’ hold rich information about style. Stylometry offers the promise of finding patterns in language use in large collections of samples, patterns which are not necessarily evident to the unaided reader and which can be tested for statistical significance. The best-known application of stylometry is authorship detection, but it has been used for stylistic analysis more broadly, and across and within numerous languages, modes and periods.

Within SIA researchers can assemble and manage collections of texts, make counts of words within them and then either run experiments with the word counts locally or export tables for analysis elsewhere. Notable features are the ability to filter word counting by XML elements, to segment texts as overlapping or non-overlapping blocks, and to store and edit text sets for repeated use. SIA runs a caching system to speed up large and complex operations. 

From the home page of SIA, you can upload Texts or Text Sets, then run one of four experiments.

<p align="center"><img src="https://github.com/user-attachments/assets/e64cff68-0cfe-4b1d-862c-ff07fdc28a6c" height="150">&nbsp;<img src="https://github.com/user-attachments/assets/3ba02b25-6760-4677-8be5-292b4329f403" height="150"></p>

![1](https://github.com/Systemik-Solutions/sia_site/assets/29685780/1df5c2ba-2bcd-4e77-af82-4500e8d9133f)

- **Concordance**: A listing of all the occurrences of a target word in a Text, together with the words occurring immediately before and after.
- **Word Frequencies**: A measure of the frequency of certain words or strings of words (NGrams) within a Text.
- **Divergence**: A measure of the similarity or difference between two Text Sets.
- **Burrows’ Zeta**: A method for using the frequencies of mid-range words for a classification exercise. Take a Text Set of Class A samples and a Text Set of Class B samples. Which words which are used regularly in Class A and rarely in Class B, and vice-versa? Each sample in the analysis is then given two scores, based on how many of the words in the two lists occur in it.

Explore the Table of Contents for more detail on Texts, Text Sets, and the four experiments. SIA can be used on the web platform, or through an API. Each chapter has instructions for both methods. The API documentation sections use JSON Schema to describe the API request and response data objects. Note that the JSON Schema is only used for descriptive purposes. It should be by no means used for validations.

You can also visit [the SIA Github Discussions page](https://github.com/Systemik-Solutions/sia_site/discussions) to share any thoughts you have about SIA or this Jupyter Book.

```{tableofcontents}
```
