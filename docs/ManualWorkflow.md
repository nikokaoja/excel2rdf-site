# Manual Workflow

This page describes the manual process of:

- creating a vocabulary of controlled terms in Excel
- converting that vocabulary into a semantic standard known as [SKOS](https://www.w3.org/TR/skos-primer/),
- and if desired, submitting the result into the [BioPortal](https://bioportal.bioontology.org/) ontology repository (or any OntoPortal-compatible repository).

To perform this process, you don't have to set up a GitHub repo or other automated system.
Instead, you can perform each step manually, inspecting the results and making necessary changes
before going on to the next step.

Once you have a baseline ontology and confirm the process works as expected,
you may want to automate the process,
Instructions for automating the process are described in [Automatic Workflow](./AutomaticWorkflow.md) page.

## Choosing your template

The core of the transformation is engineered by software from [SKOS Play](https://labs.sparna.fr/skos-play/),
and in particular its [xls2rdf](https://labs.sparna.fr/skos-play/convert), which converts Excel sheets to SKOS lexicalized RDF files (e.g., in [Turtle format](<https://en.wikipedia.org/wiki/Turtle_(syntax)>)).
Developer [Thomas Francart](https://github.com/tfrancart) of Sparna Labs developed this sophisticated tool
to address a wide range of vocabulary representation and needs,
and it is available both as a web service (described here) and as a command-line Java app(see [Automatic Workflow](./AutomaticWorkflow.md) page).

To begin, the user needs to choose an appropriate spreadsheet for representing their vocabularies.
There are 8 examples on [the SKOS Play web site](https://labs.sparna.fr/skos-play/convert#excel-file-structure), each illustrating different features of the system.
Examples 4 and 7 are not intended to produce rigorous SKOS outputs, and can be ignored.

Each example template contains a description of the purpose for which the template is intended.

Here are 3 examples you could start with to create your template.

### Example 2 from SKOS Play Convert
Use the drop-down menu to choose one of the example files.

### Enhanced Metadata version of Example 2

We created a generic [Enhanced Template based on SKOS Play Example 2](https://github.com/fair-data-collective/excel2rdf-template/blob/main/vocabulary.xlsx).
We primarily enhanced the template's metadata (at the top of the file), so that your vocabulary would be well described. 
You have to replace most of the values in the header section with values appropriate to your vocabulary.

Read the next subsection to learn about customizations for multiple hierarchies.

### Detailed working example

The Excel file in [this directory](https://github.com/fair-data-collective/zonmw-project-admin/tree/main/ontology) contains both the Excel `.xlsx` source, 
and the Turtle-formatted SKOS ontology that resulted. 
A primary improvement in this example is that the identifiers are automatically generated from the labels.
If you want to create different identifiers you can simply override the spreadsheet's formula for generating the identifiers.
(There are many reasons it might not be appropriate for the identifiers to be generated from the labels, 
which we will not explore here.)

You can see from this working example that theskos:broader column is empty for all the top-level terms.
The SKOS PLAY conversion infers that a term is at the top level of the class tree if this column is empty,
and issues the appropriate RDF triples to make that clear in the resulting ontology.
This approach lets you manage a number of different term lists within the same overarching ontology.

## Converting your file to SKOS using SKOS Play

Once you have chosen your desired template, you can edit its content,
then submit it to the [SKOS Play site for conversion](https://labs.sparna.fr/skos-play/convert).
The site produces a SKOS file in various formats—we recommend Turtle (TTL) for readability—
and displays the results in a window.
You can copy this text and paste it into a file, whose extension should be .ttl for the Turtle format
and .rdf for the XML/RDF format.

## Validation

Several SKOS validation sites exist, in case you want to perform additional validations or quality checks beyond what SKOS Play provides.
For example, [SKOSify](https://github.com/NatLibFi/Skosify) and [qSKOS](https://github.com/cmader/qSKOS) are two examples of such validators,
although the latter requires some installation.

## Submission to BioPortal or OntoPortal

[BioPortal](https://bioportal.bioontology.org) (its web site is at https://bioontology.org) allows anyone to submit an ontology.
Simply visit the [Ontologies page]((https://bioportal.bioontology.org/ontologies) and click on the Submit New Ontology button.
(The site requests that ontologies that are not of value for public use be submitted as private resources,
but there are no constraints on ontology submissions based on their subject.)

When you submit your ontology, be sure to select the `SKOS` option for the ontology type.
The ontology will still parse if the OWL format is selected, but will not treat the SKOS concepts as classes on the BioPortal site,
so they will not be visible in the UI or in response to the classes API call.

You can verify your terms are visible to the public by visiting the Classes tab and viewing the tree that's shown in that tab.

You may have to wait some time before your ontology and its classes become fully visible in BioPortal. 
If the classes are not visible by several hours after submission,
first double-check that your .ttl file is valid, as described above.
If it is, you can get help by emailing support@bioontology.org.

## Making Private Ontology Externally Accessible

If your ontology Viewing Restrictions field is set to Private, 
but you want the ontology to be visible to an external system (like the [CEDAR Workbench](https://metadatacenter.org)),
you must explicitly give your external application or user permission to access the ontology using their own account's API key.
To do so for the CEDAR Workbench tool, enter `cedar`, `cedar-mjd`, `cedar-test`, and `cedar-public` in the Viewing Restrictions field for those accounts.

To validate that the classes are visible to CEDAR, use the command:
```
https://data.bioontology.org/ONT_ACRONYM/classes
```
replacing `ONT_ACRONYM` with your ontology acronym. Potentially add your API key to the call if you haven't done that so before (replace `YOUR_API_KEY` with your API key):
```
https://data.bioontology.org/ONT_ACRONYM/classes?apikey=YOUR_API_KEY
```

You should see the terms of your ontology within the JSON response.

It takes as many as 7 hours for the ontology to be detected in CEDAR once it is published by BioPortal.

## Automation

To automate this set of processes, please see the [Automatic Workflow](./AutomaticWorkflow.md) page.
