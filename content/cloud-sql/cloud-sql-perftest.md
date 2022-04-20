---
title: With Load Test
weight: 20
---
# Cloud SQL with Perfomance Test

## Description

[Cloud SQL](https://cloud.google.com/sql) with Load Test example

List of the components used in this sandbox:

* [`VPC Network`](https://github.com/agilestacks/google-components/tree/main/network)
* [`Cloud SQL`](https://github.com/agilestacks/google-components/tree/main/cloud-sql)
* [`K6 Example SQL Performance Test`](https://github.com/agilestacks/google-components/tree/main/k6-sql-load-test)

## Setup

First you need initialize the sandbox configuration.
To do this please run the initialization command:

```bash
hub stack init
```

The command will prompt you to enter the `GCP project ID` and
will create initial configuration files and download required components.

By default command enable auto configure of sandbox during `deploy` or `undeploy`.

### Advance case

For some advanced cases you need to configure sandbox manually.
To do this just pass `--disable-auto-configure` flag:

```bash
hub stack init --disable-auto-configure
```

And after that to configure sandbox manually run command:

```bash
hub stack configure
```

The [Hub CLI] produces a state file after provisioning of sandbox,
same as its components, and it must be uploaded to the cloud.
For simplicity, we pre-create `Cloud DNS Zone` and `GCS bucket`
in your GCP project with pre-generated names.
This is done during `configure` command.

## Deploy/Undeploy

Once you are done with the configuration, use the following command to deploy the sandbox:

```bash
hub stack deploy
```

To re-run the load test:

```bash
hub stack deploy -c k6-sql-load-test
```

Feel free to adjust parameter values in `hub.yaml` such as`component.k6SQLloadTest.testDuration`
or `component.k6SQLloadTest.numberOfUsers` to change test behaviour.

If any of the sandbox components would require additional configuration parameters,
users will be asked to provide them.

To delete the sandbox, run the followng command:

```bash
hub stack undeploy
```

### Parameters

Each `component` has a set of parameters (key-value pairs) such as
GKE cluster name or number of nodes in a node pool, etc.

Please explore `parameters` section of the `hub.yaml` to see what parameters are available.
Any default value can be overridden.
To get more information on how components are implemented refer to their README files.

## Recover

Google Cloud Shell environments are ephemeral, which means any files stored locally
on the Cloud Shell machine will be lost when the machine restarts.
Luckily, we store sandbox state files in the Cloud, which means,
if we lose a local state, we can always recover it from the Cloud.

In order to recover your sandbox first you need to find ID of sandbox
in the list of sandboxes in your GCP project.

To do this run next command:

```bash
hub state ls
```

Next step will be initializations of your target sandbox:

```bash
hub stack init [id]
```

[Hub CLI] will find location of state file, download it and prepare configuration files.

## Architecture Diagram

![Cloud SQL Sandbox Architecture](/images/cloud_sql_diagram.png)

[Hub CLI]: https://superhub.io