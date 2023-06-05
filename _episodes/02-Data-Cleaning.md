---
title: "Preparing and Cleaning Data for Darwin Core Archives"
teaching: 30
exercises: 30
questions:
- "How to convert dates to Darwin Core Archive standard?"
- "How to match scientific names to ALA?"
- "How to convert latitudes and longitudes to decimal degrees?"
objectives:
- "Aligning dates to the ISO 8601 standard used by Darwin Core."
- "Matching scientific names to the ALA."
- "Converting latitude and longitude variations to decimal degrees North and East."
keypoints:
- "When doing conversions it's best to break out your data into it's component pieces."
- "Dates are messy to deal with. Some packages have easy solutions, otherwise use regular expressions to align date strings to ISO 8601."
- "WoRMS LSIDs are a requirement for OBIS."
- "Latitude and longitudes are like dates, they can be messy to deal with. Take a similar approach."
---

### Preparing Your Data for a Darwin Core Archive
Now that we understand a bit more about what a Darwin Core Archive is and how it is used today, we can begin talking about data standards. These data standards ensure that we, as well as others, get the most out of our data.  The key resource when mapping data to Darwin Core is the [Darwin Core Quick Reference Guide](https://dwc.tdwg.org/terms/). This document provides an easy-to-read reference of the currently recommended terms for the Darwin Core standard. There are a lot of terms there, and you won't use them all for every dataset (or even use them all on any dataset).  However, as you apply the standard to more datasets, you'll become more familiar with the terms.

> ## Tip 
> If your raw column headers are Darwin Core terms verbatim then you can skip this step! Next time you plan data collection use the standard DwC term headers!
{: .callout}

> ## Exercise
> 
> **Challenge:** Find the matching Darwin Core term for these column headers.
> 
> 1. SAMPLE_DATE (example data: 09-MAR-21 05.45.00.000000000 PM)
> 2. lat (example data: 32.6560)
> 3. depth_m (example data: 6 meters)
> 4. COMMON_NAME (example data: staghorn coral)
> 5. percent_cover (example data: 15)
> 6. COUNT (example data: 2 Females)
> 
> > ## Solution
> > 1. [`eventDate`](https://dwc.tdwg.org/terms/#dwc:eventDate)
> > 2. [`decimalLatitude`](https://dwc.tdwg.org/terms/#dwc:decimalLatitude)
> > 3. [`minimumDepthInMeters`](https://dwc.tdwg.org/terms/#dwc:minimumDepthInMeters) and [`maximumDepthInMeters`](https://dwc.tdwg.org/terms/#dwc:maximumDepthInMeters)
> > 4. [`vernacularName`](https://dwc.tdwg.org/terms/#dwc:vernacularName)
> > 5. [`organismQuantity`](https://dwc.tdwg.org/terms/#dwc:organismQuantity) and [`organismQuantityType`](https://dwc.tdwg.org/terms/#dwc:organismQuantityType)
> > 6. This one is tricky- it's two terms combined and will need to be split. [`indvidualCount`](https://dwc.tdwg.org/terms/#dwc:individualCount) and [`sex`](https://dwc.tdwg.org/terms/#dwc:sex)
> >
> > {: .output}
> {: .solution}
{: .challenge}

> ## Tip 
> To make the mapping step easier on yourself, we recommend starting a mapping document/spreadsheet (or document it as a comment in your script). List out all of your column headers in one column and document the appropriate Dawin Core term(s) in a second column. For example:
> 
> | my term | DwC term |
> |---------|----------|
> | lat | decimalLatitude |
> | date | eventDate |
> | species | scientificName |
{: .callout}


### What are the **required** Darwin Core terms for publishing to ALA?
When doing your mapping some required information may be missing. Below are the Darwin Core terms that are required to share your data to ALA plus a few that are needed for GBIF.

| Darwin Core Term | Definition | Comment | Example |
|------------------|------------------------------------|---------------------------------------|-----------------|
| [`occurrenceID`](https://dwc.tdwg.org/terms/#dwc:occurrenceID) | An identifier for the Occurrence (as opposed to a particular digital record of the occurrence). In the absence of a persistent global unique identifier, construct one from a combination of identifiers in the record that will most closely make the occurrenceID globally unique. | To construct a globally unique identifier for each occurrence you can usually concatenate station + date + scientific name (or something similar) but you'll need to check this is unique for each row in your data. It is preferred to use the fields that are least likely to change in the future for this. **For ways to check the uniqueness of your occurrenceIDs see the [QA / QC]({{ page.root }}/06-qa-qc/index.html) section of the workshop.** | Station_95_Date_09JAN1997:14:35:00.000_Atractosteus_spatula |
| [`basisOfRecord`](https://dwc.tdwg.org/terms/#dwc:basisOfRecord) | The specific nature of the data record.  | Pick from these controlled vocabulary terms: [HumanObservation](http://rs.tdwg.org/dwc/terms/HumanObservation), [MachineObservation](http://rs.tdwg.org/dwc/terms/MachineObservation), [MaterialSample](http://rs.tdwg.org/dwc/terms/MaterialSample), [PreservedSpecimen](http://rs.tdwg.org/dwc/terms/PreservedSpecimen), [LivingSpecimen](http://rs.tdwg.org/dwc/terms/LivingSpecimen), [FossilSpecimen](http://rs.tdwg.org/dwc/terms/FossilSpecimen) | HumanObservation |
| [`scientificName`](https://dwc.tdwg.org/terms/#dwc:scientificName) | The full scientific name, with authorship and date information if known. When forming part of an Identification, this should be the name in lowest level taxonomic rank that can be determined. This term should not contain identification qualifications, which should instead be supplied in the `identificationQualifier` term. | Note that cf., aff., etc. need to be parsed out to the `identificationQualifier` term. For a more thorough review of `identificationQualifier` see [this paper](https://doi.org/10.3389/fmars.2021.620702). | Atractosteus spatula |
| [`scientificNameID`](https://dwc.tdwg.org/terms/#dwc:scientificNameID) | An identifier for the nomenclatural (not taxonomic) details of a scientific name. | **Must be a WoRMS LSID for sharing to OBIS. Note that the numbers at the end are the AphiaID from WoRMS.** | urn:lsid:marinespecies.org:taxname:218214 |
| [`eventDate`](https://dwc.tdwg.org/terms/#dwc:eventDate) | The date-time or interval during which an Event occurred. For occurrences, this is the date-time when the event was recorded. Not suitable for a time in a geological context. | Must follow [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601). **See more information on dates in the [Data Cleaning]({{ page.root }}/03-data-cleaning/index.html) section of the workshop.** | 2009-02-20T08:40Z |
| [`decimalLatitude`](https://dwc.tdwg.org/terms/#dwc:decimalLatitude) | The geographic latitude (in decimal degrees, using the spatial reference system given in geodeticDatum) of the geographic center of a Location. Positive values are north of the Equator, negative values are south of it. Legal values lie between -90 and 90, inclusive. | **For OBIS and GBIF the required `geodeticDatum` is WGS84. Uncertainty around the geographic center of a Location (e.g. when sampling event was a transect) can be recorded in `coordinateUncertaintyInMeters`. See more information on coordinates in the [Data Cleaning]({{ page.root }}/03-data-cleaning/index.html) section of the workshop.** | -41.0983423 |
| [`decimalLongitude`](https://dwc.tdwg.org/terms/#dwc:decimalLongitude) | The geographic longitude (in decimal degrees, using the spatial reference system given in geodeticDatum) of the geographic center of a Location. Positive values are east of the Greenwich Meridian, negative values are west of it. Legal values lie between -180 and 180, inclusive | **For OBIS and GBIF the required `geodeticDatum` is WGS84. See more information on coordinates in the [Data Cleaning]({{ page.root }}/03-data-cleaning/index.html) section of the workshop.** | -121.1761111 |
| [`occurrenceStatus`](https://dwc.tdwg.org/terms/#dwc:occurrenceStatus) | A statement about the presence or absence of a Taxon at a Location. | For the ALA, only valid values are `present` and `absent`. | present |
| [`countryCode`](https://dwc.tdwg.org/terms/#dwc:countryCode) | The standard code for the country in which the location occurs. | **Use an [ISO 3166-1-alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country code. Not required for OBIS but GBIF prefers to have this for their system. For international waters, leave blank.** | US, MX, CA |
| [`kingdom`](https://dwc.tdwg.org/terms/#dwc:kingdom) | The full scientific name of the kingdom in which the taxon is classified.| **Not required for OBIS but GBIF needs this to disambiguate scientific names that are the same but in different kingdoms.** | Animalia |
| [`geodeticDatum`](https://dwc.tdwg.org/terms/#dwciri:geodeticDatum) | The ellipsoid, geodetic datum, or spatial reference system (SRS) upon which the geographic coordinates given in decimalLatitude and decimalLongitude as based. | **Must be [WGS84](https://epsg.io/4326) for data shared to OBIS and GBIF but it's best to state explicitly that it is.** | WGS84 |

### What other terms should be considered?
While these terms are not required for publishing data to the ALA, they are extremely helpful for downstream users because without them the data are less useful for future analyses. For instance, `depth` is a crucial piece of information for marine observations, but it is not always included. For the most part the ones listed below are not going to be sitting there in the data, so you'll have to determine what the values should be and add them in. Really try your hardest to include them if you can.

| Darwin Core Term | Definition | Comment | Example |
|------------------|------------------------------------|---------------------------------------|--| 
| [`minimumDepthInMeters`](https://dwc.tdwg.org/terms/#dwc:minimumDepthInMeters) | The lesser depth of a range of depth below the local surface, in meters. | There isn't a single depth value so even if you have a single value you'll put that in both minimum and maximum depth fields. | 0.1 |
| [`maximumDepthInMeters`](https://dwc.tdwg.org/terms/#dwc:maximumDepthInMeters) | The greater depth of a range of depth below the local surface, in meters. | For observations above sea level consider using [`minimumDistanceAboveSurfaceInMeters`](https://dwc.tdwg.org/terms/#dwc:minimumDistanceAboveSurfaceInMeters) and [`maximumDistanceAboveSurfaceInMeters`](https://dwc.tdwg.org/terms/#dwc:maximumDistanceAboveSurfaceInMeters) | 10.5 |
| [`coordinateUncertaintyInMeters`](https://dwc.tdwg.org/terms/#dwc:coordinateUncertaintyInMeters) | 	The horizontal distance (in meters) from the given decimalLatitude and decimalLongitude describing the smallest circle containing the whole of the Location. Leave the value empty if the uncertainty is unknown, cannot be estimated, or is not applicable (because there are no coordinates). Zero is not a valid value for this term | There's always uncertainty associated with locations. Recording the uncertainty is crucial for downstream analyses. | 15 |
| [`samplingProtocol`](https://dwc.tdwg.org/terms/#dwc:samplingProtocol) | The names of, references to, or descriptions of the methods or protocols used during an Event. |  | Bag Seine |
| [`taxonRank`](https://dwc.tdwg.org/terms/#dwc:taxonRank) | The taxonomic rank of the most specific name in the scientificName. | Also helps with disambiguation of scientific names. | Species |
| [`organismQuantity`](https://dwc.tdwg.org/terms/#dwc:organismQuantity) | A number or enumeration value for the quantity of organisms. | OBIS also likes to see this in the Extended Measurement or Fact extension. | 2.6 |
| [`organismQuantityType`](https://dwc.tdwg.org/terms/#dwc:organismQuantityType) | The type of quantification system used for the quantity of organisms. |  | Relative Abundance |
| [`datasetName`](https://dwc.tdwg.org/terms/#dwc:datasetName) | The name identifying the data set from which the record was derived. |  | TPWD HARC Texas Coastal Fisheries Aransas Bag Bay Seine |
| [`dataGeneralizations`](https://dwc.tdwg.org/terms/#dwc:dataGeneralizations) | Actions taken to make the shared data less specific or complete than in its original form. Suggests that alternative data of higher quality may be available on request. | This veers somewhat into the realm of metadata and will not be applicable to all datasets but if the data were modified such as due to sensitive species then it's important to note that for future users. | Coordinates generalized from original GPS coordinates to the nearest half degree grid cell |
| [`informationWithheld`](https://dwc.tdwg.org/terms/#dwc:informationWithheld) | 	Additional information that exists, but that has not been shared in the given record. | Also useful if the data have been modified this way for sensitive species or for other reasons. | location information not given for endangered species |
| [`institutionCode`](https://dwc.tdwg.org/terms/#dwc:institutionCode) | 	The name (or acronym) in use by the institution having custody of the object(s) or information referred to in the record. |  | TPWD |

Other than these specific terms, work through the data that you have and try to crosswalk it to the Darwin Core terms that match best. 


Now that you know what the mapping is between your raw data and the Darwin Core standard, it's time to start cleaning up 
the data to align with the conventions described in the standard. The following activities are the three most common 
conversions a dataset will undergo to align to the Darwin Core standard:
1. [Ensuring dates follow the ISO 8601 standard](#getting-your-dates-in-order)
2. [Matching scientific names to an authoritative resource](#matching-your-scientific-names-to-worms)
3. [Ensuring latitude and longitude values are in decimal degrees](#getting-latlon-to-decimal-degrees)

Below is a short summary of each of those conversions as well as some example conversion scripts. The exercises are 
intended to give you a sense of the variability we've seen in datasets and how we went about converting them. While the
examples use the [pandas package for Python](https://pandas.pydata.org/) and the [tidyverse collection of packages for R](https://www.tidyverse.org/)
(in particular the [lubridate](https://cloud.r-project.org/web/packages/lubridate/lubridate.pdf) package),
those are not the only options for dealing with these conversions but simply the ones we use more frequently in our 
experiences. 


# Getting your dates in order
Dates can be surprisingly tricky because people record them in many different ways. For our purposes we must follow 
[ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) which means using a four digit year, two digit month, and two digit 
day with dashes as separators (i.e. `YYYY-MM-DD`). You can also record time in ISO 8601 but make sure to include the time 
zone which can also get tricky if your data take place across time zones and throughout the year where daylight savings 
time may or may not be in effect (and start and end times of daylight savings vary across years). There are packages in 
R and Python that can help you with these vagaries. Finally, it is possible to record time intervals in ISO 8601 using a 
slash (e.g. `2022-01-02/2022-01-12`). Examine the dates in your data to determine what format they are following and what 
amendments need to be made to ensure they are following ISO 8601. Below are some examples and solutions in Python and R 
for them.

ISO 8601 dates can represent moments in time at different resolutions, as well as time intervals, which use "/" as a separator. Date and time are separated by "T". Timestamps can have a time zone indicator at the end. If not, then they are assumed to be local time. When a time is UTC, the letter "Z" is added at the end (e.g. 2009-02-20T08:40Z, which is the equivalent of 2009-02-20T08:40+00:00). 

> ## Tip 
> Focus on getting your package of choice to read the dates appropriately. While you can use [regular expressions](https://en.wikipedia.org/wiki/Regular_expression)
> to replace and substitute strings to align with the ISO convention, it will typically save you time if you work in 
> your package of choice to translate the dates.
{: .callout}
 
| Darwin Core Term | Description | Example   |
|------------------|-------------|-----------|
| [eventDate](https://dwc.tdwg.org/list/#dwc_eventDate) | The date-time or interval during which an Event occurred. For occurrences, this is the date-time when the event was recorded. Not suitable for a time in a geological context. | `1963-03-08T14:07-0600` (8 Mar 1963 at 2:07pm in the time zone six hours earlier than UTC).<br/>`2009-02-20T08:40Z` (20 February 2009 8:40am UTC).<br/>`2018-08-29T15:19` (3:19pm local time on 29 August 2018).<br/>`1809-02-12` (some time during 12 February 1809).<br/>`1906-06` (some time in June 1906).<br/>`1971` (some time in the year 1971).<br/>`2007-03-01T13:00:00Z/2008-05-11T15:30:00Z` (some time during the interval between 1 March 2007 1pm UTC and 11 May 2008 3:30pm UTC).<br/>`1900/1909` (some time during the interval between the beginning of the year 1900 and the end of the year 1909).<br/>`2007-11-13/15` (some time in the interval between 13 November 2007 and 15 November 2007). |

> ## Examples in Python
> 
> When dealing with dates using pandas in Python it is best to create a Series as your time column with the appropriate 
> datatype. Then, when writing your file(s) using [.to_csv()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_csv.html)
> you can specify the format which your date will be written in using the `date_format` parameter. 
>
> The examples below show how to use the [pandas.to_datetime()](https://pandas.pydata.org/docs/reference/api/pandas.to_datetime.html)
> function to read various date formats. The process can be applied to entire columns (or Series) within a DataFrame.
> <br/>
> 1. `01/31/2021 17:00 GMT`
> 
>    This date follows a typical date construct of `month`**/**`day`**/**`year` `24-hour`**:**`minute` `time-zone`. The 
>    pandas `.to_datetime()` function will correctly interpret these dates without the `format` parameter.
> 
>    ```python
>    import pandas as pd
>    df = pd.DataFrame({'date':['01/31/2021 17:00 GMT']})
>    df['eventDate'] = pd.to_datetime(df['date'], format="%m/%d/%Y %H:%M %Z")
>    df
>    ```
>    ```output
>                       date                 eventDate
>       01/31/2021 17:00 GMT 2021-01-31 17:00:00+00:00
>    ``` 
>    
> 2. `31/01/2021 12:00 EST`
> 
>    This date is similar to the first date but switches the `month` and `day` and identifies a different `time-zone`.
>    The construct looks like `day`**/**`month`**/**`year` `24-hour`**:**`minute` `time-zone`
>    ```python
>    import pandas as pd
>    df = pd.DataFrame({'date':['31/01/2021 12:00 EST']})
>    df['eventDate'] = pd.to_datetime(df['date'], format="%d/%m/%Y %H:%M %Z")
>    df
>    ```
>    ```output
>                       date                 eventDate
>       31/01/2021 12:00 EST 2021-01-31 12:00:00-05:00
>    ``` 
>    
> 3. `January, 01 2021 5:00 PM GMT`
>    
>    ```python
>    import pandas as pd
>    df = pd.DataFrame({'date':['January, 01 2021 5:00 PM GMT']})
>    df['eventDate'] = pd.to_datetime(df['date'],format='%B, %d %Y %I:%M %p %Z')
>    df
>    ```
>    ```output
>                               date                 eventDate
>       January, 01 2021 5:00 PM GMT 2021-01-01 17:00:00+00:00
>    ```
>    
> 4. `1612112400` in seconds since 1970
>    
>    This uses the units of `seconds since 1970` which is common when working with data in [netCDF](https://www.unidata.ucar.edu/software/netcdf/).
>    ```python
>    import pandas as pd
>    df = pd.DataFrame({'date':['1612112400']})
>    df['eventDate'] = pd.to_datetime(df['date'], unit='s', origin='unix')
>    df
>    ```
>    ```output
>             date           eventDate
>       1612112400 2021-01-31 17:00:00
>    ```
> 5. `44227.708333333333`
>    
>    This is the numerical value for dates in Excel because Excel stores dates as sequential serial numbers so that they 
>    can be used in calculations. In some cases, when you export an Excel spreadsheet to CSV, the 
>    dates are preserved as a floating point number.
>    ```python
>    import pandas as pd
>    df = pd.DataFrame({'date':['44227.708333333333']})
>    df['eventDate'] = pd.to_datetime(df['date'].astype(float), unit='D', origin='1899-12-30')
>    df
>    ```
>    ```output
>                     date                     eventDate
>       44227.708333333333 2021-01-31 17:00:00.000000256
>    ```
> 6. Observations with a start date of `2021-01-30` and an end date of `2021-01-31`.
> 
>    Here we store the date as a duration following the ISO 8601 convention. In some cases, it is easier to use a regular 
>    expression or simply paste strings together:
>    ```python
>    import pandas as pd
>    df = pd.DataFrame({'start_date':['2021-01-30'],
>                       'end_date':['2021-01-31']})
>    df['eventDate'] = df['start_time']+'/'+df['end_time']
>    df
>    ```
>    ```output
>       start_time    end_time              eventDate
>       2021-01-30  2021-01-31  2021-01-30/2021-01-31
>    ```
>
{: .solution}

> ## Examples in R
>
> When dealing with dates using R, there are a few base functions that are useful to wrangle your dates in the correct format. An R package that is useful is [lubridate](https://cran.r-project.org/web/packages/lubridate/lubridate.pdf), which is part of the `tidyverse`. It is recommended to bookmark this [lubridate cheatsheet](https://evoldyn.gitlab.io/evomics-2018/ref-sheets/R_lubridate.pdf).
>
> The examples below show how to use the `lubridate` package and format your data to the ISO-8601 standard.
> <br/>
> 1.  `01/31/2021 17:00 GMT`
> 
>    ```r
>    library(lubridate)
>    date_str <- '01/31/2021 17:00 GMT'
>    lubridate::mdy_hm(date_str,tz="UTC")
>    date <- lubridate::format_ISO8601(date) # Separates date and time with a T.
>    date <- paste0(date, "Z") # Add a Z because time is in UTC.
>    ```
>    ```output
>    [1] "2021-01-31T17:00:00Z"
>    ```
> 2. `31/01/2021 12:00 EST`
> 
>    ```r
>    library(lubridate)
>    date_str <- '31/01/2021 12:00 EST'
>    date <- lubridate::dmy_hm(date_str,tz="EST")
>    lubridate::with_tz(date,tz="UTC")
>    date <- lubridate::format_ISO8601(date)
>    date <- paste0(date, "Z")
>    ```
>    ```output
>    [1] "2021-01-31T17:00:00Z"
>    ```
>
> 3. `January, 01 2021 5:00 PM GMT`
>
>    ```r
>    library(lubridate)
>    date_str <- 'January, 01 2021 5:00 PM GMT'
>    date <- lubridate::mdy_hm(date_str, format = '%B, %d %Y %H:%M', tz="GMT")
>    lubridate::with_tz(date,tz="UTC")
>    lubridate::format_ISO8601(date)
>    date <- paste0(date, "Z")
>    ```
>    ```output
>    [1] "2021-01-01T17:00:00Z"
>    ```
>    
> 4. `1612112400` in seconds since 1970
>
>    This uses the units of `seconds since 1970` which is common when working with data in [netCDF](https://www.unidata.ucar.edu/software/netcdf/).
>
>    ```r
>    library(lubridate)
>    date_str <- '1612112400'
>    date_str <- as.numeric(date_str)
>    date <- lubridate::as_datetime(date_str, origin = lubridate::origin, tz = "UTC")
>    date <- lubridate::format_ISO8601(date)
>    date <- paste0(date, "Z")
>    print(date)
>    ```
>    ```output
>    [1] "2021-01-31T17:00:00Z"
>    ```
>    
> 5. `44227.708333333333`
>    
>    This is the numerical value for dates in Excel because Excel stores dates as sequential serial numbers so that they 
>    can be used in calculations. In some cases, when you export an Excel spreadsheet to CSV, the 
>    dates are preserved as a floating point number.
>
>    ```r
>    library(openxlsx)
>    library(lubridate)
>    date_str <- 44227.708333333333
>    date <- as.Date(date_str, origin = "1899-12-30") # If you're only interested in the YYYY-MM-DD
>    fulldate <- openxlsx::convertToDateTime(date_str, tz = "UTC")
>    fulldate <- lubridate::format_ISO8601(fulldate)
>    fulldate <- paste0(fulldate, "Z")
>    print(date)
>    print(fulldate)
>    ```
>    ```output
>    [1] "2021-01-31"
>    [1] "2021-01-31T17:00:00Z"
>    ```
> 6. Observations with a start date of `2021-01-30` and an end date of `2021-01-31`. For added complexity, consider adding in a 4-digit deployment and retrieval time.
> 
>    Here we store the date as a duration following the ISO 8601 convention. In some cases, it is easier to use a regular 
>    expression or simply paste strings together:
>    
>    ```r
>    library(lubridate)
>    event_start <- '2021-01-30'
>    event_finish <- '2021-01-31'
>    
>    deployment_time <- 1002
>    retrieval_time <- 1102
> 
>    Time is recorded numerically (1037 instead of 10:37), so need to change these columns:
>    deployment_time <- substr(as.POSIXct(sprintf("%04.0f", deployment_time), format = "%H%M"), 12, 16)
>    retrieval_time <- substr(as.POSIXct(sprintf("%04.0f", retrieval_time, format = "%H%M"), 12, 16)
>
>    # If you're interested in just pasting the event dates together:
>    eventDate <- paste(event_start, event_finish, sep = "/") 
>
>    # If you're interested in including the deployment and retrieval times in the eventDate:
>    eventDateTime_start <- lubridate::format_ISO8601(as.POSIXct(paste(event_start, deployment_time), tz = "UTC"))
>    eventDateTime_start <- paste0(eventDateTime_start, "Z")
>    eventDateTime_finish <- lubridate::format_ISO8601(as.POSIXct(paste(event_finish, retrieval_time), tz = "UTC"))
>    eventDateTime_finish <- paste0(eventdateTime_finish, "Z")
>    eventDateTime <- paste(eventDateTime_start, eventDateTime_finish, sep = "/") 
>    
>    print(eventDate)
>    print(eventDateTime)
>    ```
>    ```output
>    [1] "2021-01-30/2021-01-31"
>    [1] "2021-01-30T10:02:00Z/2021-01-31T11:02:00Z"
>    ```
{: .solution}

> ## Tip 
> When all else fails, treat the dates as strings and use substitutions/regular expressions to manipulate the strings 
> into ISO 8601. 
{: .callout}

# Matching your scientific names to the ALA
The ALA uses the [Name matching service](https://bie.ala.org.au/search) as the taxonomic backbone for its system. GBIF uses the [Catalog of Life](https://www.catalogueoflife.org/). The key Darwin Core terms that we need are `scientificNameID`, also known as the ALA `taxonConceptID`.

To find the taxa you want, go to the [Name matching service](https://bie.ala.org.au/search) and search either for the vernacular name or the scientific name of your species.

| Darwin Core Term         | Description                                                                       | Example                                               |
|--------------------------|-----------------------------------------------------------------------------------|-------------------------------------------------------|
| [scientificNameID](https://dwc.tdwg.org/list/#dwc_scientificNameID) | An identifier for the nomenclatural (not taxonomic) details of a scientific name. | `urn:lsid:ipni.org:names:37829-1:1.3` |                
| [kingdom](https://dwc.tdwg.org/list/#dwc_kingdom) | The full scientific name of the kingdom in which the taxon is classified.         |   `Animalia`, `Archaea`, `Bacteria`, `Chromista`, `Fungi`, `Plantae`, `Protozoa`, `Viruses` |
| [taxonRank](https://dwc.tdwg.org/list/#dwc_taxonRank) | The taxonomic rank of the most specific name in the scientificName.               | `subspecies`, `varietas`, `forma`, `species`, `genus` |

> ## Using the ALA Name Matching Tool
> Go to [Name matching service](https://bie.ala.org.au/search) and type either the scientific or common name of your species.
>    Hit `search` and find the result that is closest to your species
>    ![screenshot]({{ page.root }}/fig/species_file_screenshot.png){: .image-with-shadow }
>
> Ensure your species name in your data matches that of the one on the ALA.
{: .solution}

# Getting lat/lon to decimal degrees

Latitude (`decimalLatitude`) and longitude (`decimalLongitude`) are the geographic coordinates (in decimal degrees north and east, respectively), using the spatial reference system given in `geodeticDatum` of the geographic center of a location.
* `decimalLatitude`, positive values are north of the Equator, negative values are south of it. All values lie between -90 and 90, inclusive. 
* `decimalLongitude`, positive values are east of the Greenwich Meridian, negative values are west of it. All values lie between -180 and 180, inclusive.

Note, that the requirement for `decimalLatitude` and `decimallLongitude` is they must be in decimal degrees in [WGS84](https://en.wikipedia.org/wiki/World_Geodetic_System). Since this is the requirement for Darwin Core, **OBIS and GBIF will assume data shared using those Darwin Core terms are in the geodetic datum `WGS84`**. We highly recommend checking the coordinate reference system (CRS) of your observations to confirm they are using the same datum and documenting it in the `geodeticDatum` Darwin Core term. If your coordinates are not using `WGS84`, they will need to be converted in order to share the data to OBIS and GBIF since `decimalLatitude` and `decimalLongitude` are required terms. 

Helpful packages for managing CRS and geodetic datum:
* python: [GeoPandas](https://geopandas.org/en/stable/getting_started.html) has a [utility](https://geopandas.org/en/stable/docs/user_guide/projections.html#re-projecting).
* R: [rgdal](https://cran.r-project.org/web/packages/rgdal/index.html) and [sp](https://cran.r-project.org/web/packages/sp/index.html).

> ## Tip 
> If at all possible, it's best to extract out the components of the information you have in order to compile the 
> appropriate field. For example, if you have the coordinates as one lone string `17° 51' 57.96" S 149° 39' 13.32" W`, 
> try to split it out into its component pieces: `17`, `51`, `57.96`, `S`, `149`, `39`, `13.32`, and `W` just be sure to 
> track which values are latitude and which are longitude.
{: .callout}

| Darwin Core Term | Description | Example        |
|------------------|-------------|----------------|
| [decimalLatitude](https://dwc.tdwg.org/list/#dwc_decimalLatitude) | The geographic latitude (in decimal degrees, using the spatial reference system given in geodeticDatum) of the geographic center of a Location. Positive values are north of the Equator, negative values are south of it. Legal values lie between -90 and 90, inclusive. | `-41.0983423`  |
| [decimalLongitude](https://dwc.tdwg.org/list/#dwc_decimalLongitude) | The geographic longitude (in decimal degrees, using the spatial reference system given in geodeticDatum) of the geographic center of a Location. Positive values are east of the Greenwich Meridian, negative values are west of it. Legal values lie between -180 and 180, inclusive. | `-121.1761111` |
| [geodeticDatum](https://dwc.tdwg.org/list/#dwc_geodeticDatum) | The ellipsoid, geodetic datum, or spatial reference system (SRS) upon which the geographic coordinates given in decimalLatitude and decimalLongitude as based. | `WGS84` |

![coordinate_precision](https://imgs.xkcd.com/comics/coordinate_precision.png){: .image-with-shadow }
*Image credit: [xkcd](https://xkcd.com/)*

> ## Examples in Python
> 
> 1. `17° 51' 57.96" S` `149° 39' 13.32" W`
>    * This example assumes you have already split the two strings into discrete components (as shown in the table). An 
>      example converting the full strings `17° 51' 57.96" S` `149° 39' 13.32" W` to decimal degrees can be found [here](https://github.com/MathewBiddle/misc/blob/07d643da831255069fd1f6e936ca0902e21c0d0c/data302_DON_Oxidation_working_hollibaugh_20190514_process.py#L24-L62).
> 
>    lat_degrees | lat_minutes | lat_seconds | lat_hemisphere | lon_degrees | lon_minutes | lon_seconds | lon_hemisphere
>    ------------|----|-------------|----------------|-------------|-------------|-------------|---------------
>    17 | 51 | 57.96 | S | 149 | 39 | 13.32 | W
> 
>    ```python
>    df = pd.DataFrame({'lat_degrees':[17],
>                       'lat_minutes':[51],
>                       'lat_seconds':[57.96],
>                       'lat_hemisphere':['S'],
>                       'lon_degrees': [149], 
>                       'lon_minutes': [39], 
>                       'lon_seconds':[13.32], 
>                       'lon_hemisphere': ['W'],
>                      })
>    
>    df['decimalLatitude'] = df['lat_degrees'] + ( (df['lat_minutes'] + (df['lat_seconds']/60) )/60)
>    df['decimalLongitude'] = df['lon_degrees'] + ( (df['lon_minutes'] + (df['lon_seconds']/60) )/60)
> 
>    # Convert hemisphere S and W to negative values as units should be `degrees North` and `degrees East`
>    df.loc[df['lat_hemisphere']=='S','decimalLatitude'] = df.loc[df['lat_hemisphere']=='S','decimalLatitude']*-1
>    df.loc[df['lon_hemisphere']=='W','decimalLongitude'] = df.loc[df['lon_hemisphere']=='W','decimalLongitude']*-1
>       
>    df[['decimalLatitude','decimalLongitude']]
>    ```
>    ```output
>       decimalLatitude  decimalLongitude
>              -17.8661         -149.6537
>    ```
>
> 2. `33° 22.967' N` `117° 35.321' W`
>    * Similar to above, this example assumes you have already split the two strings into discrete components (as shown 
>      in the table).
>  
>    lat_degrees | lat_dec_minutes | lat_hemisphere | lon_degrees | lon_dec_minutes | lon_hemisphere
>    ------------|-----------------|----------------|-------------|-----------------|---------------
>    33 | 22.967 | N | 117 | 35.321 | W
> 
>    ```python
>    df = pd.DataFrame({'lat_degrees':[33],
>                       'lat_dec_minutes':[22.967],
>                       'lat_hemisphere':['N'],
>                       'lon_degrees': [117], 
>                       'lon_dec_minutes': [35.321], 
>                       'lon_hemisphere': ['W'],
>                      })
>    
>    df['decimalLatitude'] = df['lat_degrees'] + (df['lat_dec_minutes']/60)
>    df['decimalLongitude'] = df['lon_degrees'] + (df['lon_dec_minutes']/60)
>    
>    # Convert hemisphere S and W to negative values as units should be `degrees North` and `degrees East`
>    df.loc[df['lat_hemisphere']=='S','decimalLatitude'] = df.loc[df['lat_hemisphere']=='S','decimalLatitude']*-1
>    df.loc[df['lon_hemisphere']=='W','decimalLongitude'] = df.loc[df['lon_hemisphere']=='W','decimalLongitude']*-1
>    
>    df[['decimalLatitude','decimalLongitude']]
>    ```
>    ```output
>    decimalLatitude  decimalLongitude
>    0        33.382783       -117.588683
>    ```
> 
{: .solution}

> ## Examples in R
> 1. `17° 51' 57.96" S` `149° 39' 13.32" W`
>
>    lat_degrees | lat_minutes | lat_seconds | lat_hemisphere | lon_degrees | lon_minutes | lon_seconds | lon_hemisphere
>    ------------|----|-------------|----------------|-------------|-------------|-------------|---------------
>    17 | 51 | 57.96 | S | 149 | 39 | 13.32 | W
> 
>    ```r
>    library(tibble)
>    tbl <- tibble(lat_degrees = 17,
>                  lat_minutes = 51,
>                  lat_seconds = 57.96,
>                  lat_hemisphere = "S",
>                  lon_degrees = 149,
>                  lon_minutes = 39, 
>                  lon_seconds = 13.32, 
>                  lon_hemisphere = "W")
>    
>    tbl$decimalLatitude <- tbl$lat_degrees + ( (tbl$lat_minutes + (tbl$lat_seconds/60)) / 60 )
>    tbl$decimalLongitude <- tbl$lon_degrees + ( (tbl$lon_minutes + (tbl$lon_seconds/60)) / 60 )
>    
>    tbl$decimalLatitude = as.numeric(as.character(tbl$decimalLatitude))*(-1)
>    tbl$decimalLongitude = as.numeric(as.character(tbl$decimalLongitude))*(-1)
>    ```
>    ```output
>    > tbl$decimalLatitude
>    [1] -17.8661
>    > tbl$decimalLongitude
>    [1] -149.6537
>   ```
>    
>    
> 2. `33° 22.967' N` `117° 35.321' W` 
>
>    lat_degrees | lat_dec_minutes | lat_hemisphere | lon_degrees | lon_dec_minutes | lon_hemisphere
>    ------------|-----------------|----------------|-------------|-----------------|---------------
>    33 | 22.967 | N | 117 | 35.321 | W
>
>    ```r
>    library(tibble)
>    tbl <- tibble(lat_degrees = 33,
>                  lat_dec_minutes = 22.967,
>                  lat_hemisphere = "N",
>                  lon_degrees = 117, 
>                  lon_dec_minutes = 35.321, 
>                  lon_hemisphere = "W")
>    
>    tbl$decimalLatitude <- tbl$lat_degrees + ( tbl$lat_dec_minutes/60 )
>    tbl$decimalLongitude <- tbl$lon_degrees + ( tbl$lon_dec_minutes/60 )
>    
>    tbl$decimalLongitude = as.numeric(as.character(tbl$decimalLongitude))*(-1)
>    ```
>    ```output
>    > tbl$decimalLatitude
>    [1] 33.38278
>    > tbl$decimalLongitude
>    [1] -117.5887
>    ```
> 
> 3. `33° 22.967' N` `117° 35.321' W`
>    * Using the [measurements package](https://cran.r-project.org/web/packages/measurements/measurements.pdf) the `conv_unit()` can work with space separated strings for coordinates.
>
>    lat | lat_hemisphere | lon | lon_hemisphere
>    ----|----------------|-----|---------------
>    33 22.967 | N | 117 35.321 | W
>    
>   ```r
>    tbl <- tibble(lat = "33 22.967",
>                  lat_hemisphere = "N",
>                  lon = "117 35.321", 
>                  lon_hemisphere = "W")
>   
>   tbl$decimalLongitude = measurements::conv_unit(tbl$lon, from = 'deg_dec_min', to = 'dec_deg')
>   tbl$decimalLongitude = as.numeric(as.character(tbl$decimalLongitude))*(-1)
>   
>   tbl$decimalLatitude = measurements::conv_unit(tbl$lat, from = 'deg_dec_min', to = 'dec_deg')
>   ``` 
>   ```output
>    > tbl$decimalLatitude
>    [1] 33.38278
>    > tbl$decimalLongitude
>    [1] -117.5887
>   ```
{: .solution}

{% include links.md %}
  