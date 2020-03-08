# mba
Market Basket Analysis

R script to be used inside Azure Machine Learning Studio, R module.
--------------------------------------------------------------------------

# Map 1-based optional input ports to variables
mydata <- maml.mapInputPort(1) # class: data.frame

# Split data
dt <- split(mydata$Products, mydata$ID)

# Loading arules package
if(!require(arules)) install.packages("arules")

# Convert data to transaction level
dt2 = as(dt,"transactions")

# aggregated data
rules = apriori(dt2, parameter=list(support=0.005, confidence=0.8))
#rules = apriori(dt2, parameter=list(support=0.005, confidence=0.8, minlen = 3))
#rules = apriori(dt2, parameter=list(support=0.005, confidence=0.8, maxlen = 4))

#Convert rules into data frame
rules3 = as(rules, "data.frame")

#Clean Rules
rules3$rules=gsub("\\{", "", rules3$rules)
rules3$rules=gsub("\\}", "", rules3$rules)
rules3$rules=gsub("\"", "", rules3$rules)

# Select data.frame to be sent to the output Dataset port
maml.mapOutputPort("rules3");

R script ends here
-------------------------------------------------------------------------------------------
