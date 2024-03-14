# PerfConf 2024 Arcaflow Demo

## Setup

### Installation

Download the binary for arcaflow that corresponds to your system from the [development release page](https://github.com/arcalot/arcaflow-engine/releases/tag/v0.12.1-dev1).
Move it to this folder, and rename it to `arcaflow`

### Configuration

The config has podman set as the deployer. If you're using Docker, change the line `deployer_name: podman` to `deployer_name: docker`

## Demo 1: Standalone Plugin Execution

Requirements: Docker or Podman

```
$ echo '{"threads": 2, "time": 5}' | podman run -i --rm quay.io/arcalot/arcaflow-plugin-sysbench:latest -s sysbenchcpu -f -
```

Explanation: The JSON input is being piped into the `sysbenchcpu` `step` within the `arcaflow-plugin-sysbench` plugin.

## Demo 3: Basic workflow

Run:
```
$ ./arcaflow -config=./config.yaml -input=./cpu-input.yaml -context=. -workflow=sysbench-cpu-workflow.yaml
```

## Demo 3: Parent workflow

Run:
```
$ ./arcaflow -config=./config.yaml -input=./parent-input.yaml -context=. -workflow=parent-workflow.yaml 
```


## Demo 4: Modifying Expressions

Modify the UUID line in the outputs section of `parent-workflow.yaml`.
Change the expression from:
```
uuid: !expr $.steps.uuidgen.outputs.success.uuid
```
to
```
uuid: !expr '"appended string " + $.steps.uuidgen.outputs.success.uuid'
```

Then Run:
```
$ ./arcaflow -config=./config.yaml -input=./parent-input.yaml -context=. -workflow=parent-workflow.yaml 
```

