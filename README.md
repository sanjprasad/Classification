# Classification
To begin analyzing the LIHC risk factor dataset from The Cancer Genome Atlas, the data and the corresponding annotated data were read into separate Pandas dataframes. With this data read in, it became obvious that the naming conventions were different between the datasets so the annotation data was manipulated so that the variable names matched with those in the LIHC data. With these names now consistent, the data frames were merged into a single data frame and processed to separate out the numeric values from the pathologic stages and create train and test data sets using the sklearn train_test_split function. The original dataset contained 42 samples so the test_size parameter in this function was set to 0.75 because smaller values led to not enough data from each of the 7 pathologic stage (Stage I, Stage II, Stage III, Stage IIIA, Stage IIIC, Stage IV, and Not Available) being present in the test data, therefore the confusion matrix was failing to be produced due to mismatched array sizes.

With the test and train data splits, these values were first fed to a linear SVC model that had a poor model accuracy of 15.6%. Upon looking at the reduced dimensionality PCA plot for the data, there was no clear distinction among the pathologic stages and only 27.92% of the variance was explained by 2 principal components. The confusion matrix and classification report of this model showed the Stage II data performed best in this linear model with 2 of the 5 samples accurately being classified as Stage II.

After the poor linear SVC results, a random forest model was applied to the data with n_estimators (i.e. the number of trees in the forest) set to 500. This resulted in a model with a 28.1% accuracy, significantly outperforming the linear model but still not serving as a good representation of the data. In fact, the only samples that were classified correctly at all were the 9 Stage I samples out of the total 13 in the test data.

After analyzing both the linear SVC model and random forest, the most significant features were plotted for both models. The linear model yielded C4A720, CLVS1157807, SERPINA312, APOF319, and AHCTF125909 as significant features with the highest value being 0.006 while the random forest model produced LOH12CR1118426, DSCR410281, RAB369609, FBLN7129804, and ARIH210425 as significant and 0.0035 as the highest value. Because there was no overlap in significant features between these models and neither one accurately classified the data, marker genes to explore further can be taken from both groups such as C4A720, CLVS1157807, and SERPINA312 from the linear model and LOH12CR1118426 and DSCR410281 from the random forest.

The final model tested with the data was the Lazy Classifier to test multiple models simultaneously then plotted against one another with time, F1 scores, and accuracy included to compare results. The top 5 classification models per the Lazy Classifier were Linear SVC, XGB Classifier, Nearest Centroid, Random Forest, and Label Propagation. Of these, the best F1 score was in the Label Propagation model and the fastest model was the Random Forest. Despite the speed increase from the Random Forest model, the Label Propagation F1 scores demonstrated a better fit for the data, and given the poor results of all models, would be worth exploring further.
