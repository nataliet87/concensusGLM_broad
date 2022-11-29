# concensusGLM -- working to get functional with dplyr > 0.7

Forked from https://github.com/broadinstitute/concensusGLM

Eachan O. Johnson *et. al.*, [New inhibitors of *Mycobacterium tuberculosis* identified using systems chemical biology](https://doi.org/10.1101/396440), *bioRxiv*, Aug 2018, doi: [10.1101/396440](https://doi.org/10.1101/396440)

## Basic usage

The script contained in `exec/concensusGLM.R` allows for a lot of imagined use cases.

If you use the `--sge` option, you need to provide the path on your computer to the `exec/sge-templae.sh` template submission script, which you may need to edit for your particular setup.

```
Usage:
concensusGLM.R --help
concensusGLM.R --data <data-file> --neg <negative-control> [--meta <experiment-metadata> --pos <positive-control> --no-count-threshold --checkpoint --parallel --sge <template-script>]

Options:
-h --help                      Show this message and exit
--data <data-file>             Input count table CSV, one line per lane per strain per well
--meta <experiment-metadata>   CSV of handling records
--neg <negative-control>       Name of negative control compound, e.g. DMSO or none or untreated
--pos <positive-control>       Name of positive control compound, e.g. rifampin or BRD-K01507359-001-19-5 or BRD-K01507359
--checkpoint                   Save checkpoints to allow restart in case of failure
--parallel                     Analyze strains in parallel (needs multiple cores)
--sge <template-script>        Use GridEngine cluster with template to analyze strains in parallel. 
--no-count-threshold           Don\'t discard plates or strains based on low counts
```

## More advanced usage

This package provides the objects:

- `concensusDataSet`, a container for ConcensusGLM analysis
- `concenusWorkflow`, a wrapper to the `workflow` object from [workflows](https://github.com/eachanjohnson/workflows), allowing scale-up to multi-core on a laptop or a GridEngine style cluster

Both objects `concensusDataSet` have methods to carry out ConcensusGLM analysis. Using `?methodName` in the R console will give documentation.

- `getRoughDispersions`
- `getBatchEffects`
- `getFinalDispersions`
- `getFinalModel`
- `write_concensusDataSet`

Application of these methods in an order other than listed above is not fully supported and may not work.

Applying the methods to a `concensusDataSet` will execute the method immediately. Applying it to a `concenusWorkflow` will add it to a list of commands, which can be executed at some other time by calling `execute` on a `concensusWorkflow` object. 

You can also `scatter` a `concenusWorkflow` based on categorical columns in the input data set to allow chunking for embarassingly parallel computation when `execute` is called. This can be done on the local machine using multiple cores, or using a GridEngine style cluster like SGE or UGE, in which case the submission script `exec/sge-template.sh` may need to be edited for your specific cluster.


Dependencies:

- R
- [broom](https://github.com/tidymodels/broom)
- [dplyr](https://github.com/hadley/dplyr)
- [errR](https://github.com/eachanjohnson/errR)
- [future](https://github.com/HenrikBengtsson/future)
- [magrittr](https://github.com/hadley/magrittr)
- MASS
- parallel
- [readr](https://github.com/hadley/readr) for fast loading of large CSV files
- [workflows](https://github.com/eachanjohnson/workflows)

