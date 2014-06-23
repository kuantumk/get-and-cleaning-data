### here is a explanation of how the following script works...



x_train <- read.table ("./train/X_train.txt",comment.char = "",colClasses="numeric")
y_train <- read.table ("./train/y_train.txt",comment.char = "",colClasses="numeric")
subject_train <- read.table ("./train/subject_train.txt",comment.char = "",colClasses="numeric")
-------------------------
the above codes read in the training set. each variable has the same meaning as the oringal data


x_test <- read.table ("./test/X_test.txt",comment.char = "",colClasses="numeric")
y_test <- read.table ("./test/y_test.txt",comment.char = "",colClasses="numeric")
subject_test <- read.table ("./test/subject_test.txt",comment.char = "",colClasses="numeric")
-------------------------
the above codes read in the test set. each variable has the same meaning as the oringal data



x <- rbind(x_train, x_test)
y <- rbind(y_train, y_test)
subject <- rbind(subject_train, subject_test)
-------------------------
the above codes read in the training set. each variable has the same meaning as the oringal data




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
