Figure out where these photos were taken
================
Marissa Lee
11/27/2018

This file will allow you to determine where a folder of photos with non-helpful filenames were taken in the field.

I have a list of sites that were visited and the date they were visited -- `data/sites_rand.csv` and `data/sites_dataDictionary.csv`. My folder of unidentified photos are here -- `data/photos`.

I sometimes took more than one photo at a given site, but I didn't take a photo at every site.

We'll start by extracting metadata from photo files.

``` r
library(exifr) # see https://www.r-bloggers.com/extracting-exif-data-from-photos-using-r/
#getwd() # you should be in GWUAnalysesInR2018
photopath <- "data/photos"
photo.df <- read_exif(path = photopath, recursive = T, quiet = F)
```

This includes a ton of information that we don't need. We just want to keep when the photo was taken. Also, this time stamp format is more info than we need and it doesn't match the date format in `sites.csv`.

``` r
library(tidyverse)
photo.df %>%
  select(FileName, DateTimeOriginal) %>%
  separate(DateTimeOriginal, into = c("date","time"), sep = " ", remove = F) %>%
  separate(date, into = c("year","month","day"), remove = T) %>%
  mutate(month = as.integer(month)) %>%
  mutate(day = as.integer(day)) %>%
  select(-c(year, time)) -> photo.df.c
```

Now, load the site data.

``` r
site.df.c <- read_csv(file = "data/sites_rand.csv")
```

Use the site data collection dates to identify a list of potential sites for each photo.

``` r
photo.df.c %>%
  left_join(site.df.c, by = c("month","day")) -> photo.df.ann
photo.df.ann
```

    ## # A tibble: 6 x 5
    ##       FileName    DateTimeOriginal month   day        site
    ##          <chr>               <chr> <int> <int>       <chr>
    ## 1 IMG_5999.JPG 2018:09:19 12:04:40     9    19 MHC-ONE-NCD
    ## 2 IMG_6002.JPG 2018:09:19 15:06:33     9    19 MHC-ONE-NCD
    ## 3 IMG_6001.JPG 2018:09:19 15:06:30     9    19 MHC-ONE-NCD
    ## 4 IMG_6011.JPG 2018:09:20 10:02:50     9    20        <NA>
    ## 5 IMG_6020.JPG 2018:09:20 15:30:22     9    20        <NA>
    ## 6 IMG_5997.JPG 2018:09:19 11:48:45     9    19 MHC-ONE-NCD

This table tells us that all the photos were taken at MHC-ONE-NCD except IMG\_6011.JPG and IMG\_5997.JPG

Let's save the table for later.

``` r
write.csv(photo.df.ann, file = "output/finalTab.csv", row.names = F)
```

Wooo hooo!
