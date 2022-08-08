
<!-- README.md is generated from README.Rmd. Please edit that file -->

# EMLeditor

##### v0.1.0-beta

##### “Lilly Lake Loop”

<!-- badges: start -->
<!-- badges: end -->

The goal of EMLeditor is to edit EML-formatted xml files. Specifically,
EMLeditor provides many functions that will be useful to the U.S.
National Park Service when generating metadata for statistical data
packages uploaded to DataStore. NPS affiliation is assumed as default.
However, some of the functions for viewing and editing metadata may be
useful to people outside the NPS.

EMLeditor’s primary objective is to edit and view EML formatted files,
not to generate them from scratch. A suggested workflow is:

1)  Use the EMLassemblyline::make_eml() to generate an initial EML
    document and save it as a .xml file (NPS naming convention is:
    \*\_metadata.xml)
2)  Use the EML::read_eml() function to load your EML file into R as an
    R object.
3)  Use EMLeditor functions to edit the metadata in R and evaluate
    whether your metadata is acceptable.
4)  Use the EML::write_eml() function to write the R object back to XML
    (remember the NPS naming convention for metadata files is
    \*\_metadata.xml).

If you use EMLeditor functions they will silently add the National Park
Service as a publisher to your metadata unless you set NPS=FALSE.

EMLeditor will also add information about the version of EMLeditor you
used to your metadata.

## Installation

You can install the development version of EMLeditor from
[GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
devtools::install_github("nationalparkservice/EMLeditor")
```

## Example 1:

This is a basic workflow for adding a Digital Object Identifier:

``` r
library(EMLeditor)
library(EML)

#load a pre-existing EML-formated xml file:
my_eml<-EML::read_eml("EML_metadata.xml", from="xml")

#the 7-digit number is your DataStore Reference number automatically generated when you initiate a draft Reference.
#Your DOI is reserved but will not be registered/activated until publication.
my_eml2<-set.DOI(my_eml, "1234567")

#write the new R object back to XML:
write_eml(my_eml2, "EML_metadata.xml")
```

## Example 2:

When you upload a data package (data files and \*\_metadata.xml) to
DataStore, DataStore will (or will soon) automatically create a
readme.txt file to accompany your data package. You can generate a
*mock* readme.txt file to see whether some of the basic metadata in your
\*\_metadata.xml file is correct. There is no need to upload this *mock*
file. It is just a tool for you to inspect your metadata. The actual
readme generated by DataStore may deviate slightly.

``` r
#load a pre-existing EML-formated xml file:
my_eml<-EML::read_eml("my_EML_metadata.xml", from="xml")

#Do some stuff with your EML metadata in R.
#For instance, add a literature cited section.
#bibtex_file is a file with your literature cited in bibtex format, typically with a .bib suffix.
#Set NPS=FALSE if you do NOT want NPS listed as the publisher. 
#If NPS is the publisher, this defaults to TRUE and need not be specified.
my_eml2<-set.lit(my_eml, bibtex_file, NPS=FALSE)

#writes a .txt file to your working directory.
#FYI Lit cited doesn't show up in the mock readme
write.readMe(my_eml2, "MockReadMe.txt")

#Assumig you're happy with the readme file generated, write the object back to XML:
EML::write_eml(my_eml2, "my_EML2_metadata.xml")
```
