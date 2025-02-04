---
title: Run a runbook
description: Using the Octopus CLI to run a runbook.
position: 287
---

The [Octopus CLI](/docs/octopus-rest-api/octopus-cli/index.md) can be used to run runbooks that have already been created.

```text
Runs a Runbook.

Usage: octo run-runbook [<options>]

Where [<options>] is any of:

Run Runbook:

      --project=VALUE        Name or ID of the project that contains the
                             Runbook. This is required.
      --runbook=VALUE        Name or ID of the runbook. If the name is
                             supplied, the project parameter must also be
                             specified.
      --environment=VALUE    Name or ID of the environment to run in, e.g .,
                             'Production' or 'Environments-1'; specify this
                             argument multiple times to run in multiple
                             environments.
      --snapshot=VALUE       [Optional] Name or ID of the snapshot to run. If
                             not supplied, the command will attempt to use
                             the published snapshot.
      --forcePackageDownload [Optional] Whether to force downloading of
                             already installed packages (flag, default false).
      --guidedFailure=VALUE  [Optional] Whether to use Guided Failure mode.
                             (True or False. If not specified, will use
                             default setting from environment)
      --specificMachines=VALUE
                             [Optional] A comma-separated list of machine
                             names to target in the specified environment/s.
                             If not specified all machines in the environment
                             will be considered.
      --excludeMachines=VALUE
                             [Optional] A comma-separated list of machine
                             names to exclude in the specified environment/s.
                             If not specified all machines in the environment
                             will be considered.
      --tenant=VALUE         [Optional] Run a runbook on the tenant with this
                             name or ID; specify this argument multiple times
                             to add multiple tenants or use `*` wildcard to
                             deploy to all tenants who are ready for this
                             release (according to lifecycle).
      --tenantTag=VALUE      [Optional] Run a runbook on the tenants matching
                             this tag; specify this argument multiple times
                             to build a query/filter with multiple tags, just
                             like you can in the user interface.
      --skip=VALUE           [Optional] Skip a step by name
      --runAt=VALUE          [Optional] Time at which runbook run should
                             start (scheduled run), specified as any valid
                             DateTimeOffset format, and assuming the time
                             zone is the current local time zone.
      --noRunAfter=VALUE     [Optional] Time at which scheduled runbook run
                             should expire, specified as any valid
                             DateTimeOffset format, and assuming the time
                             zone is the current local time zone.
  -v, --variable=VALUE       [Optional] Specifies the value for a prompted
                             variable in the format Name:Value. For JSON
                             values, embedded quotation marks should be
                             escaped with a backslash.
      --waitForRun           [Optional] Whether to wait synchronously for
                             deployment to finish.
      --progress             [Optional] Show progress of the runbook run
      --runTimeout=VALUE     [Optional] Specifies maximum time (timespan
                             format) that the console session will wait for
                             the runbook run to finish (default 00:10:00).
                             This will not stop the run. Requires --
                             waitForRun parameter to be set.
      --cancelOnTimeout      [Optional] Whether to cancel the runbook run if
                             the run timeout is reached (flag, default false).
      --runCheckSleepCycle=VALUE
                             [Optional] Specifies how much time (timespan
                             format) should elapse between runbook run status
                             checks (default 00:00:10)
      --noRawLog             [Optional] Don't print the raw log of failed
                             tasks
      --rawLogFile=VALUE     [Optional] Redirect the raw log of failed tasks
                             to a file

Common options:

      --help                 [Optional] Print help for a command.
      --helpOutputFormat=VALUE
                             [Optional] Output format for help, valid options
                             are Default or Json
      --outputFormat=VALUE   [Optional] Output format, valid options are
                             Default or Json
      --server=VALUE         [Optional] The base URL for your Octopus Server,
                             e.g., 'https://octopus.example.com/'. This URL
                             can also be set in the OCTOPUS_CLI_SERVER
                             environment variable.
      --apiKey=VALUE         [Optional] Your API key. Get this from the user
                             profile page. You must provide an apiKey or
                             username and password. If the guest account is
                             enabled, a key of API-GUEST can be used. This
                             key can also be set in the OCTOPUS_CLI_API_KEY
                             environment variable.
      --user=VALUE           [Optional] Username to use when authenticating
                             with the server. You must provide an apiKey or
                             username and password. This Username can also be
                             set in the OCTOPUS_CLI_USERNAME environment
                             variable.
      --pass=VALUE           [Optional] Password to use when authenticating
                             with the server. This Password can also be set
                             in the OCTOPUS_CLI_PASSWORD environment variable.
      --configFile=VALUE     [Optional] Text file of default values, with one
                             'key = value' per line.
      --debug                [Optional] Enable debug logging.
      --ignoreSslErrors      [Optional] Set this flag if your Octopus Server
                             uses HTTPS but the certificate is not trusted on
                             this machine. Any certificate errors will be
                             ignored. WARNING: this option may create a
                             security vulnerability.
      --enableServiceMessages
                             [Optional] Enable TeamCity or Team Foundation
                             Build service messages when logging.
      --timeout=VALUE        [Optional] Timeout in seconds for network
                             operations. Default is 600.
      --proxy=VALUE          [Optional] The URL of the proxy to use, e.g.,
                             'https://proxy.example.com'.
      --proxyUser=VALUE      [Optional] The username for the proxy.
      --proxyPass=VALUE      [Optional] The password for the proxy. If both
                             the username and password are omitted and
                             proxyAddress is specified, the default
                             credentials are used.
      --space=VALUE          [Optional] The name or ID of a space within
                             which this command will be executed. The default
                             space will be used if it is omitted.
      --logLevel=VALUE       [Optional] The log level. Valid options are
                             verbose, debug, information, warning, error and
                             fatal. Defaults to 'debug'.
```

## Basic Examples {#RunningRunbooks-Basicexamples}

This runs a runbook using the published snapshot:

```bash
octo run-runbook --runbook="Hello World"  \
                 --project="Smurfs"       \
                 --environment="Test"     \
                 --server http://octopus/ \
                 --apiKey API-ABCDEF123456
```

This runs a runbook using a specific snapshot (e.g., Snapshot KGHSL3L):

```bash
octo run-runbook --runbook="Hello World"       \
                 --project="Smurfs"            \
                 --environment="Test"          \
                 --snapshot="Snapshot KGHSL3L" \
                 --server http://octopus/      \
                 --apiKey API-ABCDEF123456
```

To specify multiple environments, you can use the following:

```bash
octo run-runbook --runbook="Hello World"  \
                 --project="Smurfs"       \
                 --environment="Test"     \
                 --environment="Dev"      \
                 --server http://octopus/ \
                 --apiKey API-ABCDEF123456
```

:::success
You can run a runbook on ALL tenants in an environment by using the `--tenant=*` argument.
:::

## Learn more

- [Octopus CLI](/docs/octopus-rest-api/octopus-cli/index.md)
- [Runbooks](/docs/runbooks/index.md)
- [Octopus REST API](/docs/octopus-rest-api/index.md)
- [Creating API keys](/docs/octopus-rest-api/how-to-create-an-api-key.md)
