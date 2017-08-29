---
layout: layout.pug
title: dcos node log
menuWeight: 5
excerpt: ""
featureMaturity: ""
enterprise: 'no'
navigationTitle:  dcos node log
---

<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

    
# Description
Print the Mesos logs for the leading master node, agent nodes, or both.

# Usage

```bash
dcos node log [OPTION]
```

# Options

| Name, shorthand | Default | Description |
navigationTitle:  dcos node log
|---------|-------------|-------------|
| `--leader`   |             |  The leading master. |
| `--follow`   |             |  Dynamically update the log. |
| `--lines=N`   |     10      |  Print the last N lines. |
| `--master`   |             |  This option is deprecated and is replaced by `--leader`. |
| `--mesos-id=<mesos-id>`   |             | The agent ID of a node. |
| `--slave=<agent-id>`   |             | This option is deprecated and is replaced by `--mesos-id`. |

# Parent command

| Command | Description |
navigationTitle:  dcos node log
|---------|-------------|
| [dcos node](/docs/1.10/cli/command-reference/dcos-node/) | View DC/OS node information. | 

<!-- # Examples -->
