# Customizing Snakemake Wrappers
(WIP)

Thanks to [Tessa Pierce](https://bluegenes.github.io/On-Snakemake-and-Wrappers-for-workflows/) and [Taylor Reiter](https://taylorreiter.github.io/) from the [DIB lab](http://ivory.idyll.org/lab/) for helping with this!


In my latest variant discovery work, I found myself grappling with [Snakemake wrappers](https://snakemake-wrappers.readthedocs.io/en/stable/). I needed to run many `vcf` files through GATK's [GenomicsDBImport]( https://gatk.broadinstitute.org/hc/en-us/articles/360051305591-GenomicsDBImport command) (v4.1.9.0)

Here is a usage example for GenomicsDBImport provided on the official webpage

```
gatk --java-options "-Xmx4g -Xms4g" GenomicsDBImport \
  -V data/gvcfs/mother.g.vcf.gz \
  -V data/gvcfs/father.g.vcf.gz \
  -V data/gvcfs/son.g.vcf.gz \
  --genomicsdb-workspace-path my_database \
  --tmp-dir=/path/to/large/tmp \
  -L 20
```
As you can see, I need a `-V` flag in front of each `vcf.gz` file being processed. To pass all my vcf files through this command, I would have to write a little bash script. That's not great for me.

Thankfully, there's a neat little [Snakemake wrapper](https://github.com/snakemake/snakemake-wrappers/tree/master/bio/gatk/genomicsdbimport) for this:

```
__author__ = "Filipe G. Vieira"
__copyright__ = "Copyright 2021, Filipe G. Vieira"
__license__ = "MIT"

rule genomics_db_import:
    input:
        gvcfs=["calls/a.g.vcf.gz", "calls/b.g.vcf.gz"],
    output:
        db=directory("db"),
    log:
        "logs/gatk/genomicsdbimport.log"
    params:
        intervals="ref",
        db_action="create", # optional
        extra="",  # optional
        java_opts="",  # optional
    resources:
        mem_mb=1024
    wrapper:
        "0.77.0/bio/gatk/genomicsdbimport"
```

My problem is that this wrapper calls GATK version 4.2.0.0. And I need version 4.1.9.0.

Turns out that there's no good documentation or resource for wrapper version shifts. The easiest way to get the version of a software you want, is to customize the wrapper script.

This is a mini tutorial on how to customize the version of `GenomicsDBImport` in a pre-written wrapper.

## Step 1: Download and save the wrapper

I copied the code into a text editor and saved it with a `.py` extension. The script for the `GenimicsDBImport` lives [here](https://github.com/snakemake/snakemake-wrappers/blob/master/bio/gatk/genomicsdbimport/wrapper.py)

## Step 2: Download and edit the `environment.yml`
I copied the `environment.yml` from the [GitHub repo](https://github.com/snakemake/snakemake-wrappers/blob/master/bio/gatk/genomicsdbimport/environment.yaml), pasted it into a text file and saved it to `environment.yml` extension. Then changed the version of GATK like shown here:

```
channels:
  - bioconda
  - conda-forge
  - defaults
dependencies:
  - gatk4 ==4.1.9.0
  - snakemake-wrapper-utils ==0.1.3
```

## Step 3: Replace Wrapper

In my Snakefile, I replaced the `wrapper: /path/to/wrapper` directive with:

```
script: /path/name-of-wrapper.py
conda: /path/this-version.environment.yml
```

And that should work!
