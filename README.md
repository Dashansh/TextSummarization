# Abstractive Text Summarization

Text summarization, as the name suggests is the task to convert large pieces of information in shorter version, while making sure the short version conveys almost same information. This task falls under two categories:



1. Extractive Summarization

2. Abstractive Summarization



Consider below text,



```"Peter and Elizabeth took a taxi to attend the night party in the city. While in the party, Elizabeth collapsed and was rushed to the hospital."```



**Extractive Summarization**, extracts phrases or words from the text and concatenates them to obtain a summary. Though this summary is grammatically less accurate. For example, with extractive summarization the above sentence is summarized as ```"Peter and Elizabeth attend party city. Elizabeth rushed hospital."``` The techniques involved here mostly fall under the set of unsupervised algorithms, for instance, Markov Chains, TextRank, Latent Semantic Analysis, Bayesian Topic Models and few of the many. They are computationally less intensive than their supervised counterparts. 



On the contrary, **Abstractive Summarization** reduces the length of the text while making sure that the output feels more natural. For example, the same text is summarized here as ```"Elizabeth was hospitalized after attending a party with Peter."``` Deep Learning models that deals with sequential data like LSTM's are used here. They are called sequence-to-sequence models which are based on encoder decoder architectures. Since it involves sequential layers, the large number of parameters drastically increases training period. 



Here, we have tested three different models of increasing complexity - Simple LSTM, Stacked LSTM, Attention with Stacked LSTM.



## Dataset Description

This dataset consists of reviews of fine foods from amazon. The data span a period of more than 10 years, including all ~500,000 reviews up to October 2012. Reviews include product and user information, ratings, and a plain text review. It also includes reviews from all other Amazon categories.



Data includes:

* Reviews from Oct 1999 - Oct 2012

* 568,454 reviews

* 256,059 users

* 74,258 products



It consists of following columns: \['**Id**', '**ProductId**', '**UserId**', '**ProfileName**', '**HelpfulnessNumerator**', '**HelpfulnessDenominator**', '**Score**', '**Time**', '**Summary**', '**Text**']. However, we are only concerned with the **Text** and **Summary**. 



Data is available as a part of a [Kaggle competition](https://www.kaggle.com/snap/amazon-fine-food-reviews) and is stored in google drive for ease in access.



## Model Building

The seq2seq model is an encoder decoder architecture.



The encoder model consists of following parts-



1. Embedding layer with latent dimension = 256

* Input: (None, 80)

* Output: (None, 80, 256)



2. 3 LSTM layers stacked, with same latent dimension and same i/p and o/p size

* Input: (None, 80, 256)

* Output: "Hidden_state" - (None, 80, 256), "state_h" - (None, 256), "state_c" - (None, 256) 



The decoder model is made up of-



1. Embedding Layer with same latent dimension

* Input: (None,) ***\*Note: Here we avoid specifying sequence length of summary, since in inference phase we would like to predict 1 word at a time.\****

* Output: (None, None, 256)



2. LSTM layer

* Input: (None, None, 256)

* Output: "Hidden_state" - (None, None, 256), "state_h" - (None, 256), "state_c" - (None, 256)



3. TimeDistributed Layer with softmax activation

* Input: (None, None, 256)

* Output: (None, None, 13871)



The encoder and decoder model are interconnected by the means of **Attention Mechanism**. 



The job of the encoder is to store the information of the complete sequence in a fixed-length vector. The decoder is initialized with this vector and it produces output sequences. Sometimes, when the input sequence is large enough, the information of the past gets lost in the vector. Attention is a technique which considers encoder output at every time-step and decides how much information is to be carried forward to which decoder state. This makes sense, because in reality humans translate few words at a time rather than entire sentence at ones. The following figure is an example of one such attention mechanism known as Bahdanau Attention.



​    ![Bahdanau Attention](https://blog.floydhub.com/content/images/2019/09/Slide38.JPG "Bahdanau Attention")

    

## Output

Let's take a look at some of the predications.



`"use get virginia peanuts family first year bought online company virginia gourment peanuts exactly expected purchasing"`

**Summary**:  great peanuts great value Predicted.

**Predicted**:  peanut butter review.



`"cookies arrived fresh delicious usual good value plenty share understand use word necessary describe experience"`

**Summary**:  famous amos.

**Predicted**:  great cookies.



`"really loved pasta tastes like natural spaghetti without pain issues associated gluten many products would come mushy like oatmeal one tastes much like ordinary pasta cannot tell difference purchased bionaturae organic gluten free pasta heartland pasta increased price frequently stock longer subscribe save option product mentioned bionaturae pasta gives packs spaghetti still costs less choosing subscribe save bionaturae offered subscribe save also great reviews see turns measure heartland bit bullet order heartland product"`

**Summary**:  no longer offered in the subscribe save program. 

**Predicted**:  low carb pasta.



`"really way add creamer enhance flavor blend signed subscription delivered every month drink every morning work works great home cafe system"`

**Summary**:  if you dont like black coffee. 

**Predicted**:  great coffee.



`"neighborhood dogs walks piper dogs products always fresh consistent size color taste yes said taste read years ago people take snacks hiking share dogs human consumption quality tried maybe would prefer oreo bad test occasion see consistent natural soft peanut butter chews look like fig newtons stay fresh time open package till gone never want overdo snack fastest way cause obesity pet actually count cookies day put little tin keep helped control weight help remember many given"`

**Summary**:  scrumptious. 

**Predicted**:  good dog food.



## Resources

* [Comprehensive Guide to Text Summarization using Deep Learning in Python](https://www.analyticsvidhya.com/blog/2019/06/comprehensive-guide-text-summarization-using-deep-learning-python/)
* [Custom Attention Layer](https://github.com/thushv89/attention_keras/blob/master/src/layers/attention.py)

* [Neural Machine Translation by Jointly Learning to Align and Translate](https://arxiv.org/pdf/1409.0473.pdf)

