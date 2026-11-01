# An easy-to-use version of seqff

> Seqff is tool for estimating fetal DNA fraction from https://obgyn.onlinelibrary.wiley.com/doi/full/10.1002/pd.4615

## How to use it?

+ You need to prepare:
	+ `.bam` file of a sample aligned to hg19 as input for estimating its fetal DNA fraction.
	+ All files listed in this repo., and put in the same directory.
	+ An R software with `Rsamtools`, `argparse`, `dplyr`, `stringr` installed.

+ And then, just run the following code in the terminal:

```shell
Rscript seqff.r -f input.bam -o output.txt
```
Where `input.bam` is the file name of your hg19-aligned `.bam` file, and `output.txt` is the file which will store the result of seqff.

If your `.bam` file was aligned to hg38, re-align the original sequencing reads to hg19 first. Then use the resulting hg19-aligned BAM as `input.bam` in the command above, without `--hg38`. Omitting `--hg38` does not convert an hg38-aligned BAM to hg19.

The `--hg38` mode is no longer recommended. [Issue #3](https://github.com/Yilab-AIPT/easy-to-use-seqff/issues/3) reports substantially different estimates between hg38- and hg19-aligned data and more reasonable results after re-alignment to hg19.

## What have been revised?

+ You can input `.bam` files straightly after alignment instead of `.sam` files without header.
+ Sometimes, output of `enet` would be `NA` due to `NA` values in variable `bincounts`. We set them to zeros before calculating `enet` value.
+ Less arguments needed.
+ In some conditions, the chromosome names in `.bam` file do not contain string 'chr'. This script automatically detects them and adds the 'chr' strings (see [Issue #1](https://github.com/Wubeizhongxinghua/easy-to-use-seqff/issues/1)).
+ An experimental [hg38 mode](https://github.com/Wubeizhongxinghua/easy-to-use-seqff/issues/2) was implemented using `--hg38` and [liftOver](https://genome.ucsc.edu/cgi-bin/hgLiftOver)-converted bins. It remains available in the script, but is no longer recommended (see above).
> Disclaimer: When using `liftOver` for conversion, 8.29% of bins cannot correspond to hg38. Therefore, the author cannot guarantee that the hg38 mode necessarily reflects the ideal performance of the model.
