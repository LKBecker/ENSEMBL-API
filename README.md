# ENSEMBL-API
Quick Python script for rate-limited access to ENSEMBLs databases, supporting a multitude of REST calls (See also: rest.ensembl.org).
Based on the original script [here](https://github.com/Ensembl/ensembl-rest/wiki/Example-Python-Client)

Tested on Python 3.2.3

All functions are executed on the contents of text files in the same directory as ENSEMBL_API.py. 
General input format is one item per line with tab-separated values. Output follows the same convention and should be easy to import into Excel or R.

### Available functions:
#### `batch_SNPs(species)`
For each SNP in `input SNP.txt` (one rsID per line), retrieves: Allele string, ancestral and minor allele, reference assembly version, strand, chromosome, position and any registered synonyms.


#### `batch_SNP_Consequences(species)`
For each SNP in `input SNP.txt` (one rsID per line), retrieves: Associated gene ID, associated transcript ID, associated gene name, associated gene type, impact rating, variant allele and consequence terms, based on ENSEMBL's VEP.


#### `batch_Feature_Overlap(species, [feature(s)])`
##### Parameters: 
  `features`: any of "gene", "transcript"
##### Settings: 
 `MAX_SLICE_FOR_GENES`: Controls the maximum search range to either side of the start/stop position(s).
 
This function searches for overlapping features for each position in `input feature search.txt`. One position per line, each line containing tab-separated values for: base feature ID (any), chromosome number (integer), start position (integer) and optional stop position (may be omitted, e.g. for SNPs, and will be inferred). 
Returns: Neighbor item name, neighbor ENSEMBL ID, item type, biological type, canonical/long form name, data source, neighbor chromosome, neighbor start and stop positions, assembly version and parent gene (if neighbor item is transcript)


#### `batch_remap(species)`
Remaps between assembly versions for each item in `input remap.txt`. Each item requires the following tab-separated values: Input assembly (GrCHxx), chromosome (integer), start (integer), stop (integer) and target assembly (String).


#### `batch_Characterise()`
For each item in `input characterise.txt` (one ENSEMBL ID per line), retrieves any available data. Generally this includes name, biotype, linked IDs, et cetera. Used for exploring SNPs or turning ENSEMBL IDs into human-readable names. 
**Output format will not contain column headings for all data types and is not ready to import into R**


#### `batch_human_homolog()`
For each item in `input human homolog.txt`, retrieves the human *paralogue* (see ENSEMBL's explanation of their protein tree methodology [here](http://www.ensembl.org/info/genome/compara/homology_method.html)). One ENSEMBL ID per line input. 

#### `batch_get_sequence(sequence_type)`
##### Parameters: 
  `sequence_type`: one of 'gene', 'protein'
For each item in `input get sequence.txt` (one ENSEMBL ID per line), retrieves sequence of the requested item and type. Some sequences may not have the requested type available, in which case an error will be reported.


#### `batch_SNP_r2_retrieval(species, population)`
##### Parameters:
  `species`: The species in which to search for r2 values. Default: human
  
  `population`: The population from which to retrieve r2 values. Default: 1000GENOMES:phase_3:GBR (for a full list of available populations, see [ENSEMBL's REST documentation](https://rest.ensembl.org/documentation/info/variation_populations)


Constructs an r2 value matrix for two lines of SNPs from `input rsquared retrieval.txt`. That is, line 1 creates columns and line 2 creates rows of a matrix, with the values being r2 between these SNPs.

**Experimental function, still slow and partially redundant in case of rows == columns**
