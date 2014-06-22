### The following R script produce the MeanTidyData.txt from the X_train/test, y_train/test and subject_train/test data
###This script is also submitted as run_analysis.R in the same repository.

x_train <- read.table ("./train/X_train.txt",comment.char = "",colClasses="numeric")
y_train <- read.table ("./train/y_train.txt",comment.char = "",colClasses="numeric")
subject_train <- read.table ("./train/subject_train.txt",comment.char = "",colClasses="numeric")

x_test <- read.table ("./test/X_test.txt",comment.char = "",colClasses="numeric")
y_test <- read.table ("./test/y_test.txt",comment.char = "",colClasses="numeric")
subject_test <- read.table ("./test/subject_test.txt",comment.char = "",colClasses="numeric")

x <- rbind(x_train, x_test)
y <- rbind(y_train, y_test)
subject <- rbind(subject_train, subject_test)

activityLabels <- read.table ("activity_labels.txt",comment.char = "")

features <- read.table ("./features.txt", comment.char = "")

allIndex <- grep("mean\\(\\)|std\\(\\)", features[,2])
rawNames <- features[allIndex, 2]
refinedNames <- gsub("\\(\\)", "", rawNames)
refinedNames <- gsub("-", ".", refinedNames)

activityNames <- activityLabels[y[,1], 2]

data <- cbind(activityNames, x[, allIndex])

data <- cbind(subject, data)

colnames(data) <- c("subject", "activityNames", refinedNames)

d <- melt(data, id.vars=c("subject", "activityNames"))

dgroup <- dcast(data = d, subject + activityNames ~ variable, fun = mean)

dgroup
