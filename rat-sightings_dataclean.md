data cleaning
================
Laura Robles-Torres
2023-11-10

``` r
library(tidyverse)
library(tidyr)
library(readxl)
library(stringr)
library(dbplyr)
```

## Cleaning rat sightings dataset

``` r
rat_sightings = 
  read_csv ("./data/Rat_Sightings.csv") |>
  janitor::clean_names(case = "snake") |>
  separate(created_date, sep="/", into = c("month", "day", "year")) |> 
  separate(year, sep=" ", into = c("year")) |>
  filter(borough != "STATEN ISLAND") |> 
  filter(year %in% c("2019", "2020", "2021", "2022", "2023")) |>
  mutate(
    borough_id = recode(
      borough, 
      "MANHATTAN" = 1,
      "BRONX" =2,
      "BROOKLYN"=3,
      "QUEENS"= 4)) |>
  mutate(
    month = as.numeric(month),
    year = as.numeric(year)
  ) |>
  select(unique_key, month, day, year, location_type, incident_zip, borough, location, borough_id) |>
  mutate(
    borough = str_to_sentence(borough)
  )
```

PLEASE LORD.
