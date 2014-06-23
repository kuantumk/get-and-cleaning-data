### here is a explanation of how the following script works...


```{r}
#read in the training set. each variable has the same meaning as the oringal data
x_train <- read.table ("./train/X_train.txt",comment.char = "",colClasses="numeric")
y_train <- read.table ("./train/y_train.txt",comment.char = "",colClasses="numeric")
subject_train <- read.table ("./train/subject_train.txt",comment.char = "",colClasses="numeric")


#read in the test set. each variable has the same meaning as the oringal data
x_test <- read.table ("./test/X_test.txt",comment.char = "",colClasses="numeric")
y_test <- read.table ("./test/y_test.txt",comment.char = "",colClasses="numeric")
subject_test <- read.table ("./test/subject_test.txt",comment.char = "",colClasses="numeric")



#combine the training set and test set
x <- rbind(x_train, x_test)
y <- rbind(y_train, y_test)
subject <- rbind(subject_train, subject_test)


#read in the activity lables from activity_labels.txt file
activityLabels <- read.table ("activity_labels.txt",comment.char = "")

#read in the feature IDs from features.txt file
features <- read.table ("./features.txt", comment.char = "")


#grap out all indices that point to mean() and std() measured variables
allIndex <- grep("mean\\(\\)|std\\(\\)", features[,2])


#construct a data frame that stores the decriptive names for the features
rawNames <- features[allIndex, 2]
refinedNames <- gsub("\\(\\)", "", rawNames)
refinedNames <- gsub("-", ".", refinedNames)


#construct a data frame that stores the decriptive names for the activities
activityNames <- activityLabels[y[,1], 2]

#combine all subjects, activities and measured data set
data <- cbind(activityNames, x[, allIndex])
data <- cbind(subject, data)
colnames(data) <- c("subject", "activityNames", refinedNames)

#melt the data and regroup them by subject and activityNames for dcast()
d <- melt(data, id.vars=c("subject", "activityNames"))

#calculate the mean for every subject and every activity.
dgroup <- dcast(data = d, subject + activityNames ~ variable, fun = mean)

dgroup
```
