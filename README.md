Getting-and-cleaning-data-assignment
====================================

The repository contains the code and readme files for the peer assessment exercise for getting and cleaning data in the Courser course for getting and cleaning data.

#Reading main files
```{r}
features <- read.table("C:/Users/ravi.verma/Desktop/UCI HAR Dataset/features.txt")
activity <- read.table("C:/Users/ravi.verma/Desktop/UCI HAR Dataset/activity_labels.txt")
features$V2 <- as.character(features$V2)
colnames(activity) <- c("subject_ID","activity_ID")
```
This code reads the main files describing the dataset and renames them for better understanding.



#Reading test and train files and putting appropriate variable names
```{r}
subject_test <- read.table("C:/Users/ravi.verma/Desktop/UCI HAR Dataset/test/subject_test.txt")
colnames(subject_test) <- c("subject_ID")

readings_test <- read.table("C:/Users/ravi.verma/Desktop/UCI HAR Dataset/test/X_test.txt")
colnames(readings_test) <- c(features$V2[1:561])

activity_test <- read.table("C:/Users/ravi.verma/Desktop/UCI HAR Dataset/test/y_test.txt")
colnames(activity_test) <- c("activity_ID")

subject_train <- read.table("C:/Users/ravi.verma/Desktop/UCI HAR Dataset/train/subject_train.txt")
colnames(subject_train) <- c("subject_ID")

readings_train <- read.table("C:/Users/ravi.verma/Desktop/UCI HAR Dataset/train/X_train.txt")
colnames(readings_train) <- c(features$V2[1:561])

activity_train <- read.table("C:/Users/ravi.verma/Desktop/UCI HAR Dataset/train/y_train.txt")
colnames(activity_train) <- c("activity_ID")
```
This code reads the files in the test and train folder and renames the columns for better understanding.


#merging test and train files and using descriptive names
```{r}
test_final <- cbind(subject_test,activity_test,readings_test)
train_final <- cbind(subject_train,activity_train,readings_train)
readings <- rbind(test_final,train_final)
readings_final <- readings[order(readings$subject_ID , readings$activity_ID),]
readings_final$activity_ID[readings_final$activity_ID == 1] <- c("WALKING")
readings_final$activity_ID[readings_final$activity_ID == 2] <- c("WALKING_UPSTAIRS")
readings_final$activity_ID[readings_final$activity_ID == 3] <- c("WALKING_DOWNSTAIRS")
readings_final$activity_ID[readings_final$activity_ID == 4] <- c("SITTING")
readings_final$activity_ID[readings_final$activity_ID == 5] <- c("STANDING")
readings_final$activity_ID[readings_final$activity_ID == 6] <- c("LAYING")
```
This code merges the train and test datasets and renames the activity categorical variables to their decriptive values.


#extracting measurements of mean and standard deviation.
```{r}
ext <- grep("mean" , colnames(readings_final))
ext1 <- grep("std" , colnames(readings_final))
mean <- readings_final[ , ext]
std <- readings_final[ , ext1]
final <- cbind(readings_final$subject_ID, readings_final$activity_ID, mean, std)
colnames(final)[1:2] <- c("subject_ID", "activity_ID")
```
This code creates a subset of the dataset with only the mean and standar deviation columns.

#Creating new dataset
```{r}
test_final <- cbind(subject_test,activity_test,readings_test)
train_final <- cbind(subject_train,activity_train,readings_train)
readings <- rbind(test_final,train_final)
readings_final <- readings[order(readings$subject_ID , readings$activity_ID),]
tidy <-aggregate(readings_final, by=list(readings_final$subject_ID,readings_final$activity_ID),  FUN=mean, na.rm=TRUE)
tidy$activity_ID[tidy$activity_ID == 1] <- c("WALKING")
tidy$activity_ID[tidy$activity_ID == 2] <- c("WALKING_UPSTAIRS")
tidy$activity_ID[tidy$activity_ID == 3] <- c("WALKING_DOWNSTAIRS")
tidy$activity_ID[tidy$activity_ID == 4] <- c("SITTING")
tidy$activity_ID[tidy$activity_ID == 5] <- c("STANDING")
tidy$activity_ID[tidy$activity_ID == 6] <- c("LAYING")
```
This code creates a new tidy dataset with the mean values for every variable for each subject and activity.

#saving new tide table
```{r}
write.table(tidy, file = "C:/Users/ravi.verma/Desktop/tidy.txt" , row.names = FALSE)
```
This code saves the dataset in a file named tidy.
