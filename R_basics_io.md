---
title: "FSiB - IO"
author: "Robert LeRhmann (modified by David Gomez-Cabrero)"
date: "2022"
output: html_document
---

# Week 1

## \# Reading/Writing Data in R

## Comma-separated values (CSV)

# Make sure we are in the right directory, otherwise we won't find the expected 
# input data file.
##setwd('???')
#setwd("C:/Users/alami/Desktop/BIo/bioInformaticCourses")

# Read commands have a range of parameters to adjust to the format of the input
# file. I like to visually separate the input to improve readability.

#"C:/Users/alami/Desktop/BIo/bioInformaticCourses/DATA/DATA_FSB_SET_1.csv"

data <- read.table("DATA/DATA_FSB_SET_1.csv", 
                   header = TRUE,
                   row.names = 1,
                   sep=',',
                   quote=NULL,
                   skip = 0
                  )
dim(data)
head(data,4)
class(data)

# Skip is a useful feature in case data files have header annotations that are 
# not marked by leading comment characters.

# Notice how choosing the wrong separator leads to columns not being split up.
data <- read.table("DATA/DATA_FSB_SET_1.csv", 
                   sep='#'
                  )
dim(data)
head(data)

# Shortcut: read.csv is like read.table but with csv presets
data <- read.csv("DATA/DATA_FSB_SET_1.csv",
                 row.names = 1)
dim(data)
head(data)


# Writing is equally easy.
outfile <- 'DATA/DATA_SET_TMP.csv'
write.csv(data,
         file = outfile,
         quote = FALSE)



## Tab-separated values (TSV)

# Separators can be chosen freely, should not occur in the data though.
# Comma and Tab are the most common.
outfile <- 'DATA/DATA_SET_TMP.tsv'
write.table(data,
         file = outfile,
         quote = FALSE)

data.reread <- read.table(outfile)
dim(data.reread)
head(data.reread)

outfile <- 'DATA/DATA_SET_TMP.tsv'
write.table(data,sep = "\t",
         file = outfile,
         quote = FALSE)

data.reread <- read.table(outfile)
dim(data.reread)
head(data.reread)


## Binary


# Let's first save our dataset as binary file.
outfile <- 'DATA/DATA_SET_TMP.Rdata'
save(data, 
     file = outfile)
# Now remove the data object from the work space.
rm(data)

# Loading lets data reappear in the work space.
load(outfile)

# Same with saveRDS.
outfile <- 'DATA/DATA_SET_TMP.Rds'
saveRDS(object = data, 
     file = outfile)

data.reread <- readRDS(outfile)
dim(data.reread)


## Excel



# Load Excel IO package
# install.packages("openxlsx")
require(openxlsx)

# Write our data set or list of data to xlsx
outfile <- 'DATA/DATA_SET_TMP.xlsx'
write.xlsx(data, outfile)

# Read our written data back from the Excel file
data.reread <- read.xlsx(outfile)
dim(data.reread)
head(data.reread)
