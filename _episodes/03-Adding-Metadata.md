---
title: "Adding Metadata"
teaching: 0
exercises: 120
questions:
- "What does EML mean?"
- "How do I format the metadata files?"
- "What metadata are required for publishing with the ALA?"
objectives:
- "Introduction to Ecological Metadata Language (EML)"
- "Formatting the `eml.xml` and `meta.xml` files"
- "Know which fields are required for data submission to the ALA"
keypoints:
- "Know what Ecological Metadata Language is"
- "Know how to correctly generate metadata files"
- "Strive to write more than the minimal metadata"
---

# Ecological Metadata Language (EML)

Both ALA and GBIF use [Ecological Metadata Language (EML)](https://eml.ecoinformatics.org/) as the metadata standard associated with the data. For this workshop, we will give you a template EML file that you can save and edit in the future. 

With the `eml.xml` file, you will need to ensure you have downloaded a program that will allow you to edit this file.  This is because, while the EML file does contain text, it needs to be computer-readable as well.  Examples of these types of programs are: Emacs, Visual Studio Code, Spyder, and RStudio.

When including metadata files, the more information the better!  The ALA requires at minimum the following fields: `title`, `abstract`, `citation`, and several `contacts`. 

More information on EML can be found at the [EML standard page](https://eml.ecoinformatics.org/), and in the [bio data guide](https://ioos.github.io/bio_data_guide/extras.html#ecological-metadata-language-eml). There are also a number of R packages for working with EML, reviewed [here](https://livingnorway.github.io/LivingNorwayR/articles/EML_R_packages_overview.html).

> ## Tip 
> Try to collect as much of this information as possible before and during the Darwin Core alignment process.  This will make it easier when publishing data in the ALA.
{: .callout}

## Required EML metadata fields for sharing to ALA

| EML Fields | Definition | Comment |
| ---------- | ---------- | ------- |
| `Title` | A good descriptive title is indispensable and can provide the user with valuable information, making the discovery of data easier. | |
| `Abstract` | The abstract or description of a dataset provides basic information on the content of the dataset. The information in the abstract should improve understanding and interpretation of the data.| |
| `Data License` | The licence that you apply to the resource. The license provides a standardized way to define appropriate uses of your work. | Must use CC-0, CC-BY, or CC-BY-NC. ADD DESCRIPTION HERE. |
| `Resource Contact(s)` | The list of people and organizations that should be contacted to get more information about the resource, that curate the resource or to whom putative problems with the resource or its data should be addressed. | Last name, Postition, and Organization are required, helpful to include an ORCID and a contact method like email or phone number. |
| `Resource Creator(s)` | The people and organizations who created the resource, in priority order. The list will be used to auto-generate the resource citation (if auto-generation is turned on). | |
| `Metadata Provider(s)` | the people and organizations responsible for producing the resource metadata. | |
| `Citation` | The dataset citation allows users to properly cite the datasets in further publications or other uses of the data. The OBIS download function provides a list of the dataset citations packaged with the data in a zipped file. | |

## Other EML fields to consider

| EML Fields               | Definition | Comment |
|--------------------------|------------|---------|
| `Bounding Box`           | Farthest North, South, East, and West coordinate. |  |
| `Geographic Description` | A textual description of the geographic coverage.  |  |
| `Temporal Coverage`      | This can either be a Single Date, Date Range, Formation Period, or Living Time Period. |  |
| `Study Extent`           | This field represents both a specific sampling area and the sampling frequency (temporal boundaries, frequency of occurrence) of the project. |  |
| `Sampling Description`   | This field allows for a text-based/human readable description of the sampling procedures used in the research project. | The content of this element would be similar to a description of sampling procedures found in the methods section of a journal article.  |
| `Step Description`       | This field allows for repeated sets of elements that document a series of methods and procedures used in the study, and the processing steps leading to the production of the data files. These include e.g. text descriptions of the procedures, relevant literature, software, instrumentation and any quality control measurements taken. | Each method should be described in enough detail to allow other researchers to interpret and repeat the study, if required. |

{% include links.md %}