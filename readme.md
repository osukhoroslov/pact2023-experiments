# Scheduling of Workflows with Task Resource Requirements in Cluster Environments

This repository contains files and instructions to reproduce experiments from [Scheduling of Workflows with Task Resource Requirements in Cluster Environments](https://link.springer.com/chapter/10.1007/978-3-031-41673-6_14).

## Structure

- `dags` - contains files with workflow instances
- `example` - contains files to reproduce example from Section 3.3
- `systems` - contains files with system configurations
- `exp-*.yaml` - experiment configuration files
  - `exp-simple.yaml` - simple scenario
  - `expo-regular-*.yaml` - regular scenario
  - `expo-irregular-*.yaml` - irregular scenario

## Installing simulator

For simulation purposes we use [DSLab DAG](https://github.com/osukhoroslov/dslab/tree/main/crates/dslab-dag), a library for studying the DAG scheduling algorithms. Clone the main branch of DSLab repository and use the specified revision:

```
$ git clone https://github.com/osukhoroslov/dslab.git
$ cd dslab
$ git checkout dbcafe123ebec71c7641a804239a77e1671b3045
```

To run DSLab DAG you will also need to [install Rust](https://www.rust-lang.org/tools/install). 

## Running experiments

Experiments are run with `dag-run-experiment` tool by passing it the path to experiment configuration file:

```
$ cd dslab/tools/dag-run-experiment
$ cargo run --release -- --config PATH_TO_EXPERIMENT_CONFIG
```

You can set the number of used threads with `--treads` option. By default, it uses all available CPU cores. 

After the experiment execution is completed, its results are saved to file named `$experiment-results.json` in the same directory as experiment file. You can specify other path to save results with `--output` option.

## Results format

The results of each experiment are saved in JSON file ([example](example/example-results.json)). The file contains JSON array which elements correspond to individual simulation runs. Each run contains the following fields:

- `dag` - workflow instance
- `system` - system configuration
- `scheduler` - scheduling algorithm
- `completed` - whether the workflow execution completed successfully
- `makespan` - achieved makespan (workflow execution time)
- `exec_time` - simulation execution time (including algorithm execution time)
- `run_stats` - various collected metrics (see [documentation](https://osukhoroslov.github.io/dslab/docs/dslab_dag/run_stats/struct.RunStats.html))
