---
layout: page
title: Text mining application in transit disruption management
img: /assets/img/7.jpg
importance: 3
category: work
---

[Paper](https://ieeexplore.ieee.org/abstract/document/9261594)
[Code](https://github.com/PepSalehi/NLP_incidents)

Despite rapid advances in automated text processing, many related tasks in transit and other transportation agencies are still performed manually. For example, incident management reports are often manually processed and subsequently stored in a standardized format for later use. The information contained in such reports can be valuable for many reasons: identification of issues with response actions, underlying causes of each incident, impacts on the system, etc. 
    In this paper, we develop a comprehensive, pragmatic automated framework for analyzing rail incident reports to  support a wide range of applications and functions, depending on the constraints of the available data. 
    The objectives are twofold:
    a) extract information that is required in the standard report forms (automation), and
    b) extract other useful content and insights from the unstructured text in the original report that would have otherwise been lost/ignored (knowledge discovery). 
    The approach is demonstrated through a case study involving analysis of 23,728 records of general incidents in the London Underground (LU).
The results show that it is possible to automatically extract delays, impacts on trains, mitigating strategies, underlying incident causes, and insights related to the potential actions and causes, as well as accurate classification of incidents into predefined categories.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/text_questions.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
   Typical questions for analysis of incidents
</div>

In this paper, we use both, knowledge-based and machine learning techniques. Figure below summarizes the information that can be extracted from each incident’s report along with the methods used for each task.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/text_overview.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
   Overview of the analysis process and methods
</div>


Figure \ref{fig:NER} gives an example of the Bi-LSTM CRF model. Each sentence is represented as a sequence of tokens, typically labeled with the BIO (beginning, inside, outside) scheme. For example, "Oxford Station" is tagged as "B-station" and "I-station". 
Let $$\mathbf{x} = \{x_1, x_2, \dots, x_n\}$$ and $$\mathbf{y} = \{y_1, y_2, \dots, y_n\}$$ denote the input token sequence and their tags, respectively, where each token $x_i \in \mathbb{R}^d$ is represented by a d-dimensional vector. A bidirectional Long Short-Term Memory (Bi-LSTM) then computes two hidden representations $$h_t \in \mathbb{R}^H$$ and $$h'_t \in \mathbb{R}^H$$ of the sentence, capturing the left and right context at each word. The final representation is obtained by concatenating the two, $$\hat{h_t} = [h_t;h'_t]$$, which now effectively possesses a representation of a word in context. 
Then a linear layer on top of the Bi-LSTM is used to predict the score of each tag for each word $$e_t= tanh(W\hat{h_t})$$.

<div class="row justify-content-sm-center">
    <div class="col-mm mt-6 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/crf.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
   CRF BLSTM for NER
</div>
<div class="row justify-content-sm-center">
    <div class="col-mm mt-6 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/text_pred.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
   TABLE I: Prediction accuracy for NERTypic
</div>