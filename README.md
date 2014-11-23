run_analysis.R
==============

Instructions for Course Project:
You should create one R script called run_analysis.R that does the following. 
  1.Merges the training and the test sets to create one data set.
  2.Extracts only the measurements on the mean and standard deviation for each measurement. 
  3.Uses descriptive activity names to name the activities in the data set
  4.Appropriately labels the data set with descriptive variable names. 
  5.From the data set in step 4, creates a second, independent tidy data set with the average of each variable for        each activity and each subject.

## First Step: download raw data using the URL provided on the Course Project webpage.
    fileURL <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
    download.file(fileURL, destfile = "./projdata.zip")
    unzip("./projdata.zip", exdir="getDataProj")
## Create new directory to store all files
    directory <- "getDataProj"
## 'folder' is a string vector containing the files of directory
    folder <- list.files(directory, full.names = TRUE)
    folder
#  OUTPUT : ([1] "getDataProj/UCI HAR Dataset")
## 'files' contains the files within 'folder' 
    files <- list.files(folder, full.names = TRUE)
    files
#  OUTPUT: [1] "getDataProj/UCI HAR Dataset/activity_labels.txt" "getDataProj/UCI HAR Dataset/features.txt"       
           [3] "getDataProj/UCI HAR Dataset/features_info.txt"   "getDataProj/UCI HAR Dataset/README.txt"         
           [5] "getDataProj/UCI HAR Dataset/test"                "getDataProj/UCI HAR Dataset/train"
## 'test' and 'train' extract the fifth and sixth elements respectively of 'files'
    test <- files[5]
    train <- files[6]

## Read and combine 'subject_test.txt', 'X_test.txt', and 'y_test.txt' from 'test' folder to create one data set
## called 'test_x' that contains all the data relevant to the testing data set.
    test_files <- list.files(test, full.names = TRUE)
    test_sub <- read.table(test_files[2]) ## 2947 obs, 1 var
    test_x <- read.table(test_files[3])   ## 2947 obs, 561 vars
    test_y <- read.table(test_files[4])   ## 2947 obs, 1 var
    test_x$SubjectID <- test_sub$V1
    test_x$Activity <- test_y$V1

## Repeat process for training data
    train_files <- list.files(train, full.names = TRUE)
    train_sub <- read.table(train_files[2]) ## 7352 obs, 1 var
    train_x <- read.table(train_files[3])   ## 7352 obs, 561 vars
    train_y <- read.table(train_files[4])   ## 7352 obs, 1 var
    train_x$SubjectID <- train_sub$V1
    train_x$Activity <- train_y$V1

## Merge 'train_x' and 'test_x'
    pd1 <- merge(train_x, test_x, all=TRUE)  ## 10299 obs, 563 vars
    str(pd2)
## Replicate data frame so that original dataset is preserved in 'pd1'
    pd2 <- pd1

## Assign only the variables pertaining to mean and standard deviation to 'pd2'
    pd2 <- data.frame(cbind(pd2$V1, pd2$V2, pd2$V3, pd2$V4, pd2$V5, pd2$V6, pd2$V41, pd2$V42, pd2$V43, 
                        pd2$V44, pd2$V45, pd2$V46, pd2$V81, pd2$V82, pd2$V83, pd2$V84, pd2$V85, 
                        pd2$V86, pd2$V121, pd2$V122, pd2$V123, pd2$V124, pd2$V125, pd2$V126, pd2$V161, 
                        pd2$V162, pd2$V163, pd2$V164, pd2$V165, pd2$V166, pd2$V201, pd2$V202, pd2$V214, 
                        pd2$V215, pd2$V227, pd2$V228, pd2$V240, pd2$V241, pd2$V253, pd2$V254, pd2$V266, 
                        pd2$V267, pd2$V268, pd2$V269, pd2$V270, pd2$V271, pd2$V345, pd2$V346, pd2$V347,
                        pd2$V348, pd2$V349, pd2$V350, pd2$V424, pd2$V425, pd2$V426, pd2$V427, pd2$V428, 
                        pd2$V429, pd2$V503, pd2$V504, pd2$V516, pd2$V517, pd2$V529, pd2$V530, pd2$V542, 
                        pd2$V543, pd2$Activity, pd2$SubjectID))
## NOTE: The assignment asked us to "Extract only the measurements on the mean and standard deviation for each
## measurement." Therefore, I chose to take only the variables where the mean and standard deviation were actually
## being applied to the measurements (aka, the variables ending with 'mean()' and 'std()'. This was simply my
## interpretation of the assignment. I chose to omit the variables 'gravityMean', 'tBodyAccMean', 'tBodyAccJerkMean',
## 'tBodyGyroMean', 'tBodyGyroJerkMean' because I felt these variables themselves represented the mean, and the mean
## of these variables were not actually being calculated.

## Create factor variable for 'Activity' with descriptive levels
    pd2$Activity <- factor(pd2$X67)
    levels(pd2$Activity) <- c("WALKING", "WALKING_UPSTAIRS", "WALKING_DOWNSTAIRS", 
                              "SITTING", "STANDING", "LAYING")


## Rename variables so that they are more descriptive
## FOR MORE INFORMATION on variable names, see the 'Codebook' file.
    names(pd2) <- c("Mean_BodyTime_Accelerometer_Xaxis", "Mean_BodyTime_Accelerometer_Yaxis",
                "Mean_BodyTime_Accelerometer_Zaxis", "StDev_BodyTime_Accelerometer_Xaxis",
                "StDev_BodyTime_Accelerometer_Yaxis", "StDev_BodyTime_Accelerometer_Zaxis", 
                "Mean_GravityTime_Accelerometer_Xaxis", "Mean_GravityTime_Accelerometer_Yaxis",
                "Mean_GravityTime_Accelerometer_Zaxis", "StDev_GravityTime_Accelerometer_Xaxis",
                "StDev_GravityTime_Accelerometer_Yaxis", "StDev_GravityTime_Accelerometer_Zaxis", 
                "Mean_BodyJerkTime_Accelerometer_Xaxis", "Mean_BodyJerkTime_Accelerometer_Yaxis",
                "Mean_BodyJerkTime_Accelerometer_Zaxis", "StDev_BodyJerkTime_Accelerometer_Xaxis",
                "StDev_BodyJerkTime_Accelerometer_Yaxis", "StDev_BodyJerkTime_Accelerometer_Zaxis", 
                "Mean_BodyTime_Gyroscope_Xaxis", "Mean_BodyTime_Gyroscope_Yaxis", "Mean_BodyTime_Gyroscope_Zaxis",
                "StDev_BodyTime_Gyroscope_Xaxis", "StDev_BodyTime_Gyroscope_Yaxis", "StDev_BodyTime_Gyroscope_Zaxis",
                "Mean_BodyJerkTime_Gyroscope_Xaxis", "Mean_BodyJerkTime_Gyroscope_Yaxis", 
                "Mean_BodyJerkTime_Gyroscope_Zaxis", "StDev_BodyJerkTime_Gyroscope_Xaxis",
                "StDev_BodyJerkTime_Gyroscope_Yaxis", "StDev_BodyJerkTime_Gyroscope_Zaxis",
                "Mean_BodyTime_Accelerometer_Magnitude", "StDev_BodyTime_Accelerometer_Magnitude", 
                "Mean_GravityTime_Accelerometer_Magnitude", "StDev_GravityTime_Accelerometer_Magnitude",
                "Mean_BodyJerkTime_Accelerometer_Magnitude", "StDev_BodyJerkTime_Accelerometer_Magnitude",
                "Mean_BodyTime_Gyroscope_Magnitude", "StDev_BodyTime_Gyroscope_Magnitude", 
                "Mean_BodyJerkTime_Gyroscope_Magnitude", "StDev_BodyJerkTime_Gyroscope_Magnitude",
                "Mean_BodyFrequencyDomain_Accelerometer_Xaxis", "Mean_BodyFrequencyDomain_Accelerometer_Yaxis",
                "Mean_BodyFrequencyDomain_Accelerometer_Zaxis", "StDev_BodyFrequencyDomain_Accelerometer_Xaxis", 
                "StDev_BodyFrequencyDomain_Accelerometer_Yaxis", "StDev_BodyFrequencyDomain_Accelerometer_Zaxis",
                "Mean_BodyJerkFrequencyDomain_Accelerometer_Xaxis", 
                "Mean_BodyJerkFrequencyDomain_Accelerometer_Yaxis",
                "Mean_BodyJerkFrequencyDomain_Accelerometer_Zaxis",
                "StDev_BodyJerkFrequencyDomain_Accelerometer_Xaxis",
                "StDev_BodyJerkFrequencyDomain_Accelerometer_Yaxis",
                "StDev_BodyJerkFrequencyDomain_Accelerometer_Zaxis", "Mean_BodyFrequencyDomain_Gyroscope_Xaxis", 
                "Mean_BodyFrequencyDomain_Gyroscope_Yaxis", "Mean_BodyFrequencyDomain_Gyroscope_Zaxis",
                "StDev_BodyFrequencyDomain_Gyroscope_Xaxis", "StDev_BodyFrequencyDomain_Gyroscope_Yaxis",
                "StDev_BodyFrequencyDomain_Gyroscope_Zaxis", "Mean_BodyFrequencyDomain_Accelerometer_Magnitude", 
                "StDev_BodyFrequencyDomain_Accelerometer_Magnitude",
                "Mean_BodyJerkFrequencyDomain_Accelerometer_Magnitude",
                "StDev_BodyJerkFrequencyDomain_Accelerometer_Magnitude", 
                "Mean_BodyFrequencyDomain_Gyroscope_Magnitude", "StDev_BodyFrequencyDomain_Gyroscope_Magnitude",
                "Mean_BodyJerkFrequencyDomain_Gyroscope_Magnitude", 
                "StDev_BodyJerkFrequencyDomain_Gyroscope_Magnitude", "SubjectID", "Activity")

## Create new, tidy data set with the average of each variable for each activity and each subject
## Melt data set using only 'SubjectID' and 'Activity' as ID variables
    install.packages("reshape2")
    library(reshape2)
    pdMelt <- melt(pd2, id.vars = c("SubjectID", "Activity"), measure.vars = c("Mean_BodyTime_Accelerometer_Xaxis",
      "Mean_BodyTime_Accelerometer_Yaxis", "Mean_BodyTime_Accelerometer_Zaxis", "StDev_BodyTime_Accelerometer_Xaxis",
      "StDev_BodyTime_Accelerometer_Yaxis", "StDev_BodyTime_Accelerometer_Zaxis",
      "Mean_GravityTime_Accelerometer_Xaxis", "Mean_GravityTime_Accelerometer_Yaxis",
      "Mean_GravityTime_Accelerometer_Zaxis", "StDev_GravityTime_Accelerometer_Xaxis",
      "StDev_GravityTime_Accelerometer_Yaxis", "StDev_GravityTime_Accelerometer_Zaxis",
      "Mean_BodyJerkTime_Accelerometer_Xaxis", "Mean_BodyJerkTime_Accelerometer_Yaxis",
      "Mean_BodyJerkTime_Accelerometer_Zaxis", "StDev_BodyJerkTime_Accelerometer_Xaxis",
      "StDev_BodyJerkTime_Accelerometer_Yaxis", "StDev_BodyJerkTime_Accelerometer_Zaxis",
      "Mean_BodyTime_Gyroscope_Xaxis", "Mean_BodyTime_Gyroscope_Yaxis", "Mean_BodyTime_Gyroscope_Zaxis",
      "StDev_BodyTime_Gyroscope_Xaxis", "StDev_BodyTime_Gyroscope_Yaxis", "StDev_BodyTime_Gyroscope_Zaxis",
      "Mean_BodyJerkTime_Gyroscope_Xaxis", "Mean_BodyJerkTime_Gyroscope_Yaxis", 
      "Mean_BodyJerkTime_Gyroscope_Zaxis", "StDev_BodyJerkTime_Gyroscope_Xaxis", "StDev_BodyJerkTime_Gyroscope_Yaxis",
      "StDev_BodyJerkTime_Gyroscope_Zaxis", "Mean_BodyTime_Accelerometer_Magnitude",
      "StDev_BodyTime_Accelerometer_Magnitude", "Mean_GravityTime_Accelerometer_Magnitude",
      "StDev_GravityTime_Accelerometer_Magnitude", "Mean_BodyJerkTime_Accelerometer_Magnitude",
      "StDev_BodyJerkTime_Accelerometer_Magnitude", "Mean_BodyTime_Gyroscope_Magnitude",
      "StDev_BodyTime_Gyroscope_Magnitude", "Mean_BodyJerkTime_Gyroscope_Magnitude",
      "StDev_BodyJerkTime_Gyroscope_Magnitude", "Mean_BodyFrequencyDomain_Accelerometer_Xaxis", 
      "Mean_BodyFrequencyDomain_Accelerometer_Yaxis", "Mean_BodyFrequencyDomain_Accelerometer_Zaxis",
      "StDev_BodyFrequencyDomain_Accelerometer_Xaxis", "StDev_BodyFrequencyDomain_Accelerometer_Yaxis",
      "StDev_BodyFrequencyDomain_Accelerometer_Zaxis", "Mean_BodyJerkFrequencyDomain_Accelerometer_Xaxis",
      "Mean_BodyJerkFrequencyDomain_Accelerometer_Yaxis", "Mean_BodyJerkFrequencyDomain_Accelerometer_Zaxis",
      "StDev_BodyJerkFrequencyDomain_Accelerometer_Xaxis", "StDev_BodyJerkFrequencyDomain_Accelerometer_Yaxis",
      "StDev_BodyJerkFrequencyDomain_Accelerometer_Zaxis", "Mean_BodyFrequencyDomain_Gyroscope_Xaxis",
      "Mean_BodyFrequencyDomain_Gyroscope_Yaxis", "Mean_BodyFrequencyDomain_Gyroscope_Zaxis",
      "StDev_BodyFrequencyDomain_Gyroscope_Xaxis", "StDev_BodyFrequencyDomain_Gyroscope_Yaxis",
      "StDev_BodyFrequencyDomain_Gyroscope_Zaxis", "Mean_BodyFrequencyDomain_Accelerometer_Magnitude",
      "StDev_BodyFrequencyDomain_Accelerometer_Magnitude", "Mean_BodyJerkFrequencyDomain_Accelerometer_Magnitude",
      "StDev_BodyJerkFrequencyDomain_Accelerometer_Magnitude", "Mean_BodyFrequencyDomain_Gyroscope_Magnitude",
      "StDev_BodyFrequencyDomain_Gyroscope_Magnitude", "Mean_BodyJerkFrequencyDomain_Gyroscope_Magnitude",
      "StDev_BodyJerkFrequencyDomain_Gyroscope_Magnitude"))
## Cast the data set so that the rows are consolidated by 'SubjectID' and 'Activity', and the means of the rest of the ## measurements for each person and activity are calculated.
    tidy <- dcast(pdMelt, SubjectID + Activity ~ variable, mean)
    View(tidy)

## Create .txt file for the new tidy data set
  write.table(tidy, file="./tidy.txt", row.names=FALSE)

