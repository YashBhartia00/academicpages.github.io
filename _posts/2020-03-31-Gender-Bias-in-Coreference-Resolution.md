
This post will explore what coreference is, how can it be gender-biased, and some models to reduce the Biases. Following a [paper](https://arxiv.org/pdf/1804.06876)
### **Winograd and ambiguous references**
Winograd was a challenge created as a Turing test to determine how well an NLP(Natural Language processing) agent works. The task is to assign a reference to an ambiguous pronoun.  
>The city councilmen refused the demonstrators a permit because they feared/advocated violence.  

Here `they` would refer to *councilmen* if using *feared* and to *demonstrators* if using *advocated*. This means that the system would require world knowledge along with grammar.  
The paper uses Winograd to make sentences with stereotypical and non-stereotypical biases (based on occupational statistics) 
>Nurse is more likely to be a female  
>Carpenter is more likely to be a male  

The following table represents percentage of females in various occupations.

|Occupation|%|Occupation|%|Occupation|%|Occupation|%|
| --- | --- | --- | ---|---|---|---|---|
Carpenter| 2| Chief| 27| Editor| 52|Teacher| 78|
Mechanician| 4| Janitor| 34| Designers| 54|Sewer| 80|
Construction worker| 4| Lawyer| 35| Accountant| 61|Librarian| 84|
Labourer| 4| Cook| 38| Auditro| 61|Assistant| 85|
Driver| 6| Physician| 38| Writer| 63|Cleaner| 89|
Sheriff| 14| CEO| 39| Baker| 65|Housekeeper| 89|
Mover| 18| Analyst| 41| Clerk| 72|Nurse| 90|
Developer| 20| Manager| 43| Cashier| 73|Receptionist| 90|
Farmer| 22| Supervisor| 44| Counsellors| 73|Hairdressers| 92|
Guard| 22| Salesperson| 48| Attendant| 76|Secretary| 95|

<p style = "font-size:10pt" align = "center"> Occupational Statistics showing % of females in various occupations </p>


### **Coreference and resolution systems**
Many times when we use pronouns in English, it could refer to a lot of different things in the sentence, if I may say *a con of using pronouns*.
<!-- i think a few languages dont usse pronouns as we do in english, does that decrease gender bias on trainin in those languages. -->

![Coreference example][4]
<p style = "font-size:10pt" align = "center">An example of complex coreference.</p>

A Coreference resolution system addresses this problem. In simple words, it tries to assign the pronouns to the proper noun.  

**We will be discussing three such systems:**
- Stanford deterministic coreference system [[RULE]][5]: A handcrafted rule-based system.
- Berkeley coreference resolution system [[FEATURE]][6]: It uses a shallow understanding of the features of references.
- UW end-to-end neural coreference resolution system [[E2E]][7]: State of the art model for resolution.  

*Teaching the machine how to solve an equation is a better way than telling it the individual steps.*

### **Changes in the dataset to reduce gender bias**
1. Changing named entity to anonymous entries 
    >Barack Obama was re-elected.  
    John went to his house. 
    
    is changed to 
    >E1 was re-elected.   
    E2 went to his house. 

2. Replacing male entities with females and vice versa.
    >E2 went to *her* house.  

This augmented dataset, used along with the original dataset (with anonymous name entries still there) helps equalize the count of gender pronoun usage in all contexts.

- The bias of the dataset has, in many ways, risen from history. Is changing historical sentences the right way to remove bias from a dataset? 
- The removal of gender bias in this context could also lose out to information that the system would learn.
- This was exactly what they expected to happen. Hence they decided to grade how well their data augmentation worked by the difference in the performance of pre-trained vis-a-vis newly trained models on the augmented dataset.

### **The need to make their own dataset**

Often in machine learning, especially NLP, you will realize that you need a more specific dataset for your task. The same happened here.  
While [Ontonotes 5.0][3] was perfect for all the coreference resolution system, it was difficult to use data augmentation techniques and then evaluate the gender bias (Winobias) as the augmented data had already been fed as training data. As a result there were very few entries that were anti-stereotypical in the original dataset.

- So they decided to make a new dataset to better identify gender bias in those resolution systems using occupations and pairs of sentences. This was evaluated on the basis of two types of sentences:

    One with syntactic cues
    >  { entity1 } { interacts with } { entity2 }and then { interacts with } { pronoun } for { circumstances }.

    and other with less/no cues
    >{ entity1 } { interacts with } { entity2 } { conjunction }   { pronoun }   { circumstances }

![Ambiguous pronoun, without cues][1]
![Ambiguous pronoun, with cues][2]
<p style = "font-size:10pt" align = "center">Images showing ambiguous pronoun usage without and with cues. The arrow represents the noun it is referring to, dotted line means it is anti-stereotypical.</p>

### **Reducing bias from resources:**
1. Word embeddings: replaced [GloVe](https://nlp.stanford.edu/projects/glove/) with [debiased vectors](https://arxiv.org/abs/1607.06520 "Bolukbasi et al., 2016"), which has [review papers ](https://arxiv.org/abs/1903.03862 "Lipstick on pig")against it
2. Gender lists: Used on non-word embedding approaches (feature-rich and rule-based) 
   - equalize the count of female and male nouns  

- The result is, against all the critiques, the biases were almost entirely removed, while the system performance was barely affected (remember that the performance being affected shows that the system was biased initially )
- 2/3 methods (Stanford deterministic system is rule-based and cannot be trained) showed a significant drop in Winobias


Method|OntoNotes|T1-p|T1-a|Avg|\|Diff\||T2-p|T2-a|Avg|\|Diff\||
---|---|---|---|---|---|---|---|---|---|
E2E|67.2|74.9|47.7|61.3|27.2*|88.6|77.3|82.9|11.3*
E2E-R|66.5|62.4|60.3|61.3|2.1|78.4|78.0|78.2|0.4
Feature|64.0|62.9|58.3|60.6|4.6*|68.5|57.8|63.1|10.7*
Feature-R|63.6|62.2|60.6|61.4|1.7|70.0|69.5|69.7|0.6
Rule|58.7|72.0|37.5|54.8|34.5*|47.8|26.6|37.2|21.2*

<p style = "font-size:10pt" align = "center"> 
<b>The table of results of this entire process.</b>  
</p>  

`-R`**:** Results after augmentation of data and changing resource bias.  
`-p` **:** Pro-stereotyped, `-a`**:**  Anti-stereotyped  
`Avg` **:** Average, `|Diff|` **:** Modulus of difference between `-a` and `-p` 

### **A few observations from the results**
-  Pro-stereotyped sentences got a much better evaluation than anti-stereotyped
    - Being biased helped the system predict the references more easily, meaning the system learned to be biased from the data provided
- Given syntactic cues, the system has much less bias and more average accuracy (b/w pro and anti-stereotyped sentences)
  - This means that on bigger datasets, where the world knowledge and sentence information is much more easily accessible to the prediction systems, it would be less likely to give a gender-biased outcome and focus more on the cues given to it.
  - However, this also means that in ambiguous cases, it will most likely predict the gender biased outcome since it makes more sense statistically (after all we can't deny the fact that carpenters are more likely to be men).
-	E2E had much more initial bias as compared to Feature based (more than twice as much), even if E2E is state of the art method. This shows how the best model will have a much heavier gender bias as it is ingrained into our languages and society.

 This makes natural language processing all the more challenging, because bias needs to be eliminated from the system and should not affect accuracy. The above mentioned methods are not perfect but it is one step forward in what we can do.


[1]: \assets\images\WinoBias\withoutCues.png
[2]: \assets\images\WinoBias\withCues.png
[3]: (https://catalog.ldc.upenn.edu/LDC2013T19)
[4]: \assets\images\WinoBias\corefexample.png
[5]: (https://nlp.stanford.edu/pubs/coreference-emnlp10.pdf)
[6]: (https://www.aclweb.org/anthology/D13-1203.pdf)
[7]: (https://www.aclweb.org/anthology/D17-1018.pdf)
