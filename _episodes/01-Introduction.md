---
title: "Introduction to Darwin Core"
start: true
teaching: 10
exercises: 5
questions:
- "What is Darwin Core?"
- "What is a Darwin Core Archive?"
- "Why do people use Darwin Core for their data?"
objectives:
- "Understand the purpose of Darwin Core."
- "Understand how to map data to Darwin Core."
- "Plan for mapping to Darwin Core."
keypoints:
- "Using Darwin Core allows datasets from across projects, organizations, and countries to be integrated together."
- "Applying certain general principles to the data will make it easier to map to Darwin Core."
- "Implementing Darwin Core makes data FAIR-er and means becoming part of a community of people working together to understand species no matter where they work or are based."
---

# Darwin Core - A global community of data sharing and integration
Darwin Core is a data standard to mobilize and share biodiversity data. Over the years, the Darwin Core standard has expanded to enable exchange and sharing of diverse types of biological observations from citizen scientists, ecological monitoring, eDNA, animal telemetry, taxonomic treatments, and many others. Darwin Core is applicable to any observation of an organism (scientific name, OTU, or other methods of defining a species) at a particular place and time. In Darwin Core this is an `occurrence`. To learn more about the foundations of Darwin Core read [Wieczorek et al. 2012](https://doi.org/10.1371/journal.pone.0029715).

### Demonstrated Use of Darwin Core
The power of Darwin Core is most evident in the data aggregators that harvest data using that standard. The one we will refer to most frequently in this workshop is the [Atlas of Living Australia](https://ala.org.au/). Another prominent one is the [Global Biodiversity Information Facility](https://www.gbif.org/) (learn more about [GBIF](https://vimeo.com/434831655)). It's also used by the Ocean Biodiversity Information System, iDigBio, among others. 

### Darwin Core Archives
Darwin Core Archives are the data format the ALA harvests into our systems. An "Archive" is a file format which compresses large amounts of files and data into one smaller file, allowing the ALA to store lots of data in a small amount of space.  The "Darwin Core" part of the Darwin Core Archive means your "Archive" contains the following:

1. Raw `occurrence` data, such as the latitude and longitude of a particular species you observed (these will have a `.txt` extension)
2. Metadata files in an `XML` format (ensuring they are both human and computer-readable) describing the following:
    i. What files are included in the archive (titled `meta.xml`)
    ii. an Ecological Metadata Language XML file, titled `eml.xml`.

The `meta.xml` file contains a list of what files are included in the archive, and the `eml.xml` file contains metadata about the data included in the archive, such as the license for the data, etc. 

> ## Exercise
> HAVE TEMPLATE DARWIN CORE ARCHIVE HERE
> Challenge: Download this [Darwin Core Archive](https://ipt-obis.gbif.us/archive.do?r=tpwd_harc_texasaransasbay_bagseine&v=2.3) and examine what's in it. Did you find anything unusual or that you don't understand what it is?
> 
> > ## Solution
> > ```Folder
> >  dwca-tpwd_harc_texasaransasbay_bagseine-v2.3
> >  |-- eml.xml
> >  |-- event.txt
> >  |-- extendedmeasurementorfact.txt
> >  |-- meta.xml
> >  |-- occurrence.txt
> > ```
> {: .solution}
{: .challenge}