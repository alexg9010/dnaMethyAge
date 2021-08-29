# dnaMethyAge
A user friendly **R package** to predict epigenetic age and calculate age acceleration from DNA methylation data. Supported age clocks are Horvath's clock, Hannum's clock, Levine's PhenoAge, and Zhang's clock.

## 1. Usage

### 1.1 Installation

**Install from Github**
```R
## Make sure 'devetools' is installed in your R
# install.packages("devtools")
devtools::install_github("yiluyucheng/dnaMethyAge")
```

### 2.1 How to use

Start a R work environment

#### (1) Predict epigenetic age from DNA methylation data

```R
library('dnaMethyAge')

## prepare betas dataframe
data('subGSE174422') ## load example betas

print(dim(betas)) ## probes in row and samples in column
# 485577 8

clock_name <- 'Horvath2013'  # Select one of those: Hannum2013, Horvath2013, Levine2018, Zhang2019.
## Use Horvath's clock with adjusted-BMIQ normalisation (same as Horvath's paper)
horvath_age <- methyAge(betas, clock=clock_name)

print(horvath_age)
#                         Sample     mAge
# 1 GSM5310260_3999979009_R02C02 74.88139
# 2 GSM5310261_3999979017_R05C01 62.36400
# 3 GSM5310262_3999979018_R02C02 68.04759
# 4 GSM5310263_3999979022_R02C01 61.62691
# 5 GSM5310264_3999979027_R02C01 59.65161
# 6 GSM5310265_3999979028_R01C01 60.95991
# 7 GSM5310266_3999979029_R04C02 52.48954
# 8 GSM5310267_3999979031_R06C02 64.29711

```

Currently, supported age clocks are 'Hannum2013', 'Horvath2013', 'Levine2018', 'Zhang2019'. More age models will be added in the future.


#### (2)  Predict epigenetic age and calculate age acceleration

```R
library('dnaMethyAge')

## prepare betas dataframe
data('subGSE174422') ## load example betas and info

print(dim(betas)) ## probes in row and samples in column
# 485577 8
print(info) ##  info should be a dataframe which includes at least two columns: Sample, Age.
#                         Sample  Age    Sex
# 1 GSM5310260_3999979009_R02C02 68.8 Female
# 2 GSM5310261_3999979017_R05C01 45.6 Female
# 3 GSM5310262_3999979018_R02C02 67.4 Female
# 4 GSM5310263_3999979022_R02C01 45.6 Female
# 5 GSM5310264_3999979027_R02C01 62.5 Female
# 6 GSM5310265_3999979028_R01C01 45.1 Female
# 7 GSM5310266_3999979029_R04C02 53.2 Female
# 8 GSM5310267_3999979031_R06C02 63.8 Female


clock_name <- 'Horvath2013'  # Select one of those: Hannum2013, Horvath2013, Levine2018, Zhang2019.
## Apply Horvath's clock and calculate age acceleration
## Use Horvath's clock with adjusted-BMIQ normalisation (same as Horvath's paper)
horvath_age <- methyAge(betas, clock=clock_name, age_info=info, fit_method='Linear', do_plot=TRUE)

print(horvath_age)
#                         Sample  Age    Sex     mAge Age_Acceleration
# 1 GSM5310260_3999979009_R02C02 68.8 Female 74.88139         7.334461
# 2 GSM5310261_3999979017_R05C01 45.6 Female 62.36400         3.318402
# 3 GSM5310262_3999979018_R02C02 67.4 Female 68.04759         1.013670
# 4 GSM5310263_3999979022_R02C01 45.6 Female 61.62691         2.581311
# 5 GSM5310264_3999979027_R02C01 62.5 Female 59.65161        -5.586763
# 6 GSM5310265_3999979028_R01C01 45.1 Female 60.95991         2.097534
# 7 GSM5310266_3999979029_R04C02 53.2 Female 52.48954        -9.340977
# 8 GSM5310267_3999979031_R06C02 63.8 Female 64.29711        -1.417638
```

By default, methyAge would plot the age prediction results and the distribution of age acceleration, to save the plot:
```R
pdf('savename.pdf', width=4.3, height=6)
horvath_age <- methyAge(betas, clock=clock_name, age_info=info, fit_method='Linear', do_plot=TRUE)
dev.off()
```
Here is the result plot(very nice!):

<img width="320" src="https://github.com/yiluyucheng/dnaMethyAge/blob/main/test_res/test_result1.png">

Now you can try other clocks by simply redefine the 'clock_name' and keep other codes unedited. Normally, the clock of Horvath2013 costs the highest amount of time due to its unefficient normalisation steps, for other clocks such as Hannum2013, the overall running time is very short. Please refer to the below code to learn more about how to use the method.
```R
library('dnaMethyAge')

help(methyAge)
```

## 2. Reference

Hannum2013: Genome-wide Methylation Profiles Reveal Quantitative Views of Human Aging Rates. Hannum et al., 2013

Horvath2013: DNA methylation age of human tissues and cell types. Steve Horvath, 2013

Levine2018: An epigenetic biomarker of aging for lifespan and healthspan. Levine et al., 2018

Zhang2019: Improved precision of epigenetic clock estimates across tissues and its implication for biological ageing, Zhang et al., 2019

## 3. Contact me

yw19282@essex.ac.uk



