# Tools

These are the tools (linked to their respective sites) that are involved in the Excel-to-RDF (SKOS) conversion:

- [SKOS Play Convert](https://labs.sparna.fr/skos-play/convert)
- [qSKOS](https://github.com/cmader/qSKOS)
- [BioPortal](https://bioportal.bioontology.org/)
- [GitHub actions](https://github.com/features/actions)

Each is briefly explained in their sections below.  You may also want to look at these templates:

- Example 2 from SKOS Play Convert: use the drop-down menu to choose one of the example files.
- [Enhanced Template based on SKOS Play](https://github.com/fair-data-collective/excel2rdf-template/blob/main/vocabulary.xlsx): example #2 from SKOS Play Convert with enhanced metadata
- [Working Example](https://github.com/fair-data-collective/zonmw-project-admin/tree/main/ontology): This directory contains both the Excel `.xlsx` source, and the Turtle-formatted SKOS ontology that resulted. 

If you are working a lot with SKOS and vocabularies, please visit the other example templates in SKOS Play Convert 
(using the drop-down menu to select the different examples) for nice Excel-based solutions to some tricky vocabulary maintenance tasks.

## Excel

I don't need to tell you about Excel, do I?  This process uses just plain vanilla Excel, though
you can dress up your own template however you want to make it easier to maintain.

## SKOS Play Convert

SKOS Play is an [interactive site](https://labs.sparna.fr/skos-play) with many fun operations you can perform on SKOS files.
It takes a principled approach to the SKOS conversions you might want to perform and is well documented.

The Convert page of the site converts Excel files that follow many different template patterns
and converts them into RDF. All of the example templates convert into well-mannered SKOS RDF files (see BioPortal section below)
except for examples 4 and 7, which produce other RDF things.

You will quickly see how you can build different types of SKOS RDF files, including multi-list vocabularies.
We focused on Example 2 and added to it to create a hierarchical SKOS file that could play well with BioPortal.

## qSKOS

qSKOS is a quality evaluation library for SKOS RDF Files. It is usable from within an existing freely available tool (that requires installation), 
or can be installed in a GitHub actions workflow as we describe elsewhere in this tutorial.

## BioPortal

BioPortal is an extensive ontology repository that allows community submissions, 
and offers support for SKOS files that are formatted to follow some [good practice recommendations](https://www.bioontology.org/wiki/SKOSSupport).
This allows BioPortal to recognize and present the SKOS individuals as classes in its class tree,
so users can see and select the terms.

BioPortal can be configured to upload an ontology file from a URL whenever it changes. 
(The change detection takes place nightly.) 
Scripts could also be configured to push the file whenever it changes to BioPortal's submission API, but we have not implemented this.

The Working Example linked above has been submitted to BioPortal as the [ZONMW-ADMIN-MD ontology](http://bioportal.bioontology.org/ontologies/ZONMW-ADMIN-MD).
Click on the [Classes tab](http://bioportal.bioontology.org/ontologies/ZONMW-ADMIN-MD/?p=classes&conceptid=root) to see the terms.

## GitHub actions

The steps of the process can be executed manually as described in the [Manual Workflow section](../ManualWorkflow). 
If you want to automate the process, you can use GitHub actions to do so.
This is described in the [Automatic Workflow section](../AutomaticWorkflow). 

The conversion from Excel spreadsheet to Turtle SKOS file is typically complete within 2 minutes 
from the time the Excel spreadsheet is submitted to GitHub.
