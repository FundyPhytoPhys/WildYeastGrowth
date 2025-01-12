---
title: "Wild Yeast Report"
date: "2024-12-03"
author:
- Charlie O'Brien^1^*
- Jacob MacPhee^1^*
- Kenneth McLaughlin^1^*

bibliography: "/Users/jake/Desktop/BIOL-3111/Term Project/Clean Kitchen/WildYeastGrowth/Code/WildYeastLibrary.bib"
output:
  bookdown::html_document2:
    code_folding: hide
    keep_md: yes
    toc: true
    toc_float: true
    toc_depth: 6
    fig_caption: true
editor_options:
  markdown:
    wrap: 72
---
<style type="text/css">
p.caption {
  font-size: 12px;
}
</style>

# AFFILIATIONS {-}

^1^Mount Allison University, New Brunswick, Canada  

# ACKNOWLEDGEMENTS {-}

^1^Maximilian Berthold and ^1^Douglas A. Campbell

``` r
getwd()
```

```
## [1] "/Users/jake/Desktop/BIOL-3111/Term Project/Clean Kitchen/WildYeastGrowth/Code"
```


# INTRODUCTION 

## Aim and Objectives

- This experiment aims to isolate wild yeast from the bark and leaves of oak trees and the fruit and bark of apple trees. 
- We tested wild yeast strains against commercial yeast for the rate of alcohol production, alcohol tolerance, varied culturing temperatures, and the qualitative observations of strains during isolation.
- We successfully isolated yeast from apple tree bark, oak tree leaves, and brewer yeast. 
- We were not successful in isolating yeast from the apple fruit, oak tree bark and the negative control (air) samples.

## Hypothesis

- We hypothesize that commercial yeast will outperform the wild yeast strains in alcohol production rate and alcohol tolerance [@scholesWildYeastLaboratory2021]. 
- We also hypothesize that commercial yeast will be more viable than the wild yeast strains [@scholesWildYeastLaboratory2021].

## We tested the following:

- Viability (%) of the isolated yeast strains.
- The specific gravity from the brew of each yeast strain isolated.
- Alcohol tolerance of the yeast strains.


# MATERIALS & METHODS

Sampling methods done as instructed by [@sukhvirDevelopmentAppleWine2019], [@kowallikSystematicForestSurvey2016], and [@batistaDevelopmentOptimizationNew2015]

## Photos of plated samples, taking during isolation of colonies.

## AppleTreeBark Yeast Sample Grown at 37 degree Celsius in YPD Broth/Agar (SampleID, 7)
![](/Users/jake/Desktop/BIOL-3111/Term Project/Clean Kitchen/WildYeastGrowth/Code/07.png)

## Brewers Yeast Sample Grown at 37 degree Celsius in YPD Broth/Agar (SampleID, 19)
![](/Users/jake/Desktop/BIOL-3111/Term Project/Clean Kitchen/WildYeastGrowth/Code/19.png)

## OakLeaves Yeast Sample Grown at 37 degree Celsius in YPD Broth/Agar (SampleID, 36)
![](/Users/jake/Desktop/BIOL-3111/Term Project/Clean Kitchen/WildYeastGrowth/Code/36.png)

## Brewers Yeast Sample Grown at 20 degree Celsius in YM Broth/Agar (SampleID, 41)
![](/Users/jake/Desktop/BIOL-3111/Term Project/Clean Kitchen/WildYeastGrowth/Code/41.png)

## Intial R Setup

### Set chunk options
Formatted display of content from .md file on GitHub site.
Upon knitting, figures will be saved to 'Figs/'.


### Load packages



### Import MetaData and Data from GoogleSheets
MetaData in a GoogleSheet is more generic than a project-specific .csv edited and saved locally.

``` r
# gs4_auth()

#Instead of sending a token, googlesheets4 will send an API key. This can be used to access public resources for which no Google sign-in is required. 
googlesheets4::gs4_deauth()

# Define the Google Sheet URL (replace with your actual URL)
sheet_url <- "https://docs.google.com/spreadsheets/d/17dDzASxhWDbVpQFXb201vT2oB0rkyi2h4gG33ArOaDA/edit?usp=sharing"

# Read the Google Sheet into R as a data frame
ATMetaData <- read_sheet(sheet_url)

# View the imported data (optional)
#View(ATMetaData)
```



``` r
# Authenticate with Google Sheets (you only need to do this once)
# gs4_auth()

#Instead of sending a token, googlesheets4 will send an API key. This can be used to access public resources for which no Google sign-in is required. 
googlesheets4::gs4_deauth()

# Define the Google Sheet URL (replace with your actual URL)
sheet_url <- "https://docs.google.com/spreadsheets/d/1HDlxd_bt9I49CQGU18b3h6Df69DG3vQAwbIMZTEdpBQ/edit?usp=sharing"

# Read the Google Sheet into R as a data frame
MetaData <- read_sheet(sheet_url)
#View(MetaData)
```



``` r
# Authenticate with Google Sheets (you only need to do this once)
# gs4_auth()

#Instead of sending a token, googlesheets4 will send an API key. This can be used to access public resources for which no Google sign-in is required. 
googlesheets4::gs4_deauth()

# Define the Google Sheet URL (replace with your actual URL)
sheet_url <- "https://docs.google.com/spreadsheets/d/15Qd3DYkeRX4FCy84Uw9ns9L6Bq6P959csP-opdGvBME/edit?usp=sharing"

# Read the Google Sheet into R as a data frame
SpecificGravityData <- read_sheet(sheet_url)
```



``` r
# Authenticate with Google Sheets (you only need to do this once)
# gs4_auth()

#Instead of sending a token, googlesheets4 will send an API key. This can be used to access public resources for which no Google sign-in is required. 
googlesheets4::gs4_deauth()

# Define the Google Sheet URL (replace with your actual URL)
sheet_url <- "https://docs.google.com/spreadsheets/d/118YF2qZqtC5yCfqyDeRaGQhgm5v8mn6wC_6ae1Rnyik/edit?usp=sharing"

# Read the Google Sheet into R as a data frame
BrothPlateData <- read_sheet(sheet_url)
#View(BrothPlateData)
```



``` r
# Authenticate with Google Sheets (you only need to do this once)
# gs4_auth()

#Instead of sending a token, googlesheets4 will send an API key. This can be used to access public resources for which no Google sign-in is required. 
googlesheets4::gs4_deauth()

# Define the Google Sheet URL (replace with your actual URL)
sheet_url <- "https://docs.google.com/spreadsheets/d/1YN_s-YRdM4j9t_c_l1Z4-Q53KQlURTlDpItovzZNd3g/edit?usp=sharing"

# Read the Google Sheet into R as a data frame
DataDictionnary <- read_sheet(sheet_url)
```

### Merged BrothPlateData with MetaData, via leftjoin().

``` r
MergedBrothPlateData <- BrothPlateData %>%
  left_join(MetaData, by = "SampleID")
#head(MergedBrothPlateData)
```

### Merged SpecificGravityData with MetaData, via leftjoin().

``` r
MergedSpecificGravityData <- SpecificGravityData %>%
  left_join(MetaData, by = "SampleID")
#head(MergedSpecificGravityData)
#View(MergedSpecificGravityData)
```

# RESULTS

# Yeast Viability Analysis


## The percent of living yeast compared to total yeast counts and the culturing conditions of the isolated yeast.

- Viability is the percent of living cells within a given sample.
- AppleTreeBark yeast was found to have the greatest viability, followed by brewers yeast, and OakLeaves yeast.
- We were able to determine a trend for culturing conditions for the brewers yeast, however, were not able to for the wild strains.
- Further analysis should compare the viability of the wild strains, at all the culturing conditions studied, to see if the wild strains follow a similar trend as the brewers yeast.
- YM grown at 37 °C are the optimum culturing conditions for obtaining the greatest yeast viability for the brewers yeast.

``` r
MergedBrothPlateData |>  
filter(!is.na(SampleID)) |>  
filter(!Source %in% c("Air", "Apple", "OakTreeBark")) |>  
ggplot(aes(x = as.factor(Source), y = ViabilityPercent, fill = BrothType, color = BrothType, shape = as.factor(GrowthTempC))) +
geom_point(size = 4) + 
coord_cartesian(ylim = c(0, 100)) +
geom_text(aes(label = round(ViabilityPercent, 1)), vjust = -0.5, hjust = 1.5, size = 3.25, color = "black") +
scale_x_discrete(drop = TRUE) +  
labs(title = "Viability Percent by Source, Broth Type and Temperature", x = "Source",
y = "Viability Percent (%)",
color = "Broth Type",
fill = "Broth Type",
shape = "Growth Temperature (°C)"  # update legend title for GrowthTempC
) +
theme_minimal(base_size = 12) +
theme(strip.text = element_text(size = 10, face = "bold"),
axis.text.x = element_text(angle = 0, hjust = 0.5),
plot.title = element_text(hjust = 0.5, face = "bold", size = 14)
) + scale_shape_discrete(labels = function(x) paste(x, "°C"))  # attach °C to legend items
```

![](Figs/Viability Percent by Sample ID-1.png)<!-- -->


# Analysis of Ethanol Tolerance in Wild and Commerical Yeast Strains 

Methods done as instructed by [@yingIsolationIdentificationTolerance2024].


### Settings for file import of raw ODData

- Note: The .tsv file exported from the Molecular Dynamics software with encoding UTF16LE had embedded null characters that caused problems upon import.
- As a hacked solution, download the .tsv from 'Teams'.
- Import the .tsv into a new GoogleSheet.
- Delete the first two rows of the file.
- Export from GoogleSheet as .csv.
- Move the .csv to the .Rproj folder.
- A better way would be code to read the problematic UTF16LE file with null characters.
- The issue relates to a complex file structure exported from Molecular Dynamics software.

``` r
Project <- "WildYeast"

#set variables for file import & processing
DataPath <- file.path("..", "Data", "RawData", fsep = .Platform$file.sep)
file_id <- ".csv"

# DataOneDrive <- "~/OneDrive - Mount Allison University/BIOL2201_2024/StudentDataTest"
```

### List Data file(s) before reading in raw ODData

``` r
DataFiles <- list.files(path = DataPath, pattern = file_id, full.names = TRUE)

DataFiles
```

```
## [1] "../Data/RawData/24102024_BIOL3111_YeastGroup.csv"
```

``` r
FileEncodeMD <- as.character(guess_encoding(file = DataFiles[1])[1,1])
FileEncodeMD
```

```
## [1] "UTF-8"
```

### Read ODData file(s) into form readable by R

``` r
ODData <- readr::read_csv(file =  DataFiles[1])

#head(ODData)


#data.table::fread(text = readLines(DataFiles[1], skipNul = T))
```



``` r
#code for reading multiple files, if needed
# read_delim_plus <- function(flnm, delimiter, headerrows, fileencode){read_delim(flnm, delim = delimiter,  col_names = TRUE,  skip = headerrows, escape_double = FALSE,  locale = locale(encoding = fileencode), trim_ws = TRUE) |>
#     mutate(Filename = flnm)
#   }
# 
# ODData <- DataFiles |>
#   map_df(~read_delim_plus(flnm = ., delimiter = DelimiterMD,  headerrows = HeaderRowsMD,  fileencode = FileEncodeMD))
# 
# head(ODData)
```

### Molecular Dynamics exports problematic variable names. In this case, 'temperature' had to be renamed.

``` r
#colnames(ODData)
```


``` r
# Rename column using pattern matching
ODData <- ODData %>%
  rename_with(~ gsub("Temperature.*", "TempC", .x))  # Replace any name starting with "Temperature"
```



``` r
#head(ODData)
```

### Labels for OD680 & OD720 were lost in hacked import; in this chunk, they were regenerated.

``` r
#remove blank rows by filtering out rows where Temp_C is NA
ODData <- ODData |>
  filter(!is.na(TempC))

#this is a hack solution, knowing there are only 15 rows of data
#better way would be to keep the OD settings during import
ODData <- ODData |>
  mutate(nm = ifelse(row_number() %in% c(1:15), 680, 720), .before = Time)

#head(ODData)
```


### For plotting, it is easier to have data in a 'long' format, where the Well becomes a value of a variable rather than a separate variable for each well.

``` r
ODDataLong <- pivot_longer(ODData, cols = -c(nm, Time, TempC), names_to = "Well", values_to = "OD")

#ODDataLong |>
  #ggplot() +
  #geom_point(aes(x = Time, y = OD)) +
  #facet_grid(cols = vars(nm))

#head(ODDataLong)
```


### ODData was saved in the long format as a 'rds' file,

``` r
saveRDS(ODDataLong, file = file.path("..", "Data", "ProcessedData", "ODDataLong.rds"))
```

### Converted time from h:m:s format to numeric format (1,2,3,4,5,6,7,8 etc.). This is necessary for ethanol tolerance analyses.

``` r
ODDataLong <- ODDataLong %>% 
  mutate(Time_numeric = as.numeric(hms(Time)) / 3600)  # Converts to hours
```

### Merge ODDataLong with Metadata before attempting data analyses.

``` r
# Merge ODData with ATMetaData
Merged_ATData <- ODDataLong %>%
  left_join(ATMetaData, by = "Well")
#View(Merged_ATData)
```

### Filter wells that had no media/inoculant to avoid problems in analysis.

``` r
# Exclude wells G10, G11, G12, H10, H11, H12
Merged_ATData <- Merged_ATData %>%
  filter(!Well %in% c("G10", "G11", "G12", "H10", "H11", "H12"))

# Verify
# table(Merged_ATData$Well) 
```

### Filter merged data by wavelength inorder to make  growth curves for OD720 and OD680.

``` r
# Filter data for OD 680 nm
Merged_ATData_680 <- Merged_ATData %>%
  filter(nm == 680)

# Filter data for OD 720 nm
Merged_ATData_720 <- Merged_ATData %>%
  filter(nm == 720)
```

### Double check data is in time numeric form for analyses.

``` r
Merged_ATData <- Merged_ATData %>% 
  mutate(Time_hour = as.numeric(hms(Time)) / 3600)  # Converts to hours
```

### Verify structure prior to spline test for MuMax, problematic structure, will cause problems.

``` r
# Verify dataset structure
# str(Merged_ATData)  
# head(Merged_ATData) 
```

## The change in absorbance at 680 nm over 14 hours of each stain when exposed to varying ethanol concentrations of 2 – 12 % (v/v).

``` r
Merged_ATData <- Merged_ATData %>%
mutate(EthanolConcentrationLabel = factor(paste0(EthanolConcentration, "% (v/v)"), levels = paste0(sort(unique(EthanolConcentration)), "% (v/v)")))

Merged_ATData |>
filter(nm == 680) |>
ggplot(aes(x = Time_hour, y = OD, color = BrothType, group = interaction(Well, BrothType, GrowthTempC))) +
geom_line() +
geom_point() +
facet_grid(
rows = vars(EthanolConcentrationLabel), cols = vars(as.factor(Source))) +
theme_minimal() +
theme(text = element_text(size = 12), legend.position = "bottom", strip.text.y = element_text(angle = 0) # facet labels for rows
) +
scale_shape_manual(
values = c(17, 16), # shapes for the legend
labels = paste0(unique(Merged_ATData$GrowthTempC), "°C")
) +
labs(
y = "Optical Density (OD) at 680 nm",
x = "Time (hours)",
shape = "Temperature", # legend title
color = "Broth Type"
)
```

![](Figs/Growth Curves at OD680-1.png)<!-- -->

## The change in absorbance at 720 nm over 14 hours of each stain when exposed to varying ethanol concentrations of 2 – 12 % (v/v).


``` r
Merged_ATData <- Merged_ATData %>%
mutate(EthanolConcentrationLabel = factor(paste0(EthanolConcentration, "% (v/v)"), levels = paste0(sort(unique(EthanolConcentration)), "% (v/v)")))

Merged_ATData |>
filter(nm == 720) |>
ggplot(aes(x = Time_hour, y = OD, color = BrothType, group = interaction(Well, BrothType, GrowthTempC))) +
geom_line() +
geom_point() +
facet_grid(
rows = vars(EthanolConcentrationLabel), cols = vars(as.factor(Source))) +
theme_minimal() +
theme(text = element_text(size = 12), legend.position = "bottom", strip.text.y = element_text(angle = 0) # facet labels for rows
) +
scale_shape_manual(
values = c(17, 16), # shapes for the legend
labels = paste0(unique(Merged_ATData$GrowthTempC), "°C")
) +
labs(
y = "Optical Density (OD) at 720 nm",
x = "Time (hours)",
shape = "Temperature", # legend title
color = "Broth Type"
)
```

![](Figs/Growth Curves at OD720-1.png)<!-- -->

### Spline fitting for determining maximum growth rate (MuMax (hours)) and R-squared for all tested conditions and yeast strains (ie. wells).

``` r
# Step 2: Spline Fitting and Parameter Extraction
SplineRates_nest <- Merged_ATData |> 
  group_by(nm, Well, Source, SampleID, EthanolConcentration, BrothType, GrowthTempC) |> 
  nest() |> 
  mutate(SplineFit = purrr::map(data, ~tryCatch(
      growthrates::fit_spline(.$Time_hour, .$OD),  # Fit spline
      error = function(e) NULL  # Handle errors gracefully
    )), # Fit spline with error handling
Mumax_hour = purrr::map_dbl(SplineFit, ~if (!is.null(.)) pluck(., "par", "mumax") else NA_real_),  
    rsquared = purrr::map_dbl(SplineFit, ~if (!is.null(.)) pluck(., "rsquared") else NA_real_))|> 
  ungroup()

SplineRatesResults <- SplineRates_nest |> 
  select(nm, Well, Mumax_hour, rsquared, Source, SampleID, EthanolConcentration, BrothType, GrowthTempC)

# View the results
# print(SplineRatesResults)
```

### Cleaned the resulting "SplineRatesResults", before plotting MuMax (hours) vs EtOH concentration (% v/v).

``` r
# Remove rows with any NA values
SplineRatesResults <- na.omit(SplineRatesResults)

# Check the result
# head(SplineRatesResults)

#View(SplineRatesResults)
```

### Fit linear models from cleaned "SplineRatesResults".

``` r
#define exponential decay function for data fitting.
#exp_decay <- function(x, i, mu){y = i * exp(mu * x)}

#need to merge left_join(Merged_ATMetaData_MuMax_clean, true MetaData so you can talk about growth responses to EtOH in a sensible manner
 # left_join(Merged_ATMetaData_MuMax_clean, 

MuEtOH_nest <- SplineRatesResults |>
  nest(.by = c("SampleID", "nm", "Source", "BrothType", "GrowthTempC")) |> #want to nest by source
  mutate(LinearFit = purrr::map(data, ~lm(Mumax_hour ~ EthanolConcentration,
                                            data = .x)),
         LinearTidy = purrr::map(LinearFit, tidy),
         LinearParam = purrr::map(LinearFit, glance),
         LinearPredict = purrr::map(LinearFit, augment))
```
 
## The relationship between MuMax (hours) ethanol concentration (% v/v) of the isolated yeast strains, faceted by source and culturing conditions.

- The wild yeast strains appeared to experience growth inhibition at 2% (v/v) ethanol, where no major growth was observed at the other ethanol concentrations.
- It is also plausible that the wild strains experienced a lag phase longer than the 15 hour range of the ethanol tolerance study, which would correlate to no major growth.
- For the brewers yeast, it is clear that as the ethanol concentration increased, the lag phase also increased.
- While the brewers yeast experienced an increased lag phase as ethanol concentration increased, all samples were still able to grow in the higher ethanol concentrations.


``` r
#Set label for GrowthTempC
temp_labeller <- function(value) {
  paste(value, "°C")
}
nm_labeller <- function(value) {
  paste(value, "nm")
}
#Plot Linear model
MuEtOH_nest |>
  unnest(cols = c(LinearPredict)) |>
  ggplot() +
  geom_point(aes(x = EthanolConcentration, y = Mumax_hour)) +
  geom_line(aes(x = EthanolConcentration , y = .fitted)) +
  #geom_point(aes(x = Ethanol (v/v %), y = .resid), colour = "red") +
  facet_grid(cols = vars(Source, BrothType, GrowthTempC), rows = vars(nm), labeller = labeller(GrowthTempC = temp_labeller , nm = nm_labeller)) +
   labs(x = "Ethanol Concentration % (v/v)") +
  theme_bw()
```

![](Figs/plot mumax vs EtOh-1.png)<!-- -->

### Show fit parameters.

- These parameters show that the relationships plotted above are statically significant (very small p-values).

``` r
MuEtOH_nest |>
unnest(cols = c(LinearTidy)) |>
 select(-c(data, LinearFit, LinearParam, LinearPredict)) |>
  select(-c(statistic)) |>
  pivot_wider(id_cols = c(SampleID, nm), names_from = term, values_from = c(estimate, std.error, p.value)) |>
  kable()
```



| SampleID|  nm| estimate_(Intercept)| estimate_EthanolConcentration| std.error_(Intercept)| std.error_EthanolConcentration| p.value_(Intercept)| p.value_EthanolConcentration|
|--------:|---:|--------------------:|-----------------------------:|---------------------:|------------------------------:|-------------------:|----------------------------:|
|       19| 680|            0.2030007|                    -0.0078177|             0.0064375|                      0.0008022|           0.0000000|                    0.0000002|
|       36| 680|            0.0011525|                     0.0001520|             0.0035337|                      0.0004403|           0.7495071|                    0.7355055|
|       18| 680|            0.2551484|                    -0.0065962|             0.0344146|                      0.0042884|           0.0000051|                    0.1479938|
|       43| 680|            0.4995635|                    -0.0277691|             0.0457810|                      0.0057048|           0.0000001|                    0.0003073|
|       41| 680|            0.4418388|                    -0.0212349|             0.0541076|                      0.0067424|           0.0000018|                    0.0076797|
|        7| 680|           -0.0088181|                     0.0025696|             0.0062424|                      0.0007779|           0.1812614|                    0.0057114|
|       19| 720|            0.2376157|                    -0.0089990|             0.0060420|                      0.0007529|           0.0000000|                    0.0000000|
|       36| 720|           -0.0008505|                     0.0006321|             0.0052433|                      0.0006534|           0.8736381|                    0.3509783|
|       18| 720|            0.2174016|                    -0.0062407|             0.0150228|                      0.0018720|           0.0000000|                    0.0053870|
|       43| 720|            0.3732816|                    -0.0170660|             0.0434841|                      0.0054186|           0.0000010|                    0.0076787|
|       41| 720|            0.3378377|                    -0.0151847|             0.0441729|                      0.0055044|           0.0000036|                    0.0162675|
|        7| 720|           -0.0070542|                     0.0021570|             0.0048503|                      0.0006044|           0.1695555|                    0.0034301|



``` r
# Find initial Specific Gravity (SG) at Time = 0
#initial_sg_values <- MergedSpecificGravityData %>%
#filter(Time == 0) 
#%>%
#select(SampleID, SpecificGravity) %>%
#rename(InitialSG = SpecificGravity)

# Find final SG for each SampleID
#final_sg_values <- MergedSpecificGravityData %>%
#group_by(SampleID) %>%
#summarize(FinalSG = min(SpecificGravity, na.rm = TRUE), .groups = "drop")

# merge initial SG and final SG into a single dataset
#combined_sg_values <- merge(initial_sg_values, final_sg_values, by = "SampleID")

# calculate ABV 
#combined_sg_values <- combined_sg_values %>%
#mutate(ABV = (InitialSG - FinalSG) * 131.25)
#print(combined_sg_values)
#View(combined_sg_values)
```


# Analysis of Fermentation Capabilities Between Wild and Commercial Yeast Strains.


## The change in specific gravity during the 7-day brewing process of each yeast strain and the culturing conditions of the isolated yeast. 

- Lag phase of 4 days for the yeast from apple tree bark, oak leaves 3 days and 1 day for the brewer yeast.
- Brewer yeast is the only stain which appears to have reached the end point of the brew.
- Culturing conditions appear to have little to no effect on fermenting capabilities.


``` r
ggplot(MergedSpecificGravityData, 
aes(x = Time, y = SpecificGravity, color = BrothType, shape = as.factor(GrowthTempC))) +
geom_line(size = 1) + 
geom_point(size = 2) + 
coord_cartesian(ylim = c(0, 1.5)) +
facet_grid(cols = vars(Source)) +  # facet by Source
labs(x = "Time (hours)", 
y = "Specific Gravity", 
color = "Broth Type", 
shape = "Growth Temperature (°C)",  # legend title for GrowthTempC
title = "Specific Gravity Over Time by Source") +
theme_minimal(base_size = 12) +
theme(plot.title = element_text(hjust = 0.5, face = "bold", size = 14), 
strip.text = element_text(size = 10, face = "bold"), 
axis.text.x = element_text(angle = 0, hjust = 1)) +
scale_shape_discrete(labels = function(x) paste(x, "°C"))  # attach °C to legend items
```

![](Figs/Specific Gravity Over Time by Source-1.png)<!-- -->



``` r
# Calculate rate of change in Specific Gravity
# MergedSpecificGravityData <- MergedSpecificGravityData %>%
# group_by(SampleID) %>%
# mutate(SGRate = c(NA, diff(SpecificGravity) / diff(Time)))
# Filter missing values to allow connected lines
# MergedSpecificGravityData <- MergedSpecificGravityData %>%
# filter(!is.na(SGRate) & !is.na(Time))

# Plot with facets by Source and colors by SampleID
# ggplot(MergedSpecificGravityData, aes(x = Time, y = SGRate, color = as.factor(SampleID), group = SampleID)) +
# geom_line(size = 1, na.rm = TRUE) + 
# geom_point(size = 2) +  
# facet_grid(cols = vars(Source)) +  # Facet by Source
# labs(x = "Time",
# y = "Specific Gravity Rate of Change",
# color = "Sample ID",  
# title = "Rate of Change of Specific Gravity Over Time by Source") +
# theme_minimal(base_size = 12) +
# theme(plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
# strip.text = element_text(size = 10, face = "bold"),
# axis.text.x = element_text(angle = 0, hjust = 1))
```



``` r
# Calculate completion time for each SampleID
# completion_times <- MergedSpecificGravityData %>%
# group_by(SampleID, Source) %>%  # Include Source for grouping
# filter(SpecificGravity == min(SpecificGravity, na.rm = TRUE)) %>%
# summarize(Completion_Time = min(Time), .groups = "drop")

# Plot of Completion Times without facets
# ggplot(completion_times, aes(x = factor(SampleID), y = Completion_Time, color = Source)) +
# geom_point(size = 4) +
# geom_text(aes(label = round(Completion_Time, 1)), vjust = -0.75, hjust = 0.75, size = 5, color = "black") +
# labs(title = "Completion Time by Source and Sample ID",x = "Sample ID", y = "Completion Time (hours)", color = "Source") +
# theme_minimal(base_size = 12) +
# theme(plot.title = element_text(hjust = 0.5, face = "bold", size = 14), strip.text = element_text(size = 10, face = "bold"), axis.text.x = element_text(angle = 45, hjust = 1),
# legend.position = "right")
```


## The percent of sugar consumed referred to the apparent attenuation by the yeast strains from the initial malt, facted by source and culturing conditions.

- Finding out the attenuation of the sample this will identify the efficiency of the yeast strain by comparing the sugar content in the initial wort to the amount of sugars left over giving a percent of sugars consumed and converted to alcohol or CO2.
- Culturing conditions have little to no effect on the amount of sugars consumed.
- Brewer yeast consumed significantly more sugar due to the prolonged lag phase of apple tree bark and oak leaf yeast, and the brew had not reached completion.


``` r
# Calculate Apparent Attenuation for Each Sample
AttenuationData <- MergedSpecificGravityData %>%
group_by(Source , BrothType , GrowthTempC) %>%  # Include Source for grouping
summarize(InitialSG = max(SpecificGravity, na.rm = TRUE),  # Initial SG is the max
FinalSG = min(SpecificGravity, na.rm = TRUE),    # Final SG is the min
.groups = "drop") %>%
mutate(ApparentAttenuation = ((InitialSG - FinalSG) / (InitialSG - 1)) * 100)

# Plot Apparent Attenuation Data by Source
ggplot(AttenuationData, aes(x = Source, y = ApparentAttenuation, color = BrothType, shape = as.factor(GrowthTempC))) +
geom_point(size = 4) +
coord_cartesian(ylim = c(0, 100)) +
geom_text(aes(label = round(ApparentAttenuation, 1)), vjust = -0.5, hjust = 1.5, size = 3.25, color = "black") +
labs(title = "Apparent Attenuation (%) by Source, Broth Type and Temperature", x = "Source", y = "Apparent Attenuation (%)", color = "Broth Type",
shape = "Growth Temperature (°C)"  # update legend title for GrowthTempC
) +
theme_minimal(base_size = 12) +
theme(plot.title = element_text(hjust = 0.5, face = "bold", size = 14), strip.text = element_text(size = 10, face = "bold"),
axis.text.x = element_text(angle = 0, hjust = 0.5),
legend.position = "right") + 
scale_shape_discrete(labels = function(x) paste(x, "°C"))  # attach °C to legend items
```

![](Figs/Apparent Attenuation by Source-1.png)<!-- -->

# SUMMARY AND DISCUSSION

- We successfully isolated yeast from apple tree bark, oak tree leaves, and brewer yeast. 
- We were not successful in isolating yeast from the apple fruit, oak tree bark and the negative control (air) samples.

## We tested the following:

- Viability (%) of the isolated yeast strains.
- The specific gravity from the brew of each yeast strain isolated.
- Alcohol tolerance of the yeast strains.
- In varying culturing conditions (YM/YPD and 20/37 degree Celsius)


## Our findings partially support our initial hypotheses:

### Initial Hypothesis: Commercial yeast will outperform the wild yeast strains in alcohol production rate and alcohol tolerance [@scholesWildYeastLaboratory2021]. 

- They had a lower lag phase, however, since the wild strains were not allowed to reach completion we cannot fully compare alcohol production rates.
- The wild yeast strains appeared to experience growth inhibition at 2% (v/v) ethanol, where no major growth was observed at the other ethanol concentrations.
- It is also plausible that the wild strains experienced a lag phase longer than the 15 hour range of the ethanol tolerance study, which would correlate to no major growth.
- For the brewers yeast, it is clear that as the ethanol concentration increased, the lag phase also increased.
- While the brewers yeast experienced an increased lag phase as ethanol concentration increased, all samples were still able to grow in the higher ethanol concentrations.

### Initial Hypothesis: Commercial yeast will be more viable than the wild yeast strains [@scholesWildYeastLaboratory2021].

- AppleTreeBark yeast was found to have the greatest viability, followed by brewers yeast, and OakLeaves yeast.
- We were able to determine a trend for culturing conditions for the brewers yeast, however, were not able to for the wild strains.
- Further analysis should compare the viability of the wild strains, at all the culturing conditions studied, to see if the wild strains follow a similar trend as the brewers yeast.
- YM grown at 37 °C are the optimum culturing conditions for obtaining the greatest yeast viability for the brewers yeast.

# BIBLIOGRAPHY









