## Snakemake profile for stajichlab partition@UCR hpcc

This profile is adapted from this [Cookiecutter template](https://github.com/Snakemake-Profiles/slurm). In order to submit the snakemake jobs to slurm, you have to create a profile first. This profile have been tuned to use stajichlab partition@UCR hpcc, so please make sure properly modify it as your need.

## Install the profile
You can clone this repo to your snakemake config directory. (By default is $HOME/.config/snakemake)

If you're cloning it as a submodule from my repo [metagenome-snakemake](https://github.com/chtsai0105/metagenome-snakemake). You can simply symlink the **slurm** folder to your snakemake config folder:

```
ln -s `realpath slurm` $HOME/.config/snakemake
```

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
restart-times: 1
jobscript: "slurm-jobscript.sh"
cluster: "slurm-submit.py"
cluster-status: "slurm-status.py"
max-jobs-per-second: 1
max-status-checks-per-second: 10
local-cores: 1
latency-wait: 60
default-resources: ["mem_mb=20000", "time=10080"]   # You can setup the default resources for all the rules. This value would be overwritten by the `resources` settings defined in each rule.
```

## Using the profile in snakemake

You can specify this profile during running the snakemake:

```
snakemake --profile slurm [Options...]

```