## Assignment 5 Writeup - Haoran Wei

#### Objective : 

---

The task we need to do is to make emoji prediction based given tweets. And we have two languages of tweets along with their emojis. 

#### **Steps:**

---

1. Preprocessing data

First the english and spanish dataset are read into line seperated list. And then, nltk stopwords are applied to them. And there still are many noise including some symbols : …·•’”“—➵・. I deleted them. As for hash tags, I only delete the # symbol. For url and user tags, I deleted all of them.

2. Vectorization

My baseline is a **bag of words** model . It has window size of 5. So we could imagine it is a sparse matrix which is not good for latter training and predicting phase.

And then, I use **tf-idf** model.  And for the improved part, I applied the window size of 5 to it to compare the macro F1 score.

I used **word embeddings** in fasttext, and for sentence wise. I added up all word embeddings in a sentence and calculated a average for it as its sentence vector. 

3. Model and prediction

I have tried logistic regression and SVM models only due to the limited amount of time. C values are changed in training part in order to achieve a better macro f1-score. Because I used many different type of vectors, there are many combination between them. And most of them are only logistic regression. I ran SVM just for English based on tf-idf model.

It is possible to train on CNN model if I have more time to finish it.

4. Machine Translation and emoji mapping.

I found the 1 to 1 english - spanish translation text in MUSE github.  So I created dictionary for both languages to translate each word in its dataset. During the word to word translation, some of the sentences cannot be translated. I deleted those sentenses along with their labels.

Mapping corrections : 0 1 2 3 5-4  6-10 7-15 8-11 9-5 11-9  13 14 16 19-13. For other emojis, I just simply drop them. Here the left side of “5-4” is for English emoji. As for single numbers like “0, 1, 2, 3 ...”, they are the same in both languages.

#### Results 

---

Belows are part of my results.

Part 1  ENG:

|                 | Logistic regression                                          | SVM                                                          |
| --------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| BOW             | ![C=1VCLrEng](/Users/weihaoran/Downloads/Assignment5/C=1VCLrEng.png) |                                                              |
| TFIDF           | ![C=4LrForEnglish](/Users/weihaoran/Downloads/Assignment5/C=4LrForEnglish.png) |                                                              |
| TFIDF( ngram 5) | ![C=20TFWithNgramLrEng](/Users/weihaoran/Downloads/Assignment5/C=20TFWithNgramLrEng.png) | ![SVM_eng](/Users/weihaoran/Downloads/Assignment5/SVM_eng.png) |
| W2V             | ![W2VonEngC10](/Users/weihaoran/Downloads/Assignment5/W2VonEngC10.png) |                                                              |

Part 2 SPANISH:

|                | Logistic Regression                                          | SVM                                                          |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| W2V            | ![EsW2V](/Users/weihaoran/Downloads/Assignment5/EsW2V.png)   |                                                              |
| TFIDF          | ![C26TfLrSpanish](/Users/weihaoran/Downloads/Assignment5/C26TfLrSpanish.png) |                                                              |
| TFIDF(ngram 5) | ![C26tfNgramSpanish](/Users/weihaoran/Downloads/Assignment5/C26tfNgramSpanish.png) | ![SVMEsTf](/Users/weihaoran/Downloads/Assignment5/SVMEsTf.png) |

Part 3 Translation:

|                     | English                                                      | Spanish                                                      |
| ------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Logistic Regression | ![Part2EngC90Liblinear](/Users/weihaoran/Downloads/Assignment5/Part2EngC90Liblinear.png) | ![Part2EsC90saga](/Users/weihaoran/Downloads/Assignment5/Part2EsC90saga.png) |

####Discussion

---

1. Data preprocessing is not a easy job for me. We have to assume it has many possibilities.
2. Having tried multiple sentence vector as part of preprocessing. I found out that tf-idf with window size 5 is the strongest feature. English part : 23. Spanish part : 12.207. It definitely has more features from a sentence like term frequency and word context. First I thought sentiment analysis doesn't rely on context much. But things turned out that it has some relationship with its context.
3. As for the models, I have tried svm and logistic regression. But for the parameters tuning part is hard for me to make decision concerning so many parameters. So what I got may not be the optimal value each model can reach.
4. Translation really helps? Well, the results I got from English part is not good. Perhaps the translated part from spanish draged the result. As for Spanish part, it goes up a little, maybe english part really did something. From what I know without seeing results, each countries has its own style of using emojis. This idea would explain it.