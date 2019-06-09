# Open University Dataset

In this notebook, I will discuss how a number of algorithms were investigated to predict whether a student would pass an assessment based on the number of interactions with the course material.

After checking whether there were any missing values, the first step was familiarising myself with the data in the Dataset. This could potentially allow me to better optimise my final algorithm.

The process of familiarising myself with the data took the route of :
<ul>
    <li>Finding more about the demographic of the students</li>
    <li>Investigating Pass/Fail breakdown and how the students' education affects it</li>
    <li>Examining the assessment options within the dataset</li>
    <li>Creating histograms/KDE of Pass/Fail based on interactions of the students</li>
</ul>
<img src="Data_first.JPG">

The dataset was slightly skewed towards males and heavily skewed to students under the age of 35. However, those factors didn't seem to affect the pass / fail rate of the students, and so it won't be necessary for that variable to be calculated by the chosen algorithm.

I then examined the breakdown of the Pail/fail rate and investigated whether the students' education affected that rate.

<img src="Data_second.JPG">

We can see that the "Lower than A level" and "No Formal Qualification" has a higher fail and withdrawn rate than the other groups. This could be a significant extra feature that we could add later to our algorithm to improve its performance. Additionally, if we want our algorithm to be able to predict whether our user will be able to get a distinction, the Post Graduate Students have a significant greater likelihood to achieve a distinction. Therefore, it could be used as an extra feature to predict a student's performance.

The next step was to examine how the students were assessed.

<img src="Data_three.JPG">

The charts above show a need to improve the data that is collected. The graphs show that the two main assessment types are Tutor Marked assignments (TMA) and Exams, with exams having a slightly greater possibility of affecting a student's grade. However, if we look at the pass rate, we can see a significant difference between exams and TMA as there are 7 % more students passing the TMA than Exam. This could potential indicate that Exams are more difficult and that a greater amount of interaction needs to occur to get a pass.

However, with the current data we are unable to separate the two assessment types and see whether there is a different range of material interaction needed to achieve a pass. This could have a big impact with the performance of our algorithm as the overlap between the two ranges could result in a number of misclassified predictions.

To improve the dataset, data needs to be added that would allow a separation of the interactivity of the Exams and the TMAs.

The next stage was an examination of the distribution of the Pass / Fail / Distinction.

<img src="histogram.JPG">



From the graphs above, we can see that the fail has a Gaussian Distribution and the pass / Distinction has a Gamma distribution. The KDE plot clearly shows the overlap between the classes, which will make it difficult to get high accuracy for the machine learning algorithm.

Therefore, it was decide to initially has 3 classes; pass, fail and distinction, and then to do further investigate having only two classes; pass and fail. For universities and the majority of students, there would be a strong emphasis on monitoring students on the pass/ fail boundary rather than the pass / distinction boundary.

<h2>Exploring the Machine learning algorithms</h2>



After exploring the data, a number of algorithms were picked to fit the data to. The algorithms that the report initially experiment with included :
<ul>
  <li>Logistic Regression</li>
    <li>Decision Tree</li>
    <li>KNN</li>
    <li>Random Forest</li>
    <li>Naive Bayes</li>
    <li>Support Vector Machine</li>

For each classifier, the data was fitted and the classification report was made,which included information about the Precision, recall and F1 score.

<img src="models.JPG">



I then plotted the relevant information on graphs.
<img src="model_graph.JPG">


As noted earlier on in the report, the overlap of the distinction and pass is affecting the pass performance. In this model, I will be using the pass performance in both precision and recall as the performance indicate of the model. This is due to the fact that the university could use this information to better inform the students about their potential results. Therefore, it is more important to achieve:

1) High Precision - What proportion of positive identifications was actually correct? (Primary Goal)
2) High Recall - What proportion of actual positives was identified correctly?(Secondary Goal)

as the outcome of telling a student that the likelihood of them passing is high and then due to that information, the student eases off their studying resulting in failing, will be far worst than the algorithm informing the student that he will fail at the current material interactivity and then the student passing.
Therefore, to improve the precision and recall, the class "Distinction" was merged with "Pass", and another performance check was done on the algorithms.

<img src="model_details.JPG">


I then plotted the results on graphs to compare the results.

<img src="model_details_graph.JPG">
The two models that could be a good algorithm for this dataset are the Logistic Regression and the SVC. This is because the Logistic Regression has the 2nd highest precision model and the SVC has the best SVC score.

However, despite the SVC classifier having the better F1 score, I will examine using the Logistic Regression Model. With the university collecting data year on year, I will first examine the more computationally efficient model.

<h2> Parameter Tuning </h2>
The parameters that the will be optimize are:
<ul>
  <li>C - Inverse of regularization strength</li>
  <li>Penalty- Used to specify L1 or L2 Regularization used in the penalization</li>
    <li>Threshold - the prediction is based on if a probability is above a certain threshold within the model. By changing the threshold, we can make our model more sensitive to the Pass Classification.</li>
  </ul>
  
The report will use the Grid Search method to search through the different parameters and see how their impact on the performance of the classifier.

<img src="tuning.JPG">

The C value didn't not significantly improve the model and so the investigation of the threshold occurred.

<img src="acc.JPG">

Looking at the chart we can see how the precision and recall will be affected by a change in the Threshold of the model. Again, the most important factor is the precision, as we must make sure to avoid as many False Positives as possible. This is why we will reset the threshold to 0.6. This will severely increase the number of False Negatives as there will be a lot less positive classifications, however it will increase our prediction accuracy for those positive classifications.

<img src="confusion_mat.JPG">

With the paratmers changed, we now have a model that has a 79% precision score.

<h2>Conclusion</h2>

To improve on this model, further investigation could be down to add additional features to the model, such as education. However, I think the greater improvement to the model could occur if information was collected on material interactivity on Exam assessed units and TMA units.
