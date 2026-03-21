# Discussion paper for week 10 (Thursday)

There will be no class on Tuesday (use this time to research your discussion paper selection and individual project).

You need to read the paper plus submit a small written reflection. The reflection should be **pushed to github before class**.  You won't get credit for the discussion unless you push to github before class. The reflection should include your thoughts on each of the questions under "questions to consider" below. It can be very brief and in bullet-point form but feel free to expand if you prefer.


## Thursday

* Valavi R, Guillera-Arroita G, Lahoz-Monfort JJ, Elith J (2021). Predictive performance of presence-only species distribution models: a benchmark study with reproducible code. *Ecological Monographs* 0:e01486. https://doi.org/10.1002/ecm.1486.
* This is a case study using a range of machine learning algorithms, including random forest and boosting (but not deep learning)
* Questions to consider:
  * Is the prediction task regression or classirfication?
  * Describe the scope of inference.
    * What data and context are the authors hoping to generalize from and to?
    * What aspects of the work appear to be in distribution or in sample?
    * Is there the possibility of test-set leakage?

  * What general principles of machine learning algorithms are demonstrated among the models considered?
  * What modeling approaches were found to be best?
  * What is it about those algorithms that likely made them the best performers?
  * Can the performance of any of the algorithms be improved (and likely by how much)?
  * Why does downsampled random forest perform better than regular random forest? Would this affect other algorithms considered in the paper?
  * What could be the impact of the sizes of the datasets on the conclusions from this paper?
  * Do you have any critiques of the paper, either methodological or about the conclusions drawn?
  * 2-3 other bullet points, which might be insights or questions you want to raise in the discussion




## Reading guide

Since this is a longish paper, here are some tips for sections to focus on and some to skip.

* Abstract and Intro
  * skim for context
* Materials and Methods
  * PG 1-2
    * skim
  * Modeling methods
    * skim text and Table 1
    * how much do you recognize?
    * for methods you don't know, can you understand from general principles?
  * Model tuning
    * read well
    * Table 2: study well, critically appraise choices for tuning parameters, especially for models you know (RF, GBM, XGBoost)
  * Model evaluation
    * read well
  * Statistical comparison
    * skip
* Results and discussion
  * Overview
    * read well, study figures
  * Results averaged across all regions and species
    * read well, study figures
  * Comparison with previous studies
    * skip
  * Other approaches to evaluation
    * skim
  * Best performing models per species
    * read
  * Skim the rest and dive in if something interests you
* Conclusion and perspectives
  * read
