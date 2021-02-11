---
title: "data_analysis_project"
author: "Vaneck Fokou"
date: "10/02/2021"
output: html_document
---



### The Data

The American Time Use Survey (ATUS) is a time-use survey of Americans, which is sponsored by the Bureau of Labor Statistics (BLS) and conducted by the U.S. Census Bureau. Respondents of the survey are asked to keep a diary for one day carefully recording the amount of time they spend on various activities including working, leisure, childcare, and household activities. The survey has been conducted every year since 2003.

Included in the data are main demographic variables such as respondents' age, sex, race, marital status, and education. The data also includes detailed income and employment information for each respondent. While there are some slight changes to the survey each year, the main questions asked stay the same. You can find the data dictionaries for each year on [https://www.bls.gov/tus/dictionaries.htm](https://www.bls.gov/tus/dictionaries.htm)


### Accessing the Data

There are multiple ways to access the ATUS data; however, for this project, you'll get the raw data directly from the source. The data for each year can be found at [https://www.bls.gov/tus/#data](https://www.bls.gov/tus/#data). Once there, there is an option of downloading a multi-year file, which includes data for all of the years the survey has been conducted, but **for the purposes of this project, let's just look at the data for 2016**. Under **Data Files**, click on `American Time Use Survey--2016 Microdata files`. 

You will be brought to a new screen. Scroll down to the section **2016 Basic ATUS Data Files**. Under this section, you'll want to **click to download** the following two files: `ATUS 2016 Activity summary file (zip)` and `ATUS-CPS 2016 file (zip)`. 

* `ATUS 2016 Activity summary file (zip)` contains information about the total time each ATUS respondent spent doing each activity listed in the survey. The activity data includes information such as activity codes, activity start and stop times, and locations.
* `ATUS-CPS 2016 file (zip)` contains information about each household member of all individuals selected to participate in the ATUS.

Once they've been downloaded, you'll need to **unzip the files**. Once unzipped, you will see the dataset in a number of different file formats including `.sas`, `.sps`, and `.dat` files. **We'll be working with the .dat files.**

### Loading the Data into R

Use the first approach explained above to download and access the ATUS data for 2016. Download the CPS and Activity Summary files in a folder and unzip them and within each folder upload the files ending in .dat to `data/raw_data` filder on RStudio.cloud. To load the data in, **run the code in the `atus-data` code chunk** to create an object called `atus.all`.

### Importing data


```r
atus.cps <- read.delim('data/raw_data/atuscps_2016.dat', sep=",")
atus.sum <- read.delim('data/raw_data/atussum_2016.dat', sep=",")
atus.all <- atus.sum %>%
  left_join(atus.cps %>% filter(TULINENO==1), by = c("TUCASEID"))
```

### Exploratory Analysis of Child Care Data


```r
### Size of the dataset
dim(atus.all)
```

```
## [1] 10493   798
```

```r
### Viewing the first rows of the dataset
head(atus.all)
```

```
##      TUCASEID TUFINLWGT TRYHHCHILD TEAGE TESEX PEEDUCA.x PTDTRACE.x PEHSPNON.x
## 1 2.01601e+13  24588650         -1    62     2        39          1          2
## 2 2.01601e+13   5445941         -1    69     1        37          2          2
## 3 2.01601e+13   8782622          0    24     2        39          2          2
## 4 2.01601e+13   3035910          8    31     2        40          1          2
## 5 2.01601e+13   6978586         -1    59     2        39          1          2
## 6 2.01601e+13   5191610          4    16     2        36          3          1
##   GTMETSTA.x TELFS TEMJOT TRDPFTPT TESCHENR TESCHLVL TRSPPRES TESPEMPNOT
## 1          1     5     -1       -1       -1       -1        1          2
## 2          2     5     -1       -1       -1       -1        1          2
## 3          1     5     -1       -1        2       -1        3         -1
## 4          2     1      2        2        2       -1        3         -1
## 5          1     1      1        2       -1       -1        1          2
## 6          1     5     -1       -1        1        1        3         -1
##   TRERNWA TRCHILDNUM TRSPFTPT TEHRUSLT TUDIARYDAY TRHOLIDAY TRTEC TRTHH t010101
## 1      -1          0       -1       -1          6         0    -1     0     690
## 2      -1          0       -1       -1          1         0    30     0     600
## 3      -1          2       -1       -1          1         0    -1   380     940
## 4   46944          3       -1       32          1         0    -1   705     635
## 5   30250          0       -1       12          1         0    -1     0     500
## 6      -1          4       -1       -1          1         0    -1     0     565
##   t010102 t010201 t010299 t010301 t010399 t010401 t010499 t020101 t020102
## 1       0      25       0       0       0       0       0      75       6
## 2       0      20       0       0       0       0       0      60       0
## 3       0     120       0       0       0       0       0       0       0
## 4       0      20       0       0       0       0       0      20      50
## 5       0      80       0       0       0       0       0      30      25
## 6       0      55       0       0       0       0       0       0       0
##   t020103 t020104 t020199 t020201 t020202 t020203 t020299 t020301 t020302
## 1       0       0       0      50       0      45       0       0       0
## 2       0       0       0     150       0       0       0       0       0
## 3       0      30       0      75       0       0       0       0       0
## 4      65      60       0      90       0      50       0      60       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0      90      10       0       0       0       0
##   t020303 t020399 t020401 t020402 t020499 t020501 t020502 t020601 t020602
## 1       0       0       0       0       0       0       0       6       8
## 2       0       0      20       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0     145       0
## 6       0       0       0       0       0       0       0       0       0
##   t020699 t020701 t020799 t020801 t020899 t020901 t020902 t020903 t020904
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0      10      25      15       0
## 6       0       0       0       0       0       0       0       0       0
##   t020905 t020999 t029999 t030101 t030102 t030103 t030104 t030105 t030106
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0      10      20       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t030108 t030109 t030110 t030111 t030112 t030199 t030201 t030202 t030203
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4      30       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t030204 t030299 t030301 t030302 t030303 t030399 t030401 t030402 t030403
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t030404 t030405 t030499 t030501 t030502 t030503 t030504 t030599 t039999
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t040101 t040102 t040103 t040104 t040105 t040106 t040108 t040109 t040110
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t040111 t040112 t040199 t040201 t040202 t040299 t040301 t040302 t040303
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t040399 t040401 t040402 t040403 t040404 t040405 t040499 t040501 t040502
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t040503 t040504 t040505 t040506 t040507 t040508 t040599 t049999 t050101
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t050102 t050103 t050104 t050199 t050201 t050202 t050205 t050299 t050301
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t050302 t050303 t050304 t050305 t050399 t050401 t050403 t050404 t050499
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t060101 t060102 t060103 t060104 t060199 t060201 t060202 t060299 t060301
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t060302 t060303 t060399 t060401 t060403 t060499 t069999 t070101 t070102
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0      60       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0      15
## 6       0       0       0       0       0       0       0       0       0
##   t070103 t070104 t070105 t070199 t070201 t079999 t080101 t080102 t080201
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       3       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t080202 t080203 t080299 t080301 t080302 t080401 t080402 t080403 t080499
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t080501 t080502 t080599 t080601 t080602 t080701 t080702 t080801 t089999
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t090101 t090102 t090103 t090104 t090199 t090201 t090202 t090301 t090401
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t090501 t090502 t090599 t099999 t100101 t100102 t100103 t100199 t100201
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t100304 t100305 t100399 t109999 t110101 t110201 t110299 t120101 t120201
## 1       0       0       0       0      40       0       0     115       0
## 2       0       0       0       0      30       0       0       0       0
## 3       0       0       0       0      75       0       0       0       0
## 4       0       0       0       0     165       0       0       0       0
## 5       0       0       0       0      30       0       0     107       0
## 6       0       0       0       0     120       0       0      45       0
##   t120202 t120299 t120301 t120302 t120303 t120304 t120305 t120306 t120307
## 1       0       0       0       0     350       0       0       0       0
## 2       0       0      60       0     470       0       0       0       0
## 3       0       0       0       0      20       0       0       0       0
## 4       0       0       0       0     120       0       0       0       0
## 5       0       0       0       0      70       0       0       0       0
## 6       0       0       0       0     290       0       0      20       0
##   t120308 t120309 t120310 t120311 t120312 t120313 t120399 t120401 t120402
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0      30       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t120403 t120404 t120405 t120499 t120501 t120502 t120503 t120504 t130101
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t130102 t130103 t130104 t130105 t130106 t130107 t130108 t130109 t130110
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0      20       0
##   t130112 t130113 t130114 t130116 t130117 t130118 t130119 t130120 t130121
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t130122 t130124 t130125 t130126 t130127 t130128 t130129 t130130 t130131
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0      30
##   t130132 t130133 t130134 t130135 t130136 t130199 t130202 t130203 t130205
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t130206 t130207 t130210 t130212 t130213 t130216 t130217 t130218 t130223
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t130224 t130225 t130226 t130227 t130229 t130231 t130232 t130299 t130301
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t130302 t139999 t140101 t140102 t140103 t140105 t149999 t150101 t150102
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0      50       0      10       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t150103 t150104 t150105 t150106 t150199 t150201 t150202 t150203 t150204
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0      40
## 6       0       0       0       0       0       0       0       0       0
##   t150299 t150301 t150302 t150399 t150401 t150402 t150499 t150501 t150599
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t150601 t150602 t150699 t150701 t150801 t159999 t160101 t160102 t160103
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0      45       0       0
## 5       0       0       0       0       0      90     120       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t160104 t160105 t160106 t160107 t160108 t160199 t160201 t169999 t180101
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t180201 t180202 t180203 t180204 t180205 t180206 t180207 t180208 t180209
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t180299 t180301 t180302 t180303 t180304 t180305 t180399 t180401 t180402
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t180403 t180404 t180405 t180499 t180501 t180502 t180503 t180504 t180601
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t180602 t180603 t180604 t180699 t180701 t180702 t180703 t180704 t180799
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0      60       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0      30       0      20       0
## 6       0       0       0       0       0       0       0       0       0
##   t180801 t180802 t180803 t180804 t180805 t180806 t180807 t180899 t180901
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0       0       0       0       0
##   t180902 t180903 t180904 t180905 t180999 t181001 t181002 t181099 t181101
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0      15
## 6       0       0       0       0       0       0       0       0       0
##   t181201 t181202 t181203 t181204 t181205 t181301 t181302 t181399 t181401
## 1      30       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0       0       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0      10
## 6      15       0       0       0       0      10       0       0       0
##   t181499 t181501 t181599 t181601 t181801 t189999 t500101 t500103 t500105
## 1       0       0       0       0       0       0       0       0       0
## 2       0       0       0       0       0       0       0       0       0
## 3       0       0       0       0       0       0      60       0       0
## 4       0       0       0       0       0       0       0       0       0
## 5       0       0       0       0       0       0       0       0       0
## 6       0       0       0       0       0      10     160       0       0
##   t500106 t500107 TULINENO GEREG GEDIV GESTFIPS GTMETSTA.y GTCBSA GTCO HEFAMINC
## 1       0       0        1     3     5       13          1  12060   15        3
## 2       0       0        1     3     5       51          2      0    0        6
## 3       0       0        1     3     5       11          1  47900    1        4
## 4       0       0        1     2     3       26          2      0    0        8
## 5       0       0        1     2     4       29          1      0    0       13
## 6       0       0        1     3     5       12          1  33100   86        5
##   HEHOUSUT HEPHONEO HETELAVL HETELHHD HETENURE       HRHHID HRHHID2 HRHTYPE
## 1        1        0       -1        1        1 3.509403e+14    3011       1
## 2        1        0       -1        1        2 2.025055e+14    3011       1
## 3        1        0       -1        1        2 1.243705e+14    3111       4
## 4        1        0       -1        1        2 2.041305e+14    3011       7
## 5        1        0        2        2        1 2.203965e+13    3011       1
## 6        1        0       -1        1        2 1.021017e+13    3011       4
##   HRINTSTA HRLONGLK HRMIS HRMONTH HRNUMHOU HRYEAR4 HUBUS HUBUSL1 HUBUSL2
## 1        1        2     8      11        3    2015     2      -1      -1
## 2        1        2     8      11        2    2015     2      -1      -1
## 3        1        2     8      11        4    2015     2      -1      -1
## 4        1        2     8      11        1    2015     2      -1      -1
## 5        1        2     8      11        2    2015     1       1      -1
## 6        1        2     8      11        5    2015     1       1      -1
##   HUBUSL3 HUBUSL4 HUFINAL HUINTTYP HUPRSCNT HURESPLI HUTYPB HUTYPC HUTYPEA
## 1      -1      -1     201        1        1        1     -1     -1      -1
## 2      -1      -1     201        1        1        1     -1     -1      -1
## 3      -1      -1     201        1        2        1     -1     -1      -1
## 4      -1      -1     201        1        1        1     -1     -1      -1
## 5      -1      -1     201        1        1        1     -1     -1      -1
## 6      -1      -1     201        2        0        1     -1     -1      -1
##   HXFAMINC HXHOUSUT HXPHONEO HXTELAVL HXTELHHD HXTENURE OCCURNUM PEABSPDO
## 1       23        0        0        1        0        0        1       -1
## 2        0        0        0        1        0        0        2       -1
## 3        0        0        0        1       41        0        2       -1
## 4        0        0        0        1        0        0        1       -1
## 5       23        0        0        0        0        0        2       -1
## 6        0        0        0        1        0        0        2       -1
##   PEABSRSN PEAFEVER PEAFNOW PEAFWHN1 PEAFWHN2 PEAFWHN3 PEAFWHN4 PECOHAB PECYC
## 1       -1        2       2       -1       -1       -1       -1      -1    -1
## 2       -1        2       2       -1       -1       -1       -1      -1    -1
## 3       -1        2       2       -1       -1       -1       -1      -1    -1
## 4       -1        2       2       -1       -1       -1       -1      -1     3
## 5       -1        2       2       -1       -1       -1       -1      -1    -1
## 6       -1       -1       2       -1       -1       -1       -1      -1    -1
##   PEDADTYP PEDIPGED PEDISDRS PEDISEAR PEDISEYE PEDISOUT PEDISPHY PEDISREM
## 1       -1        1        2        2        2        2        2        2
## 2       -1       -1        2        2        2        2        1        2
## 3       -1        2        2        2        2        2        2        2
## 4       -1       -1        2        2        2        2        2        2
## 5       -1        1        2        2        2        2        2        2
## 6       -1       -1        2        2        2        2        2        2
##   PEDW4WK PEDWAVL PEDWAVR PEDWLKO PEDWLKWK PEDWRSN PEDWWK PEDWWNTO PEEDUCA.y
## 1      -1      -1      -1      -1       -1      -1     -1       -1        39
## 2      -1      -1      -1      -1       -1      -1     -1       -1        37
## 3      -1      -1      -1      -1       -1      -1     -1        2        39
## 4      -1      -1      -1      -1       -1      -1     -1       -1        40
## 5      -1      -1      -1      -1       -1      -1     -1       -1        39
## 6      -1      -1      -1      -1       -1      -1     -1        2        36
##   PEERN PEERNCOV PEERNH1O PEERNH2 PEERNHRO PEERNHRY PEERNLAB PEERNPER PEERNRT
## 1    -1       -1       -1      -1       -1       -1       -1       -1      -1
## 2    -1       -1       -1      -1       -1       -1       -1       -1      -1
## 3    -1       -1       -1      -1       -1       -1       -1       -1      -1
## 4    -1        2     1500      -1       40        1        2        1      -1
## 5    -1        2     4500      -1       30        1        2        1      -1
## 6    -1       -1       -1      -1       -1       -1       -1       -1      -1
##   PEERNUOT PEERNWKP PEFNTVTY PEHGCOMP PEHRACT1 PEHRACT2 PEHRACTT PEHRAVL
## 1       -1       -1       57       -1       -1       -1       -1      -1
## 2       -1       -1       57       -1       -1       -1       -1      -1
## 3       -1       -1       57        2       -1       -1       -1      -1
## 4        2       -1       57       -1       40       -1       40      -1
## 5        2       -1       57       -1       30       25       55      -1
## 6       -1       -1      370       -1       -1       -1       -1      -1
##   PEHRFTPT PEHRRSN1 PEHRRSN2 PEHRRSN3 PEHRUSL1 PEHRUSL2 PEHRUSLT PEHRWANT
## 1       -1       -1       -1       -1       -1       -1       -1       -1
## 2       -1       -1       -1       -1       -1       -1       -1       -1
## 3       -1       -1       -1       -1       -1       -1       -1       -1
## 4       -1       -1       -1       -1       40       -1       40       -1
## 5       -1       -1       -1       -1       30       25       55       -1
## 6       -1       -1       -1       -1       -1       -1       -1       -1
##   PEHSPNON.y PEIO1COW PEIO1ICD PEIO1OCD PEIO2COW PEIO2ICD PEIO2OCD PEJHRSN
## 1          2       -1       -1       -1       -1       -1       -1      -1
## 2          2       -1       -1       -1       -1       -1       -1      -1
## 3          2       -1       -1       -1       -1       -1       -1      -1
## 4          2        4     8190     3600       -1       -1       -1      -1
## 5          2        3     7860     2540        4     6180     4600      -1
## 6          1       -1       -1       -1       -1       -1       -1      -1
##   PEJHWANT PEJHWKO PELAYAVL PELAYDUR PELAYFTO PELAYLK PELKAVL PELKDUR PELKFTO
## 1       -1      -1       -1       -1       -1      -1      -1      -1      -1
## 2       -1      -1       -1       -1       -1      -1      -1      -1      -1
## 3        1       2       -1       -1       -1      -1      -1      -1      -1
## 4       -1      -1       -1       -1       -1      -1      -1      -1      -1
## 5       -1      -1       -1       -1       -1      -1      -1      -1      -1
## 6        2       2       -1       -1       -1      -1      -1      -1      -1
##   PELKLL1O PELKLL2O PELKLWO PELKM1 PELNDAD PELNMOM PEMARITL PEMJNUM PEMJOT
## 1       -1       -1      -1     -1      -1      -1        1      -1     -1
## 2       -1       -1      -1     -1      -1      -1        1      -1     -1
## 3       -1       -1      -1     -1      -1       1        6      -1     -1
## 4       -1       -1      -1     -1      -1      -1        4      -1      2
## 5       -1       -1      -1     -1      -1      -1        1       2      1
## 6       -1       -1      -1     -1      -1       1        6      -1     -1
##   PEMLR PEMNTVTY PEMOMTYP PENATVTY PENLFACT PENLFJH PENLFRET PEPARENT PERET1
## 1     5       57       -1       57       -1       2       -1       -1      2
## 2     5       57       -1       57       -1       2       -1       -1      2
## 3     7       57        1       57        4      -1       -1        1     -1
## 4     1       57       -1       57       -1      -1       -1       -1     -1
## 5     1       57       -1       57       -1      -1       -1       -1     -1
## 6     7      370        1      370        3      -1       -1        1     -1
##   PERRP PESCHENR PESCHFT PESCHLVL PESEX PESPOUSE PRABSREA PRAGNA PRCITFLG
## 1     1       -1      -1       -1     2        2       -1     -1        0
## 2     3       -1      -1       -1     1        1       -1     -1        0
## 3     4        2      -1       -1     2       -1       -1     -1        0
## 4     2        1       2        2     2       -1       -1      2        0
## 5     3       -1      -1       -1     2        1       -1      2        0
## 6     4        2      -1       -1     2       -1       -1     -1        0
##   PRCITSHP PRCIVLF PRCOW1 PRCOW2 PRCOWPG PRDISC PRDISFLG PRDTCOW1 PRDTCOW2
## 1        1       2     -1     -1      -1     -1        2       -1       -1
## 2        1       2     -1     -1      -1     -1        1       -1       -1
## 3        1       2     -1     -1      -1     -1        2       -1       -1
## 4        1       1      4     -1       1     -1        2        6       -1
## 5        1       1      3      4       2     -1        2        9        6
## 6        5       2     -1     -1      -1     -1        2       -1       -1
##   PRDTHSP PRDTIND1 PRDTIND2 PRDTOCC1 PRDTOCC2 PREMP PREMPHRS PREMPNOT PRERELG
## 1      -1       -1       -1       -1       -1    -1        0        4       0
## 2      -1       -1       -1       -1       -1    -1        0        4       0
## 3      -1       -1       -1       -1       -1    -1        0        4       0
## 4      -1       41       -1       11       -1     1       18        1       1
## 5      -1       40       23        8       15     1       21        1       1
## 6       7       -1       -1       -1       -1    -1        0        4       0
##   PRERNHLY PRERNWA PREXPLF PRFAMNUM PRFAMREL PRFAMTYP PRFTLF PRHERNAL PRHRUSL
## 1       -1      -1      -1        1        1        1     -1       -1      -1
## 2       -1      -1      -1        1        2        1     -1       -1      -1
## 3       -1      -1      -1        2        1        3     -1       -1      -1
## 4     1500   60000       1        0        0        2      1        1       4
## 5     4500  135000       1        1        2        1      1        1       2
## 6       -1      -1      -1        1        3        1     -1       -1      -1
##   PRIMIND1 PRIMIND2 PRINUYER PRIOELG PRJOBSEA PRMARSTA PRMJIND1 PRMJIND2
## 1       -1       -1        0       0       -1        1       -1       -1
## 2       -1       -1        0       0       -1        1       -1       -1
## 3       -1       -1        0       0       -1        7       -1       -1
## 4       16       -1        0       1       -1        5       10       -1
## 5       15        8        0       1       -1        1       10        6
## 6       -1       -1       23       0       -1        7       -1       -1
##   PRMJOCC1 PRMJOCC2 PRMJOCGR PRNAGPWS PRNAGWS PRNLFSCH PRNMCHLD PRPERTYP
## 1       -1       -1       -1       -1      -1       -1        0        2
## 2       -1       -1       -1       -1      -1       -1        0        2
## 3       -1       -1       -1       -1      -1        2        2        2
## 4        3       -1        2        1       1        2        0        2
## 5        2        3        1       -1       1       -1        0        2
## 6       -1       -1       -1       -1      -1        1        0        2
##   PRPTHRS PRPTREA PRSJMJ PRTAGE PRTFAGE PRUNEDUR PRUNTYPE PRWERNAL PRWKSCH
## 1      -1      -1     -1     62       0       -1       -1       -1       0
## 2      -1      -1     -1     69       0       -1       -1       -1       0
## 3      -1      -1     -1     24       0       -1       -1       -1       0
## 4      -1      -1      1     31       0       -1       -1        1       1
## 5      -1      -1      2     59       0       -1       -1        1       1
## 6      -1      -1     -1     16       0       -1       -1       -1       0
##   PRWKSTAT PRWNTJOB PTDTRACE.y PTHR PTOT PTWK PUABSOT PUBUS1 PUBUS2OT PUBUSCK1
## 1        1        2          1    0    0    0      -1     -1       -1        2
## 2        1        2          2    0    0    0      -1     -1       -1        2
## 3        1        2          2    0    0    0       2     -1       -1        2
## 4        2       -1          1    0    0    0      -1     -1       -1       -1
## 5        2       -1          1    0    0    0      -1     -1       -1       -1
## 6        1        2          3    0    0    0       2      2       -1        1
##   PUBUSCK2 PUBUSCK3 PUBUSCK4 PUCHINHH PUDIS PUDIS1 PUDIS2 PUDWCK1 PUDWCK2
## 1       -1       -1       -1        9    -1     -1     -1      -1      -1
## 2       -1       -1       -1        9    -1     -1     -1      -1      -1
## 3       -1        2       -1        9    -1     -1     -1       4      -1
## 4       -1       -1       -1        9    -1     -1     -1      -1      -1
## 5       -1       -1       -1        9    -1     -1     -1      -1      -1
## 6       -1        2       -1        9    -1     -1     -1       4      -1
##   PUDWCK3 PUDWCK4 PUDWCK5 PUERN2 PUERNH1C PUHRCK1 PUHRCK12 PUHRCK2 PUHRCK3
## 1      -1      -1      -1     -1       -1      -1       -1      -1      -1
## 2      -1      -1      -1     -1       -1      -1       -1      -1      -1
## 3      -1      -1      -1     -1       -1      -1       -1      -1      -1
## 4      -1      -1      -1     -1       -1       2        2       5       5
## 5      -1      -1      -1     -1       -1       1        2       5       5
## 6      -1      -1      -1     -1       -1      -1       -1      -1      -1
##   PUHRCK4 PUHRCK5 PUHRCK6 PUHRCK7 PUHROFF1 PUHROFF2 PUHROT1 PUHROT2 PUIO1MFG
## 1      -1      -1      -1      -1       -1       -1      -1      -1       -1
## 2      -1      -1      -1      -1       -1       -1      -1      -1       -1
## 3      -1      -1      -1      -1       -1       -1      -1      -1       -1
## 4      -1       2       3       5        2       -1       2      -1        4
## 5      -1       1       3       5        2       -1       2      -1        4
## 6      -1      -1      -1      -1       -1       -1      -1      -1       -1
##   PUIO2MFG PUIOCK1 PUIOCK2 PUIOCK3 PUIODP1 PUIODP2 PUIODP3 PUJHCK1 PUJHCK2
## 1       -1      -1      -1      -1      -1      -1      -1      -1      -1
## 2       -1      -1      -1      -1      -1      -1      -1      -1      -1
## 3       -1      -1      -1      -1      -1      -1      -1       2       3
## 4       -1       4       3       3       1       2       1      -1      -1
## 5        4       4       3       3       1       2       1      -1      -1
## 6       -1      -1      -1      -1      -1      -1      -1       2       3
##   PUJHCK3 PUJHCK4 PUJHCK5 PUJHDP1O PULAY PULAY6M PULAYAVR PULAYCK1 PULAYCK2
## 1      -1      -1      -1       -1    -1      -1       -1       -1       -1
## 2      -1      -1      -1       -1    -1      -1       -1       -1       -1
## 3      -1       5      -1       -1     2      -1       -1       -1       -1
## 4      -1      -1      -1       -1    -1      -1       -1       -1       -1
## 5      -1      -1      -1       -1    -1      -1       -1       -1       -1
## 6      -1       5      -1       -1     2      -1       -1       -1       -1
##   PULAYCK3 PULAYDT PULINENO PULK PULKAVR PULKDK1 PULKDK2 PULKDK3 PULKDK4
## 1       -1      -1        1   -1      -1      -1      -1      -1      -1
## 2       -1      -1        2   -1      -1      -1      -1      -1      -1
## 3       -1      -1        2    2      -1      -1      -1      -1      -1
## 4       -1      -1        1   -1      -1      -1      -1      -1      -1
## 5       -1      -1        2   -1      -1      -1      -1      -1      -1
## 6       -1      -1        2    2      -1      -1      -1      -1      -1
##   PULKDK5 PULKDK6 PULKM2 PULKM3 PULKM4 PULKM5 PULKM6 PULKPS1 PULKPS2 PULKPS3
## 1      -1      -1     -1     -1     -1     -1     -1      -1      -1      -1
## 2      -1      -1     -1     -1     -1     -1     -1      -1      -1      -1
## 3      -1      -1     -1     -1     -1     -1     -1      -1      -1      -1
## 4      -1      -1     -1     -1     -1     -1     -1      -1      -1      -1
## 5      -1      -1     -1     -1     -1     -1     -1      -1      -1      -1
## 6      -1      -1     -1     -1     -1     -1     -1      -1      -1      -1
##   PULKPS4 PULKPS5 PULKPS6 PUNLFCK1 PUNLFCK2 PURETOT PUSLFPRX PUWK PXABSPDO
## 1      -1      -1      -1       -1        1       1        1    3       -1
## 2      -1      -1      -1       -1        1       1        2    3       -1
## 3      -1      -1      -1        1       -1      -1        2    2       -1
## 4      -1      -1      -1       -1       -1      -1        1    1        1
## 5      -1      -1      -1       -1       -1      -1        2    1        1
## 6      -1      -1      -1        1       -1      -1        2    2       -1
##   PXABSRSN PXAFEVER PXAFNOW PXAFWHN1 PXAGE PXCOHAB PXCYC PXDADTYP PXDIPGED
## 1       -1        0       0        1     0       1     1        1        0
## 2       -1        0       0        1     0       1     1        1        1
## 3       -1        0       0        1     0       1     1        1        0
## 4        1        0       0        1     0       1     0        1        1
## 5        1        0       0        1     0       1     1        1        0
## 6       -1        1      21        1     0       1     1        1        1
##   PXDISDRS PXDISEAR PXDISEYE PXDISOUT PXDISPHY PXDISREM PXDW4WK PXDWAVL PXDWAVR
## 1        0        0        0        0        0        0       1       1       1
## 2        0        0        0        0        0        0       1       1       1
## 3        0        0        0        0        0        0       1       1       1
## 4        0        0        0        0        0        0      -1      -1      -1
## 5        0        0        0        0        0        0      -1      -1      -1
## 6        0        0        0        0        0        0       1       1       1
##   PXDWLKO PXDWLKWK PXDWRSN PXDWWK PXDWWNTO PXEDUCA PXERN PXERNCOV PXERNH1O
## 1       1        1       1      1        1       0    -1       -1       -1
## 2       1        1       1      1        1       0    -1       -1       -1
## 3       1        1       1      1        0       0    -1       -1       -1
## 4      -1       -1      -1     -1       -1       0     1        0       41
## 5      -1       -1      -1     -1       -1       0     1        0       42
## 6       1        1       1      1        0       0    -1       -1       -1
##   PXERNH2 PXERNHRO PXERNHRY PXERNLAB PXERNPER PXERNRT PXERNUOT PXERNWKP
## 1      -1       -1       -1       -1       -1      -1       -1       -1
## 2      -1       -1       -1       -1       -1      -1       -1       -1
## 3      -1       -1       -1       -1       -1      -1       -1       -1
## 4       1       11       42        0       13       1       41        1
## 5       1       11        0        0        0       1        0        1
## 6      -1       -1       -1       -1       -1      -1       -1       -1
##   PXFNTVTY PXHGCOMP PXHRACT1 PXHRACT2 PXHRACTT PXHRAVL PXHRFTPT PXHRRSN1
## 1       -1        1       -1       -1       -1      -1        1       -1
## 2       -1        1       -1       -1       -1      -1        1       -1
## 3       -1        0       -1       -1       -1      -1        1       -1
## 4       -1        1        0        1        0       1        1        1
## 5       -1        1        0        0        0       1        1        1
## 6       -1        1       -1       -1       -1      -1        1       -1
##   PXHRRSN2 PXHRRSN3 PXHRUSL1 PXHRUSL2 PXHRUSLT PXHRWANT PXHSPNON PXINUSYR
## 1       -1       -1       -1       -1       -1       -1        0       -1
## 2       -1       -1       -1       -1       -1       -1        0       -1
## 3       -1       -1       -1       -1       -1       -1        0       -1
## 4        1        1        0        1        0        1        0       -1
## 5        1        1        0        0        0        1        0       -1
## 6       -1       -1       -1       -1       -1       -1        0       -1
##   PXIO1COW PXIO1ICD PXIO1OCD PXIO2COW PXIO2ICD PXIO2OCD PXJHRSN PXJHWANT
## 1       -1       -1       -1       -1       -1       -1       1        1
## 2       -1       -1       -1       -1       -1       -1       1        1
## 3       -1       -1       -1       -1       -1       -1       1        0
## 4        0        0        0       -1       -1       -1      -1       -1
## 5        0        0        0        0        0        0      -1       -1
## 6       -1       -1       -1       -1       -1       -1       1        0
##   PXJHWKO PXLAYAVL PXLAYDUR PXLAYFTO PXLAYLK PXLKAVL PXLKDUR PXLKFTO PXLKLL1O
## 1       1       -1       -1       -1      -1      -1      -1      -1       -1
## 2       1       -1       -1       -1      -1      -1      -1      -1       -1
## 3       0       -1       -1       -1      -1      -1      -1      -1       -1
## 4      -1       -1       -1       -1      -1      -1      -1      -1       -1
## 5      -1       -1       -1       -1      -1      -1      -1      -1       -1
## 6       0       -1       -1       -1      -1      -1      -1      -1       -1
##   PXLKLL2O PXLKLWO PXLKM1 PXLNDAD PXLNMOM PXMARITL PXMJNUM PXMJOT PXMLR
## 1       -1      -1     -1      50      50        0      -1     -1     0
## 2       -1      -1     -1      50      50        0      -1     -1     0
## 3       -1      -1     -1      50       0        0      -1     -1     0
## 4       -1      -1     -1      50      50        0       1      0     0
## 5       -1      -1     -1      50      50        0       0      0     0
## 6       -1      -1     -1      50       0        0      -1     -1     0
##   PXMNTVTY PXMOMTYP PXNATVTY PXNLFACT PXNLFJH PXNLFRET PXPARENT PXRACE1 PXRET1
## 1       -1        1       -1        1       0        1       50       0      0
## 2       -1        1       -1        1       0        1       50       0      0
## 3       -1        0       -1        0       1        1        0       0      1
## 4       -1        1       -1       -1      -1       -1       50       0     -1
## 5       -1       50       -1       -1      -1       -1       50       0     -1
## 6       -1        0       -1        0       1        1        0       0      1
##   PXRRP PXSCHENR PXSCHFT PXSCHLVL PXSEX PXSPOUSE QSTNUM TRATUSR PRDASIAN
## 1     0       -1      -1       -1     0        0  51174       1       -1
## 2     0       -1      -1       -1     0        0  31951       1       -1
## 3     0        0       1        1     0        1  66684       1       -1
## 4     0        0       0        0     0        1  36321       1       -1
## 5     0       -1      -1       -1     0        0  36565       1       -1
## 6     0        0       1        1     0        1    640       1       -1
##   PEPDEMP1 PTNMEMP1 PEPDEMP2 PTNMEMP2 PXPDEMP1 PXNMEMP1 PXPDEMP2 PXNMEMP2
## 1       -1       -1       -1       -1        0        0        0        0
## 2       -1       -1       -1       -1        0        0        0        0
## 3       -1       -1       -1       -1        0        0        0        0
## 4       -1       -1       -1       -1        1        1       -1       -1
## 5       -1       -1       -1       -1        1        1        1        1
## 6       -1       -1       -1       -1        0        0        0        0
```

```r
### Time on average that a person in the sample spend on 
### Socializing and communicating with others
mean(atus.all$t120101)
```

```
## [1] 38.06481
```



```r
### Summing all activities that is associated with “Caring For & Helping ### HH Children”.
atus.all <- atus.all %>% 
    mutate(CHILDCARE = rowSums(select(atus.all, starts_with("t0301"))))
```


```r
### density of CHILDCARE variable
ggplot(atus.all) +
    geom_density(aes(CHILDCARE))
```

<img src="data_analysis_project_files/figure-html/childcare-density-plot-1.png" width="672" />


```r
### Gender which spend more time with children
atus.all %>% 
    group_by(TESEX) %>% 
    summarise(AVG.TIME = mean(CHILDCARE))
```

```
## # A tibble: 2 x 2
##   TESEX AVG.TIME
## * <int>    <dbl>
## 1     1     19.0
## 2     2     33.2
```


```r
## replace -1 in the variable TRDPFTPT with NA.
atus.all <- atus.all %>% 
  replace_with_na_at(.vars = c("TRDPFTPT"), condition = ~.x == -1)
```

```r
## number of missing value in the variable TRDPFTPT
sum(is.na(atus.all$TRDPFTPT))
```

```
## [1] 4119
```



```r
## scatter plot of age and chidcare variable
atus.all %>%
  filter(TRCHILDNUM > 0) %>%
  ggplot(aes(TEAGE, CHILDCARE)) +
  geom_point()
```

<img src="data_analysis_project_files/figure-html/exploratory-analysis-1.png" width="672" />


```r
## replace -1 in the variable TRERNWA with NA.
atus.all <- atus.all %>% 
  replace_with_na_at(.vars = c("TRERNWA"), condition = ~.x == -1)
```

```r
## number of missing value in the variable TRERNWA
sum(is.na(atus.all$TRERNWA))
```

```
## [1] 4833
```



```r
## scatter plot of income and chidcare variable
atus.all %>%
  filter(TRCHILDNUM > 0) %>%
  ggplot(aes(HEFAMINC, CHILDCARE)) +
  geom_point()
```

<img src="data_analysis_project_files/figure-html/unnamed-chunk-6-1.png" width="672" />

```r
## box-plot of Full time or part time employment status of respondent and ## childcare

atus.all %>%
  filter(TRCHILDNUM > 0, is.na(TRDPFTPT) == FALSE) %>%
  ggplot(aes(as.factor(TRDPFTPT), CHILDCARE)) +
  geom_boxplot(color = "brown", fill = "wheat")
```

<img src="data_analysis_project_files/figure-html/unnamed-chunk-7-1.png" width="672" />


```r
## box-plot of marital status of respondent and  childcare

atus.all %>%
  filter(TRCHILDNUM > 0, is.na(TRSPPRES) == FALSE) %>%
  ggplot(aes(as.factor(TRSPPRES), CHILDCARE)) +
  geom_boxplot(fill = "steelblue")
```

<img src="data_analysis_project_files/figure-html/unnamed-chunk-8-1.png" width="672" />


### Regression Analysis


```r
## regression analysis 
reg_model <- lm(CHILDCARE ~ TEAGE + TESEX + TRDPFTPT + TRSPPRES + HEFAMINC + TRCHILDNUM, data = filter(atus.all, TRCHILDNUM > 0))
summary(reg_model)
```

```
## 
## Call:
## lm(formula = CHILDCARE ~ TEAGE + TESEX + TRDPFTPT + TRSPPRES + 
##     HEFAMINC + TRCHILDNUM, data = filter(atus.all, TRCHILDNUM > 
##     0))
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -110.22  -54.78  -30.67   24.68  722.23 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  93.7108    13.3047   7.043 2.30e-12 ***
## TEAGE        -1.4587     0.1675  -8.711  < 2e-16 ***
## TESEX        23.4814     3.4149   6.876 7.39e-12 ***
## TRDPFTPT     -5.3927     4.1258  -1.307 0.191280    
## TRSPPRES    -16.6108     2.1434  -7.750 1.24e-14 ***
## HEFAMINC      0.7036     0.4793   1.468 0.142170    
## TRCHILDNUM    6.7424     1.7801   3.788 0.000155 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 89.57 on 3115 degrees of freedom
##   (1191 observations deleted due to missingness)
## Multiple R-squared:  0.05386,	Adjusted R-squared:  0.05204 
## F-statistic: 29.56 on 6 and 3115 DF,  p-value: < 2.2e-16
```

### Exploratory Analysis of Age and Activities


```r
atus.wide <- atus.all %>%
    mutate(act01 = rowSums(select(atus.all, starts_with("t01"))),
           act02 = rowSums(select(atus.all, starts_with("t02"))),
           act03 = rowSums(select(atus.all, starts_with("t03"))),
           act04 = rowSums(select(atus.all, starts_with("t04"))),
           act05 = rowSums(select(atus.all, starts_with("t05"))),
           act06 = rowSums(select(atus.all, starts_with("t06"))),
           act07 = rowSums(select(atus.all, starts_with("t07"))),
           act08 = rowSums(select(atus.all, starts_with("t08"))),
           act09 = rowSums(select(atus.all, starts_with("t09"))),
           act10 = rowSums(select(atus.all, starts_with("t10"))),
           act11 = rowSums(select(atus.all, starts_with("t11"))),
           act12 = rowSums(select(atus.all, starts_with("t12"))),
           act13 = rowSums(select(atus.all, starts_with("t13"))),
           act14 = rowSums(select(atus.all, starts_with("t14"))),
           act15 = rowSums(select(atus.all, starts_with("t15"))),
           act16 = rowSums(select(atus.all, starts_with("t16"))),
           # act17 = , there is no category 17 in the data
           act18 = rowSums(select(atus.all, starts_with("t18")))) %>% 
    select(TUCASEID, TEAGE, HEFAMINC, starts_with("act"))
```


```r
## mean time spent on each activity

atus.wide %>% select(starts_with("act")) %>%
  sapply(function(x) mean(x))
```

```
##       act01       act02       act03       act04       act05       act06 
## 580.2775183 121.0976842  31.8539026   8.6025922 157.6653960  15.3870199 
##       act07       act08       act09       act10       act11       act12 
##  24.1533403   4.7471648   0.9868484   0.3647193  64.9878014 298.4304775 
##       act13       act14       act15       act16       act18 
##  20.2180501  13.3995997   8.4686934   6.7738492  71.5043362
```

```r
## most time spent on "work & work-related activities"
max(atus.wide$act05)
```

```
## [1] 1350
```


```r
## use code to convert the wide format to long.
atus.long <- atus.wide %>% 
  gather(key = "ACTIVITY", value = "MINS", 
         colnames(select(atus.wide, starts_with("act"))))
```


```r
## viewing the first rows of the long dataframe
head(atus.long)
```

```
##      TUCASEID TEAGE HEFAMINC ACTIVITY MINS
## 1 2.01601e+13    62        3    act01  715
## 2 2.01601e+13    69        6    act01  620
## 3 2.01601e+13    24        4    act01 1060
## 4 2.01601e+13    31        8    act01  655
## 5 2.01601e+13    59       13    act01  580
## 6 2.01601e+13    16        5    act01  620
```



```r
atus.long %>% 
    group_by(ACTIVITY, TEAGE) %>% 
    summarise(AVGMINS = mean(MINS)) %>% 
    ggplot(aes(TEAGE, AVGMINS)) + geom_line() + facet_wrap(~ACTIVITY, nrow = 3)
```

```
## `summarise()` has grouped output by 'ACTIVITY'. You can override using the `.groups` argument.
```

<img src="data_analysis_project_files/figure-html/age-activity-1.png" width="672" />

### Exploratory Analysis of Income and Activities


```r
atus.long %>% 
  group_by(ACTIVITY, HEFAMINC) %>% 
  summarise(TOTAL = sum(MINS)) %>%
  ggplot(aes(ACTIVITY, TOTAL, fill = as.factor(HEFAMINC))) +
  geom_col() +
  coord_flip() +
  scale_fill_viridis_d(option = "inferno") +
  labs(title = "How different income groups spend time doing each activity ?", subtitle = "2016 American Time Used Survey", fill = "Income Category") +
  ylab("Time (min)") +
  theme_minimal() + 
  theme(legend.position = "bottom")
```

```
## `summarise()` has grouped output by 'ACTIVITY'. You can override using the `.groups` argument.
```

<img src="data_analysis_project_files/figure-html/activity-income-1.png" width="672" />

```r
  ## add the rest of the code here
```


```r
## save the plot above
ggsave(filename = "figures/explanatory_figures/Income_Activity.png", width = 10, height = 10, units = c("cm"))
```
