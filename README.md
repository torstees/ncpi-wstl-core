# ncpi-wstl-core
NCPI Core whistle project library.

[Whistle](https://github.com/GoogleCloudPlatform/healthcare-data-harmonization) is google's Data Transformation Language which can be used to transform arbitrary JSON objects into FHIR compliant JSON objects. For the purposes of reuse, a core set of whistle functions will be developed to export transformed whistle objects into FHIR. Those objects shall be defined according to strict rules defined herein ensure that the FHIR output conforms to the [NCPI FHIR IG](https://github.com/NIH-NCPI/ncpi-fhir-ig).

# Expected use of this library
While the source data will vary greatly from one dataset to the next, the end product should be as uniform as possible. As such, we'll define a single set of functions to build the final FHIR representations with the expectation that those responsible for performing the transformation will write projections to fit the given data values into the core model configuration (specified below) and pass those objects to the specified functions. 

With this in mind, it is expected that data ingest teams will fork this library into dataset specific projects and their transform projects in as needed. Those forks will remain completely independent of the core library (i.e. no pull requests or attempts to commit dataset specific updates), but may pull updated versions down if updates are required. 

# Supplemtal Scripts
There will be a small amount of scripting that (will be) provided to support individual transformations once the dataset specific projects are ready. 

## Wrapper Script
(TBD) Python script that will transform the CSV file into suitable json objects and transformed by whistle and optionally submit to a FHIR server. 

## Build Concept Map 
(TBD) Takes an easy to edit CSV format [similar to this but more compelte](https://github.com/GoogleCloudPlatform/healthcare-data-harmonization/blob/master/mapping_configs/omop_fhir_r4/code_harmonization/OMOP-FHIR-ConceptMap.csv) and builds up a functional FHIR concept map that will be used to assist in transforming the the values if a suitable concept map doesn't already exist. 

# Key Whistle Objects
Objects passed to these core functions must conform to the specified form. All keywords conform to [snake_case](https://en.wikipedia.org/wiki/Snake_case). For certain parameters, null/missing is OK. However, for certain profiles, there are hard requirements and those will be enforced. 

## Participant
Because we are dealing with research data, we'll use Participant to differentiant from FHIR's Patient. 

| Core Library Term | Description | FHIR Target | Values/Format Accepted |
| participant_id | The subject/participant ID | (required) This will be used as the ID for the patient object and all references to it within the final bundle |
| dob | Date of Birth | (optional) This may be a complete date or just the year | YYYY-MM-DD  |
| gender | Gender | (optional) Administrative gender | [hl7 administrative gender](http://hl7.org/fhir/R4/valueset-administrative-gender.html) male, female, other, unknown | |
| dod | Date Deceased | (optional) This may be a complete date or just the year | YYYY-MM-DD |
