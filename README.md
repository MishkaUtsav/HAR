
Human Activity Recognition – Model Evaluation & Learnings

Problem Statement


Given a new datapoint from smartphone sensor readings, predict the corresponding human activity (e.g., Walking, Sitting, Standing, etc.). We use the Human Activity Recognition (HAR) dataset from UCI, which includes both pre-engineered features and raw sensor data.classical machine learning models are applied on the pre-engineered dataset.
 Exploratory Data Analysis (EDA) Dataset Overview:
Train samples: 7352 | Test samples: 2947
Features: 561 (expert-engineered from smartphone sensor data)
Classes: 6 human activities (Walking, Walking Upstairs, Walking Downstairs, Sitting, Standing, Laying)
Key Observations:
Balanced Data:Class distribution is nearly equal,All participants contributed a similar amount of data.No major class imbalance issues.
Static vs. Dynamic Activities:Feature tBodyAccMag-mean() clearly separates Static: Sitting, Standing, Laying  Dynamic: Walking, Upstairs, Downstairs
Feature Insights:
Boxplot of tBodyAccMag-mean():Values < -0.8 → Static activities ,Values > 0 → Mostly Walking Downstairs   Gravity angle (angleX,gravityMean): Positive values strongly indicate Laying activity.
t-SNE Visualization: Most classes form distinct clusters,Minor overlap observed between Sitting and Standing.
Logistic Regression
Best Estimator: LogisticRegression(C=0.23, penalty='l2')
Test Accuracy: 84.85%
Cross-Validation Score: 74.37% (3-fold CV)
Performance Insights:
The model performs very well on Laying (97%) and Walking (88%), but struggles slightly with Sitting vs. Standing — they often get confused due to similar sensor readings.
Confusion Matrix confirms most confusion occurs between Sitting (21%) and Standing (16%).
Macro and Weighted F1-score: 0.85, indicating balanced performance across classes.
Precision & Recall:
Highest for Laying (0.96 / 0.97)
Lowest for Standing and Sitting (~0.75-0.83)
Key Learnings:
Effective for Binary and Multiclass Classification:
 Logistic Regression is a simple yet powerful algorithm that works well for both binary and multiclass classification        problems like Human Activity Recognition.
Logistic Regression gives solid performance on this dataset, especially for dynamic activities.
It is lightweight and interpretable but less effective when class boundaries overlap, as seen with static postures.


Linear SVM –
Best Estimator: LinearSVC(C=0.125, tol=5e-05)
Test Accuracy: 85.72%
Cross-Validation Score: 77.89% (5-fold CV)
Performance Insights
The model performed best on Laying (F1-score: 0.97) and Walking (0.89), similar to Logistic Regression.
Confusion remains between Sitting and Standing, though slightly improved compared to Logistic Regression.
Confusion matrix shows some misclassifications between:sitting and standing
(Sitting misclassified as Standing: 18%)walking upstairs and walking downstairs 
(due to subtle transitions in movement).


Classification Report Highlights
Macro & Weighted F1-score: 0.86, indicating balanced class-wise performance.
Precision & Recall:
Highest for Laying and Walking (above 0.90).
Slightly lower for Static Postures (Sitting, Standing).
Linear SVM outperforms Logistic Regression slightly in overall F1-score and test accuracy. It generalizes better (as seen from the improved cross-validation score) and handles overlapping classes with more confidence. However, challenges remain with confusing static activities due to similar sensor patterns.
Key Learning
Linear models can be powerful with proper regularization and tuning (e.g., C and tol).
SVM shows better generalization than Logistic Regression in this case.
Fine-tuning hyperparameters and increasing cross-validation folds (from 3 to 5) helped improve reliability.


SVM (RBF Kernel) –
 Best Estimator: SVC(C=8, gamma=0.0078125)
 Test Accuracy: 87.39%
 Cross-Validation Score: 78.43% (5-fold CV)
Performance Insights
Best overall performance so far with highest accuracy and F1-scores.
Laying (F1: 0.98) and Walking (F1: 0.88) were classified very well.
Sitting vs. Standing confusion is significantly reduced compared to previous models.
Standing Recall: 0.91 (best so far)
Sitting Recall: 0.83
Macro & Weighted F1-score: 0.87, showing consistent, balanced performance across all activity classes.
Low misclassification observed across all classes in the normalized confusion matrix.
Conclusion
SVM with RBF kernel gives the most robust results, thanks to its non-linear decision boundaries. It handles complex relationships in the sensor data better than linear models. While it's computationally more intensive, it shows clear improvements in handling overlapping and transitional activities.
Key Learning
Non-linear kernels like RBF help when data isn’t linearly separable.
Proper tuning of C and gamma significantly impacts model performance.
Overall, SVM with RBF strikes a strong balance between accuracy and generalization.
Random Forest Classifier
  Best Estimator: RandomForestClassifier(max_depth=13, n_estimators=170)
 Test Accuracy: 88.34%
 Cross-Validation Score: 77.21% (5-fold CV)
 Performance Insights
Strong performer with balanced accuracy and F1-scores across all activities.
Laying (F1: 0.98) and Walking (F1: 0.87) were highly accurate and consistent.
Standing Recall: 0.93 (very high, slightly better than SVM).
Sitting Recall: 0.83 – on par with SVM.
Macro & Weighted F1-score: 0.88, indicating consistent prediction quality across both majority and minority classes.
Minor confusion observed between sitting vs standing and walking vs stairs-related activities, as expected due to similarity in sensor patterns.
 Confusion Matrix
LAYING: Almost perfect classification with only 2% confusion.
SITTING vs STANDING: Slight overlap but handled well.
WALKING_UPSTAIRS/DOWNSTAIRS: Some confusion due to transitional nature, but classification remains strong overall.
 Conclusion:   Random Forest performs very well, offering a great trade-off between accuracy, interpretability, and training time. It handles non-linear patterns and feature interactions effectively, which is crucial in sensor-based activity recognition.While it may slightly lag behind SVM in handling fine-grained transitions (e.g., SITTING/STANDING), it makes up for it with robust generalization and faster training.
 Key Learning
Ensemble methods like Random Forest provide strong baselines with minimal tuning.
They're especially effective in dealing with noise and outliers.
Tree depth and number of trees greatly influence performance – proper tuning is crucial.
LLM Prompting (Without Using Any ML Model)
In this task, I manually implemented the logic of a given prompt without using any machine learning model or AI tool. The prompt involved selecting top features, training on them, evaluating the performance, and saving the results. The model achieved a test accuracy of 78.53%, which was lower compared to models using the full feature set. This exercise helped us  understand how performance can drop with reduced features.






