subject <- rbind(subjectTrain, subjectTest)
activity <- rbind(activityTrain, activityTest)
features <- rbind(featuresTrain, featuresTest)

colnames(features) <- t(featuresNames[2])

colnames(activity) <- "Activity"
colnames(subject) <- "Subject"
completeData <- cbind(features,activity,subject)


columnsWithMeanSTD <- grep(".*Mean.*|.*Std.*", names(completeData), ignore.case = TRUE)
requiredColumns <- c(columnsWithMeanSTD, 562, 563)
dim(completeData)
extractedData <- completeData[,requiredColumns]
dim(extractedData)


extractedData$Activity <- as.character(extractedData$Activity)
for(i in 1:6){
  extractedData$Activity[extractedData$Activity == i] <- as.character(activityLabels[i, 2])
}

extractedData$Activity <- as.factor(extractedData$Activity)

names(extractedData)


extractedData$Subject <- as.factor(extractedData$Subject)
extractedData < data.table(extractedData)

tidyData <- aggregate(. ~Subject + Activity, extractedData, mean)
tidyData <- tidyData[order(tidyData$Subject,tidyData$Activity),]
write.table(tidyData, file = "Tidy.txt", row.names = FALSE)
