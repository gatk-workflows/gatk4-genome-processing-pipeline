# Germline alignment and variant calling pipeline on Azure
This repository is an example of running the germline alignment and variant calling pipeline, based on Best Practices Genome Analysis Pipeline by Broad Institute of MIT and Harvard, on Cromwell on Azure.<br/> 

Learn more about using Azure for your Cromwell WDL workflows on our GitHub repo! - [Cromwell on Azure](https://github.com/microsoft/CromwellOnAzure).<br/>

This repository is a fork from [the original](https://github.com/gatk-workflows/gatk4-genome-processing-pipeline) and has all the required changes to run the WDL workflow on Cromwell on Azure.<br/>

Here, you can find the WDL file and an example inputs JSON file with links to data hosted on a public Azure Storage account. You can use the "msgenpublicdata" storage account directly as a relative path, like in the inputs JSON file. 

The `WholeGenomeGermlineSingleSample.trigger.json` trigger file is ready to use. 

### Host tutorial data on your Storage account
If you prefer to host this data on your own Storage account, you can use [AzCopy](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-blobs#copy-a-container-to-another-storage-account) to transfer the entire blob container with the required files to your own Storage account [using a shared access signature](https://docs.microsoft.com/en-us/azure/storage/common/storage-sas-overview) with "Write" access.<br/>

```
.\azcopy.exe copy 'https://msgenpublicdata.blob.core.windows.net:443/inputs?sv=2015-04-05&sr=c&si=coa&sig=pENt%2FMMOj24uoNBZIPLa%2BNVVkvopcFK51rwADyYLEPE%3D' 'https://<destination-storage-account-name>.blob.core.windows.net/inputs?<WriteSAS-token>' --recursive --s2s-preserve-access-tier=false
```

Replace all instances of `/msgenpublicdata/inputs/` with your `/destination-storage-account-name/inputs/` in the inputs JSON file.

The `WholeGenomeGermlineSingleSample.trigger.json` file is an example. Substitute the "WorkflowInputsUrl" with the http link to your inputs JSON file hosted on your Storage account.


## gatk4-genome-processing-pipeline
Workflows used for germline processing in whole genome sequence data.

### WholeGenomeGermlineSingleSample :
This WDL pipeline implements data pre-processing and initial variant calling (GVCF
generation) according to the GATK Best Practices (June 2016) for germline SNP and
Indel discovery in human whole-genome sequencing data.

#### Requirements/expectations
- Human whole-genome paired-end sequencing data in unmapped BAM (uBAM) format
- One or more read groups, one per uBAM file, all belonging to a single sample (SM)
- Input uBAM files must additionally comply with the following requirements:
- - filenames all have the same suffix (we use ".unmapped.bam")
- - files must pass validation by ValidateSamFile
- - reads are provided in query-sorted order
- - all reads must have an RG tag
- Reference genome must be Hg38 with ALT contigs

#### Outputs 
- Cram, cram index, and cram md5 
- GVCF and its gvcf index 
- BQSR Report
- Several Summary Metrics 

### Software version requirements :
- GATK 4.0.10.1
- Picard 2.20.0-SNAPSHOT
- Samtools 1.3.1
- Python 2.7
- Cromwell version support 
  - Successfully tested on v47
  - Does not work on versions < v23 due to output syntax

### Important Notes :
- The provided JSON is a generic ready to use example template for the workflow. It is the user’s responsibility to correctly set the reference and resource variables for their own particular test case using the [GATK Tool and Tutorial Documentations](https://gatk.broadinstitute.org/hc/en-us/categories/360002310591).
- Please visit the [User Guide](https://software.broadinstitute.org/gatk/documentation/) site for further documentation on our workflows and tools.

### Contact Us :
- The following material is provided by the Data Science Platforum group at the Broad Institute. Please direct any questions or concerns to one of our forum sites : [GATK](https://gatk.broadinstitute.org/hc/en-us/community/topics) or [Terra](https://support.terra.bio/hc/en-us/community/topics/360000500432).

### LICENSING :
Copyright Broad Institute, 2019 | BSD-3
This script is released under the WDL open source code license (BSD-3) (full license text at https://github.com/openwdl/wdl/blob/master/LICENSE). Note however that the programs it calls may be subject to different licenses. Users are responsible for checking that they are authorized to run all programs before running this script.
- [GATK](https://software.broadinstitute.org/gatk/download/licensing.php)
- [BWA](http://bio-bwa.sourceforge.net/bwa.shtml#13)
- [Picard](https://broadinstitute.github.io/picard/)
- [Samtools](http://www.htslib.org/terms/)
