Amplicon Sequencing Analysis Pipeline, TB edition (TB-ASAP)
===========================================================

The Amplicon Sequencing Analysis Pipeline (ASAP) is a highly
customizable, automated way to examine amplicon sequencing data. The
important details of the amplicon targets are described in a text-based
input file written in JavaScript Object Notation (JSON). This data
includes the target name, amplicon sequence (or sequences in the case of
gene variant assays), any known SNPs or regions of interest (ROIs)
within the target, and what the presence of this target or SNP
signifies. The sequenced reads are processed by performing adapter, and
optionally, quality trimming using Trimmomatic, and then aligned to the
reference amplicon sequences extracted from the JSON file using one of
several alignment packages (BWA-MEM, bowtie2, and NovoAlign are
currently supported). The resulting BAM files are analyzed with a custom
Python script using the pysam and scikit-bio libraries to aid in
analysis. This script combines the alignment data in the BAM file with
the assay data in the JSON file and interprets the results. The output
is an XML file with complete details for each assay against each sample.
These details include number of reads aligning to each target, any SNPs
found above a user-defined threshold, and the nucleotide distribution at
each of these SNP positions. For ROI assays, the output includes the
sequence distribution at each of the regions of interest -- both the DNA
sequences and translated into amino acid sequences. Also, each assay
target is assigned a significance if it meets the requirements laid out
in the JSON file (i.e. a particular SNP or amino acid change is present)
To make this output easier for the user to interpret, an XSLT stylesheet
is provided for transforming the XML output into a more readable set of
web pages. Additionally, the use of XSLT stylesheets allows for multiple
different views of the same data, from clinical summaries showing only
the most important or relevant results to full researcher summaries
containing all of the data.

Dependencies
~~~~~~~~~~~~

-  python 3.x
-  `pysam <http://pysam.readthedocs.org/en/latest/>`__
-  `scikit-bio <http://scikit-bio.org>`__
-  `lxml <http://lxml.de>`__
-  samtools
-  Trimmomatic (optional, but recommended)
-  An aligner: either bowtie2 (default), Novoalign, or BWA-MEM
-  PBS/Torque Cluster job manager

Installation
~~~~~~~~~~~~

1. Clone this git repository:
   ``git clone git@github.com:TGenNorth/TB-ASAP.git``
2. Use python3's easy-install script as root:
   ``easy-install-3.x TB-ASAP``
3. *OR* install to your user space: ``easy-install-3.x --user TB-ASAP``

\*note: replace the 'x' with the appropriate version for your python3
installation

Running TB-ASAP
~~~~~~~~~~~~~~~

-  ``analyzeAmplicons -n <RUN_NAME> -j <INSTALL_DIR>/assay_data/Mtb6.json -r <DIRECTORY_OF_READ_FILES> -o <OUTPUT_DIR> --breadth 0``

This will run TB-ASAP with the default paramaters, but turning off coverage breadth checking, since the reference sequences include flanking regions, submitting all of the jobs using PBS/torque and exiting. The final output file will be ``<OUTPUT_DIR>/<RUN_NAME>_analysis.xml``. To see all of the additional options available, run ``analyzeAmplicons --help``.

-  Change into the ``<OUTPUT_DIR>`` and run:
   ``formatOutput -s <INSTALL_DIR>/output_transforms/TB_RunResults_Mtb6.xsl -x <RUN_NAME>_analysis.xml``

This will create HTML files: ``<RUN_NAME>.html`` and
``<RUN_NAME>_details.html`` and a directory ``<RUN_NAME>/`` of HTML
files for each sample. Open ``<RUN_NAME>.html`` in your web browser to
view the results.

License
~~~~~~~

Copyright :copyright: The Translational Genomics Research Institute. See
the included "LICENSE" document.

Contact
~~~~~~~

TGen North \| 3051 W Shamrell Blvd Ste 106 \| Flagstaff, AZ 86001-9435

Darrin Lemmer dlemmer@tgen.org
