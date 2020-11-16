Predicting death due to COVID-19
=============

<img src="https://img.shields.io/badge/Study%20Status-Started-blue.svg" alt="Study Status: Started">

- Analytics use case(s): **Patient-Level Prediction**
- Study type: **Clinical Application**
- Tags: **COVID-19**
- Study lead: **Jenna Reps**
- Study lead forums tag: **[jreps](https://forums.ohdsi.org/u/jreps)**
- Study start date: **Nov 1, 2020**
- Study end date: **-**
- Protocol: **-**
- Publications: **-**
- Results explorer: **[Shiny App](https://data.ohdsi.org/CovidDeathPrediction/)**


A study to demonstrate the complete OHDSI process for developing well tested prognostic models.  In this study we focus on validating models that predict death within 30 days of hospitalization due to COVID-19.

Suggested Requirements
===================
- R studio (https://rstudio.com)
- Java runtime environment
- Python

Instructions To Install Package
===================
```r
  # get the latest PatientLevelPrediction
  install.packages("devtools")
  devtools::install_github("OHDSI/FeatureExtraction")
  devtools::install_github("OHDSI/PatientLevelPrediction")
  # check the package
  PatientLevelPrediction::checkPlpInstallation()
  
  # install the network package
  devtools::install_github("ohdsi-studies/CovidDeath")
```


Instructions To Run Package
===================
- Execute the study by running the code in (extras/CodeToRun.R) :
```r
  library(CovidDeath)
  # USER INPUTS
#=======================
# The folder where the study intermediate and result files will be written:
outputFolder <- "./covidDeathResults"

# Details for connecting to the server:
dbms <- "you dbms"
user <- 'your username'
pw <- 'your password'
server <- 'your server'
port <- 'your port'

connectionDetails <- DatabaseConnector::createConnectionDetails(dbms = dbms,
                                                                server = server,
                                                                user = user,
                                                                password = pw,
                                                                port = port)

# Add the database containing the OMOP CDM data
cdmDatabaseSchema <- 'cdm database schema'
# Add a sharebale name for the database containing the OMOP CDM data
cdmDatabaseName <- 'a friendly shareable  name for your database'
# Add a database with read/write access as this is where the cohorts will be generated
cohortDatabaseSchema <- 'work database schema'

oracleTempSchema <- NULL

# table name where the cohorts will be generated
cohortTable <- 'covidDeathValidationCohort'
#=======================
execute(connectionDetails = connectionDetails,
                                 databaseName = cdmDatabaseName,
                                 cdmDatabaseSchema = cdmDatabaseSchema,
                                 cohortDatabaseSchema = cohortDatabaseSchema,
                                 oracleTempSchema = oracleTempSchema,
                                 cohortTable = cohortTable,
                                 outputFolder = outputFolder,
                                 createCohorts = T,
                                 runValidation = T,
                                 packageResults = F,
                                 minCellCount = 5,
                                 sampleSize = NULL)
```

The 'createCohorts' option will create the target and outcome cohorts into cohortDatabaseSchema.cohortTable if set to T.  The 'runAnalyses' option will create/extract the data for each prediction problem setting (each Analysis) and externally validate it if set to T.



- To  view the results, run the following after successfully developing the models:
```r
PatientLevelPrediction::viewMultiplePlp(outputFolder)

```

# Development status
Under development. Do not use
