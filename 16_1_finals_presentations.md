### Finals week project presentations

Thursday 30 Apr 10AM-12:30

Aim for 10 mins + 5 mins questions
The main goal of this presentation is to **teach us what you learned**

#### Suggested structure

* Introduce the problem. What is your prediction goal?
  * You might explain both:
    * what your ultimate prediction goal is
    * if you scaled back for this project, what you decided to focus on as a first step toward learning to achieve that ultimate goal
* Research the methods available. What approaches did you consider?
  * What was your prediction task (e.g. classification, regression, instance segmentation, object recognition)?
  * Briefly evaluate the pros and cons of the various approaches to this task over the previous decade up to the state of the art
* What model algorithm did you end up using and why?
  * It's OK if you decided that the state of the art was not achievable within the scope of this project and instead chose to use a simpler algorithm or dataset to learn the fundamentals of this task
  * Teach us about this model algorithm (e.g. architecture of NN)
* Describe how you trained, tuned, and evaluated the model
  * What training algorithm did you use and why?
    * Describe the options you considered
  * Describe how you designed the train-validate-test dataset
    * Consider scope of inference
    * Consider test set leakage
  * Describe any feature engineering (e.g. transformations, augmentation, weighting schemes) and how that was intended to help
  * Describe the loss functions and performance metrics available and why you chose the ones you did
  * Compare to at least one baseline model
    * Naive baseline, e.g. mean, median, most common class
    * Default baseline: linear regression, logistic regression, random forest
    * Default baseline for neural nets: one of the above but with simple feature engineering, e.g. for images average pixels in a 3 x 3 grid to give 9 features per image
  * Tell us about the logistics and any hurdles you needed to overcome (time to train, dealing with large datasets, getting software to work, etc)
* Results
  * Show us training diagnostics (e.g. plots of training and validation loss)
  * Present performance metrics (e.g. accuracy, confusion matrix)
  * Present example predictions of the model on test data
* What will you do next to work toward your ultimate goal?

