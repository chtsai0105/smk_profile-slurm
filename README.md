## Snakemake profile for batch partition@UCR hpcc

This profile is adapted from this [smk-simple-slurm](https://github.com/jdblischak/smk-simple-slurm.git) and have been tuned to use batch partition@UCR hpcc, 
so please make sure properly modify it as your need.

If you need more comprehensive feature, please refer to the [official Slurm profile for Snakemake](https://github.com/Snakemake-Profiles/slurm.git).

<br>

## Install the profile
The eaisest way to use this profile is simply clone this repo to the root folder of your snakemake project and named it as `slurm`.

If you plan to use it in several different snakemake projects. You can clone this repo to your snakemake config directory. (By default is $HOME/.config/snakemake)

<br>

## Modify the profile

You can setup some parameters from `config.yaml`. There are 3 sections in this file.
The first section is `cluster`, which basically defines the sbatch parameters.
```
cluster:
  mkdir -p logs/{rule} &&
  sbatch
    --partition={resources.partition}
    --cpus-per-task={threads}
    --time={resources.time}
    --mem={resources.mem_mb}
    --job-name=smk-{rule}
    --output=logs/{rule}/{rule}-%j.out
```

The second section is `default-resources`, which defines the default parameters that fill in the previous section. Note that it is possible to overwrite all these 
parameters in the *resources* section of each rule.

```
default-resources:
  - partition=batch
  - time="1-00:00:00"
  - mem_mb=max(input.size_mb * 5 * attempt, 2000)
```

The third section contains additional argument that are going to pass into the snakemake command. Please adjust them as your own need.
```
restart-times: 3
max-jobs-per-second: 1
max-status-checks-per-second: 10
local-cores: 1
latency-wait: 60
jobs: 8
max-threads: 12
keep-going: True
rerun-incomplete: True
printshellcmds: True
scheduler: greedy
use-conda: True
```

<br>

## Using the profile in snakemake

When specifying the profile, Snakemake will search the profile in per-project, user and global configuration directories until the name is matched.
E.g. If your project located in `$HOME/path/to/project` and the profile named `slurm`. You can have this profile saved in either `$HOME/path/to/project`, 
`$HOME/.config/snakemake` or `/etc/xdg/snakemake`.

Later you can specify to use the profile by:
```
snakemake --profile slurm [additional options...]

```
