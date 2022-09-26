## Snakemake profile for stajichlab partition@UCR hpcc

This profile is adapted from this [Cookiecutter template](https://github.com/Snakemake-Profiles/slurm). In order to submit the snakemake jobs to slurm, you have to create a profile first. This profile have been tuned to use stajichlab partition@UCR hpcc, so please make sure properly modify it as your need.

## Install the profile
The eaisest way to use this profile is simply clone this repo to the same folder of your snakefile.

If you plan to use it in several different snakemake projects. You can clone this repo to your snakemake config directory. (By default is $HOME/.config/snakemake)

## Modified the profile

There are 2 config files can be modified.

First, you can setup the cluster info from `settings.json`:

```
{
    "SBATCH_DEFAULTS": "partition=stajichlab output=slurm_logs/%x_%j.out",     # Change to the partition you want to use
    "CLUSTER_NAME": "",
    "CLUSTER_CONFIG": "",
    "ADVANCED_ARGUMENT_CONVERSION": "no"
}
```

Second, you can setup some default parameters from `config.yaml`:

```
restart-times: 3
jobscript: "slurm-jobscript.sh"
cluster: "slurm-submit.py"
cluster-status: "slurm-status.py"
max-jobs-per-second: 1
max-status-checks-per-second: 10
local-cores: 1
latency-wait: 60
default-resources: ["time=\"1-00:00:00\"", "mem_mb=max(input.size_mb * 5 * attempt, 2000)"]   # You can setup the default resources for all the rules. This value would be overwritten by the `resources` settings defined in each rule.
```

## Using the profile in snakemake

When specifying the profile, Snakemake will search this profile in the snakemake config folder (`$HOME/.config/snakemake`)
E.g. If you have this repo cloned to your snakemake config folder. (Which the full path is `$HOME/.config/snakemake/snakemake_profile-slurm`) You can specify to use it by:

```
snakemake --profile snakemake_profile-slurm [Options...]

```

Alternatively, snakemake also accept relative or absolute path for specifying the profile. E.g. You have this profile saved with your snakefile at `$HOME/path/to/project/snakemake_profile-slurm`, and you have already cd into this directory. You can either specify it with relative path:
```
snakemake --profile ./snakemake_profile-slurm [Options...]

```
Or absolute path:
```
snakemake --profile $HOME/path/to/project/snakemake_profile-slurm [Options...]

```
