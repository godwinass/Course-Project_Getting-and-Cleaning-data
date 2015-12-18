This code book generated to describes the variables, the data, and any transformations work that was performed to clean up the data. The data was collected from the accelerometers from the Samsung Galaxy S smartphone.

Getting the data
#Load Packages
packages <- c("data.table", "reshape2")
sapply(packages, require, character.only=TRUE, quietly=TRUE)

## data.table   reshape2 
##       TRUE       TRUE

#Find path
path <- getwd()
path

## [1] "C:/Users/djl/Desktop/Course Project"

urll <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
f <- "Dataset.zip"
if (!file.exists(path)) {
  dir.create(path)
}
download.file(urll, file.path(path, f),quiet = TRUE)

#Unzip the file
unzip(zipfile="./data/Dataset.zip",exdir="./data")
#unzipped files are in the folder UCI HAR Dataset. Get the list of the files
pathIn <- file.path("./data" , "UCI HAR Dataset")
files<-list.files(pathIn, recursive=TRUE)
files

##  [1] "activity_labels.txt"                         
##  [2] "features.txt"                                
##  [3] "features_info.txt"                           
##  [4] "README.txt"                                  
##  [5] "test/Inertial Signals/body_acc_x_test.txt"   
##  [6] "test/Inertial Signals/body_acc_y_test.txt"   
##  [7] "test/Inertial Signals/body_acc_z_test.txt"   
##  [8] "test/Inertial Signals/body_gyro_x_test.txt"  
##  [9] "test/Inertial Signals/body_gyro_y_test.txt"  
## [10] "test/Inertial Signals/body_gyro_z_test.txt"  
## [11] "test/Inertial Signals/total_acc_x_test.txt"  
## [12] "test/Inertial Signals/total_acc_y_test.txt"  
## [13] "test/Inertial Signals/total_acc_z_test.txt"  
## [14] "test/subject_test.txt"                       
## [15] "test/X_test.txt"                             
## [16] "test/y_test.txt"                             
## [17] "train/Inertial Signals/body_acc_x_train.txt" 
## [18] "train/Inertial Signals/body_acc_y_train.txt" 
## [19] "train/Inertial Signals/body_acc_z_train.txt" 
## [20] "train/Inertial Signals/body_gyro_x_train.txt"
## [21] "train/Inertial Signals/body_gyro_y_train.txt"
## [22] "train/Inertial Signals/body_gyro_z_train.txt"
## [23] "train/Inertial Signals/total_acc_x_train.txt"
## [24] "train/Inertial Signals/total_acc_y_train.txt"
## [25] "train/Inertial Signals/total_acc_z_train.txt"
## [26] "train/subject_train.txt"                     
## [27] "train/X_train.txt"                           
## [28] "train/y_train.txt"


Reading the files. Read the subject files.
dtSubjectTrain <- fread(file.path(pathIn, "train", "subject_train.txt"))
dtSubjectTest  <- fread(file.path(pathIn, "test" , "subject_test.txt" ))

#Read the activity files. For some reason, these are called label files in the README.txt documentation.
dtActivityTrain <- fread(file.path(pathIn, "train", "Y_train.txt"))
dtActivityTest  <- fread(file.path(pathIn, "test" , "Y_test.txt" ))

#Read the data files using a helper function, read the file with read.table instead, then convert the resulting data frame to a data table. Return the data table.

fileToDataTable <- function(f) {
  df <- read.table(f)
  dt <- data.table(df)
}
dtTrain <- fileToDataTable(file.path(pathIn, "train", "X_train.txt"))
dtTest <- fileToDataTable(file.path(pathIn, "test", "X_test.txt"))


Merging the training and the test sets to create one table
dtSubject <- rbind(dtSubjectTrain, dtSubjectTest)
setnames(dtSubject, "V1", "subject")
dtActivity <- rbind(dtActivityTrain, dtActivityTest)
setnames(dtActivity, "V1", "activityNum")
dt <- rbind(dtTrain, dtTest)

#Merging columns
dtSubject <- cbind(dtSubject, dtActivity)
dt <- cbind(dtSubject, dt)


Variable names
names(dt)

##   [1] "subject"     "activityNum" "V1"          "V2"          "V3"         
##   [6] "V4"          "V5"          "V6"          "V7"          "V8"         
##  [11] "V9"          "V10"         "V11"         "V12"         "V13"        
##  [16] "V14"         "V15"         "V16"         "V17"         "V18"        
##  [21] "V19"         "V20"         "V21"         "V22"         "V23"        
##  [26] "V24"         "V25"         "V26"         "V27"         "V28"        
##  [31] "V29"         "V30"         "V31"         "V32"         "V33"        
##  [36] "V34"         "V35"         "V36"         "V37"         "V38"        
##  [41] "V39"         "V40"         "V41"         "V42"         "V43"        
##  [46] "V44"         "V45"         "V46"         "V47"         "V48"        
##  [51] "V49"         "V50"         "V51"         "V52"         "V53"        
##  [56] "V54"         "V55"         "V56"         "V57"         "V58"        
##  [61] "V59"         "V60"         "V61"         "V62"         "V63"        
##  [66] "V64"         "V65"         "V66"         "V67"         "V68"        
##  [71] "V69"         "V70"         "V71"         "V72"         "V73"        
##  [76] "V74"         "V75"         "V76"         "V77"         "V78"        
##  [81] "V79"         "V80"         "V81"         "V82"         "V83"        
##  [86] "V84"         "V85"         "V86"         "V87"         "V88"        
##  [91] "V89"         "V90"         "V91"         "V92"         "V93"        
##  [96] "V94"         "V95"         "V96"         "V97"         "V98"        
## [101] "V99"         "V100"        "V101"        "V102"        "V103"       
## [106] "V104"        "V105"        "V106"        "V107"        "V108"       
## [111] "V109"        "V110"        "V111"        "V112"        "V113"       
## [116] "V114"        "V115"        "V116"        "V117"        "V118"       
## [121] "V119"        "V120"        "V121"        "V122"        "V123"       
## [126] "V124"        "V125"        "V126"        "V127"        "V128"       
## [131] "V129"        "V130"        "V131"        "V132"        "V133"       
## [136] "V134"        "V135"        "V136"        "V137"        "V138"       
## [141] "V139"        "V140"        "V141"        "V142"        "V143"       
## [146] "V144"        "V145"        "V146"        "V147"        "V148"       
## [151] "V149"        "V150"        "V151"        "V152"        "V153"       
## [156] "V154"        "V155"        "V156"        "V157"        "V158"       
## [161] "V159"        "V160"        "V161"        "V162"        "V163"       
## [166] "V164"        "V165"        "V166"        "V167"        "V168"       
## [171] "V169"        "V170"        "V171"        "V172"        "V173"       
## [176] "V174"        "V175"        "V176"        "V177"        "V178"       
## [181] "V179"        "V180"        "V181"        "V182"        "V183"       
## [186] "V184"        "V185"        "V186"        "V187"        "V188"       
## [191] "V189"        "V190"        "V191"        "V192"        "V193"       
## [196] "V194"        "V195"        "V196"        "V197"        "V198"       
## [201] "V199"        "V200"        "V201"        "V202"        "V203"       
## [206] "V204"        "V205"        "V206"        "V207"        "V208"       
## [211] "V209"        "V210"        "V211"        "V212"        "V213"       
## [216] "V214"        "V215"        "V216"        "V217"        "V218"       
## [221] "V219"        "V220"        "V221"        "V222"        "V223"       
## [226] "V224"        "V225"        "V226"        "V227"        "V228"       
## [231] "V229"        "V230"        "V231"        "V232"        "V233"       
## [236] "V234"        "V235"        "V236"        "V237"        "V238"       
## [241] "V239"        "V240"        "V241"        "V242"        "V243"       
## [246] "V244"        "V245"        "V246"        "V247"        "V248"       
## [251] "V249"        "V250"        "V251"        "V252"        "V253"       
## [256] "V254"        "V255"        "V256"        "V257"        "V258"       
## [261] "V259"        "V260"        "V261"        "V262"        "V263"       
## [266] "V264"        "V265"        "V266"        "V267"        "V268"       
## [271] "V269"        "V270"        "V271"        "V272"        "V273"       
## [276] "V274"        "V275"        "V276"        "V277"        "V278"       
## [281] "V279"        "V280"        "V281"        "V282"        "V283"       
## [286] "V284"        "V285"        "V286"        "V287"        "V288"       
## [291] "V289"        "V290"        "V291"        "V292"        "V293"       
## [296] "V294"        "V295"        "V296"        "V297"        "V298"       
## [301] "V299"        "V300"        "V301"        "V302"        "V303"       
## [306] "V304"        "V305"        "V306"        "V307"        "V308"       
## [311] "V309"        "V310"        "V311"        "V312"        "V313"       
## [316] "V314"        "V315"        "V316"        "V317"        "V318"       
## [321] "V319"        "V320"        "V321"        "V322"        "V323"       
## [326] "V324"        "V325"        "V326"        "V327"        "V328"       
## [331] "V329"        "V330"        "V331"        "V332"        "V333"       
## [336] "V334"        "V335"        "V336"        "V337"        "V338"       
## [341] "V339"        "V340"        "V341"        "V342"        "V343"       
## [346] "V344"        "V345"        "V346"        "V347"        "V348"       
## [351] "V349"        "V350"        "V351"        "V352"        "V353"       
## [356] "V354"        "V355"        "V356"        "V357"        "V358"       
## [361] "V359"        "V360"        "V361"        "V362"        "V363"       
## [366] "V364"        "V365"        "V366"        "V367"        "V368"       
## [371] "V369"        "V370"        "V371"        "V372"        "V373"       
## [376] "V374"        "V375"        "V376"        "V377"        "V378"       
## [381] "V379"        "V380"        "V381"        "V382"        "V383"       
## [386] "V384"        "V385"        "V386"        "V387"        "V388"       
## [391] "V389"        "V390"        "V391"        "V392"        "V393"       
## [396] "V394"        "V395"        "V396"        "V397"        "V398"       
## [401] "V399"        "V400"        "V401"        "V402"        "V403"       
## [406] "V404"        "V405"        "V406"        "V407"        "V408"       
## [411] "V409"        "V410"        "V411"        "V412"        "V413"       
## [416] "V414"        "V415"        "V416"        "V417"        "V418"       
## [421] "V419"        "V420"        "V421"        "V422"        "V423"       
## [426] "V424"        "V425"        "V426"        "V427"        "V428"       
## [431] "V429"        "V430"        "V431"        "V432"        "V433"       
## [436] "V434"        "V435"        "V436"        "V437"        "V438"       
## [441] "V439"        "V440"        "V441"        "V442"        "V443"       
## [446] "V444"        "V445"        "V446"        "V447"        "V448"       
## [451] "V449"        "V450"        "V451"        "V452"        "V453"       
## [456] "V454"        "V455"        "V456"        "V457"        "V458"       
## [461] "V459"        "V460"        "V461"        "V462"        "V463"       
## [466] "V464"        "V465"        "V466"        "V467"        "V468"       
## [471] "V469"        "V470"        "V471"        "V472"        "V473"       
## [476] "V474"        "V475"        "V476"        "V477"        "V478"       
## [481] "V479"        "V480"        "V481"        "V482"        "V483"       
## [486] "V484"        "V485"        "V486"        "V487"        "V488"       
## [491] "V489"        "V490"        "V491"        "V492"        "V493"       
## [496] "V494"        "V495"        "V496"        "V497"        "V498"       
## [501] "V499"        "V500"        "V501"        "V502"        "V503"       
## [506] "V504"        "V505"        "V506"        "V507"        "V508"       
## [511] "V509"        "V510"        "V511"        "V512"        "V513"       
## [516] "V514"        "V515"        "V516"        "V517"        "V518"       
## [521] "V519"        "V520"        "V521"        "V522"        "V523"       
## [526] "V524"        "V525"        "V526"        "V527"        "V528"       
## [531] "V529"        "V530"        "V531"        "V532"        "V533"       
## [536] "V534"        "V535"        "V536"        "V537"        "V538"       
## [541] "V539"        "V540"        "V541"        "V542"        "V543"       
## [546] "V544"        "V545"        "V546"        "V547"        "V548"       
## [551] "V549"        "V550"        "V551"        "V552"        "V553"       
## [556] "V554"        "V555"        "V556"        "V557"        "V558"       
## [561] "V559"        "V560"        "V561"

#Set key
setkey(dt, subject, activityNum)


Extracting the mean and standard deviation for each measurement Read the features.txt file.It shows which variables in dt are measurements for the mean and standard deviation.
dtFeatures <- fread(file.path(pathIn, "features.txt"))
setnames(dtFeatures, names(dtFeatures), c("featureNum", "featureName"))

#Subset only measurements for the mean and standard deviation.

dtFeatures <- dtFeatures[grepl("mean\\(\\)|std\\(\\)", featureName)]

#Convert the column numbers to a vector of variable names matching columns in dt.

dtFeatures$featureCode <- dtFeatures[, paste0("V", featureNum)]
head(dtFeatures)

##    featureNum       featureName featureCode
## 1:          1 tBodyAcc-mean()-X          V1
## 2:          2 tBodyAcc-mean()-Y          V2
## 3:          3 tBodyAcc-mean()-Z          V3
## 4:          4  tBodyAcc-std()-X          V4
## 5:          5  tBodyAcc-std()-Y          V5
## 6:          6  tBodyAcc-std()-Z          V6

dtFeatures$featureCode

##  [1] "V1"   "V2"   "V3"   "V4"   "V5"   "V6"   "V41"  "V42"  "V43"  "V44" 
## [11] "V45"  "V46"  "V81"  "V82"  "V83"  "V84"  "V85"  "V86"  "V121" "V122"
## [21] "V123" "V124" "V125" "V126" "V161" "V162" "V163" "V164" "V165" "V166"
## [31] "V201" "V202" "V214" "V215" "V227" "V228" "V240" "V241" "V253" "V254"
## [41] "V266" "V267" "V268" "V269" "V270" "V271" "V345" "V346" "V347" "V348"
## [51] "V349" "V350" "V424" "V425" "V426" "V427" "V428" "V429" "V503" "V504"
## [61] "V516" "V517" "V529" "V530" "V542" "V543"

#Subset these variables using variable names.

select <- c(key(dt), dtFeatures$featureCode)
dt <- dt[, select, with = FALSE]


Using descriptive activity names. Read activity_labels.txt file. It will be used to add descriptive names to the activities.
dtActivityNames <- fread(file.path(pathIn, "activity_labels.txt"))
setnames(dtActivityNames, names(dtActivityNames), c("activityNum", "activityName"))


Labeling with descriptive activity names
#Merging activity labels.
dt <- merge(dt, dtActivityNames, by = "activityNum", all.x = TRUE)

#Adding activityName as a key.
setkey(dt, subject, activityNum, activityName)

#Melt the data table to reshape it from a short and wide format to a tall and narrow format.
dt <- data.table(melt(dt, key(dt), variable.name = "featureCode"))


Merging activity name
dt <- merge(dt, dtFeatures[, list(featureNum, featureCode, featureName)], by = "featureCode", 
            all.x = TRUE)


Creating a new variable, activity that is equivalent to activity Name as a factor class. Create a new variable, feature that is equivalent to featureName as a factor class.
dt$activity <- factor(dt$activityName)
dt$feature <- factor(dt$featureName)

#Seperating features from featureName using the helper function grepthis.
grepthis <- function(regex) {
  grepl(regex, dt$feature)
}

## Features with 2 categories
n <- 2
y <- matrix(seq(1, n), nrow = n)
x <- matrix(c(grepthis("^t"), grepthis("^f")), ncol = nrow(y))
dt$featDomain <- factor(x %*% y, labels = c("Time", "Freq"))
x <- matrix(c(grepthis("Acc"), grepthis("Gyro")), ncol = nrow(y))
dt$featInstrument <- factor(x %*% y, labels = c("Accelerometer", "Gyroscope"))
x <- matrix(c(grepthis("BodyAcc"), grepthis("GravityAcc")), ncol = nrow(y))
dt$featAcceleration <- factor(x %*% y, labels = c(NA, "Body", "Gravity"))
x <- matrix(c(grepthis("mean()"), grepthis("std()")), ncol = nrow(y))
dt$featVariable <- factor(x %*% y, labels = c("Mean", "SD"))

## Features with 1 category
dt$featJerk <- factor(grepthis("Jerk"), labels = c(NA, "Jerk"))
dt$featMagnitude <- factor(grepthis("Mag"), labels = c(NA, "Magnitude"))

## Features with 3 categories
n <- 3
y <- matrix(seq(1, n), nrow = n)
x <- matrix(c(grepthis("-X"), grepthis("-Y"), grepthis("-Z")), ncol = nrow(y))
dt$featAxis <- factor(x %*% y, labels = c(NA, "X", "Y", "Z"))

#Checking to make sure all possible combinations of feature are accounted for by all possible combinations of the factor class variables.

r1 <- nrow(dt[, .N, by = c("feature")])
r2 <- nrow(dt[, .N, by = c("featDomain", "featAcceleration", "featInstrument", 
                           "featJerk", "featMagnitude", "featVariable", "featAxis")])
r1 == r2

## [1] TRUE


Creating a tidy data set. Creating a 2nd data set with the average of each variable for each activity and each subject.
setkey(dt, subject, activity, featDomain, featAcceleration, featInstrument, 
       featJerk, featMagnitude, featVariable, featAxis)
dtTidy <- dt[, list(count = .N, average = mean(value)), by = key(dt)]
dtTidy

##        subject         activity featDomain featAcceleration featInstrument
##     1:       1           LAYING       Time               NA      Gyroscope
##     2:       1           LAYING       Time               NA      Gyroscope
##     3:       1           LAYING       Time               NA      Gyroscope
##     4:       1           LAYING       Time               NA      Gyroscope
##     5:       1           LAYING       Time               NA      Gyroscope
##    ---                                                                    
## 11876:      30 WALKING_UPSTAIRS       Freq             Body  Accelerometer
## 11877:      30 WALKING_UPSTAIRS       Freq             Body  Accelerometer
## 11878:      30 WALKING_UPSTAIRS       Freq             Body  Accelerometer
## 11879:      30 WALKING_UPSTAIRS       Freq             Body  Accelerometer
## 11880:      30 WALKING_UPSTAIRS       Freq             Body  Accelerometer
##        featJerk featMagnitude featVariable featAxis count     average
##     1:       NA            NA         Mean        X    50 -0.01655309
##     2:       NA            NA         Mean        Y    50 -0.06448612
##     3:       NA            NA         Mean        Z    50  0.14868944
##     4:       NA            NA           SD        X    50 -0.87354387
##     5:       NA            NA           SD        Y    50 -0.95109044
##    ---                                                               
## 11876:     Jerk            NA           SD        X    65 -0.56156521
## 11877:     Jerk            NA           SD        Y    65 -0.61082660
## 11878:     Jerk            NA           SD        Z    65 -0.78475388
## 11879:     Jerk     Magnitude         Mean       NA    65 -0.54978489
## 11880:     Jerk     Magnitude           SD       NA    65 -0.58087813


Writing cleaned data in txt format
library(plyr);
write.table(dtTidy, file = "tidydata2.txt",row.name=FALSE)

