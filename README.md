# Couchbase Server Ansible Role

[![License](https://img.shields.io/github/license/couchbaselabs/ansible-couchbase-exporter.svg)](https://www.apache.org/licenses/LICENSE-2.0)


## Description

Deploy [cbhealthagent](https://docs.couchbase.com/cmos/current/index.html) Couchbase health checking agent - A basic integrated health checking agent for Couchbase Server.

## Requirements

-   Ansible >= 2.10 (It might work on previous versions, but we cannot guarantee it)
-   jmespath on deployer machine. If you are using Ansible from a Python virtualenv, install _jmespath_ to the same virtualenv via pip.


## Role Variables

All variables which can be overridden are stored in [defaults/main.yml](defaults/main.yml) file as well as in table below.  The only values necessary to change would be the `couchbase_username` and `couchbase_password`.

| **Name**           | **Default Value** | **Description**                    |
| :-------------- | :------------- | :-----------------------------------|
| cbhealthagent_version | `0.3.0` |  |
| cbhealthagent_user | `couchbase` |  |
| cbhealthagent_user_group | `couchbase` |  |
| cbhealthagent_install_dir | `/opt/cbhealthagent/bin` |  |
| cbhealthagent_binary | `cbhealthagent` |  |
| cbhealthagent_conf_dir | `/etc/cbhealthagent` |  |
| cbhealthagent_env_file | `service.env` |  |
| cbhealthagent_local_tmp_dir |  `/tmp/cbhealthagent` | path to where the binary will be downloaded to on the controller |
| cbhealthagent_local_binary_path | `""` | Full path to the local binary if already downloaded on the controller.  This should only be set, if ansible is not downloading the binary and you have
| cbhealthagent_log_level | `debug` | Level to log at.  Can be: debug, info, warn, error |
| cbhealthagent_couchbase_install_path | `/opt/couchbase` |  |
| cbhealthagent_check_interval | `10m` | How often to refresh health check data. |
| cbhealthagent_log_analyzer_janitor_interval | `10m` | How often to clean up stale log alerts. |
| cbhealthagent_log_analyzer_alert_duration | `1h` | How long will log alerts fire before they are cleaned up, if no matching message is seen in the meantime. |
| cbhealthagent_features_enable | `"health-agent,log-analyzer"` | Features to enable. Overrides `features.auto` |
| cbhealthagent_features_disable | `"fluent-bit,prometheus-exporter"` | Features to disable. Overrides `features.auto` |
| couchbase_username | `Administrator` | The Couchbase user to use. |
| couchbase_password | `password` | The Couchbase password to use. |
| cbhealthagent_http_port | `9082` | Port that exposes the agent REST API |

### Features

Currently the following features are available:

| **Name**      | **Description**                    |
| :------------ | :-----------------------------------|
| health-agent | Performs health checks against the local node |
| log-analyzer | Parses log messages sent from FluentBit |
| fluent-bit | The rpm for `cbhealthagent` will install fluent-bit and the necessary configuration files.  However, it is instead preferred to install FluentBit separately using the [`couchbaselabs.couchbase_fluent_bit`](https://github.com/couchbaselabs/ansible-couchbase-fluent-bit) role which has additional configuration for slow query logging as well as support for sending log messages to `cbhealthagent` |
| prometheus-exporter | `cbhealthagent` can optionally start a prometheus exporters.  However, it is preferred to install the exporter separately using the [`couchbaselabs.couchbase_exporter`](https://github.com/couchbaselabs/ansible-couchbase-fluent-bit) role, which has additional configuration options.

## Installation

You can install this role with the `ansible-galaxy` command, and can run it
directly from the git repository.

You should install it like this:

```
ansible-galaxy install git+https://github.com/couchbaselabs/ansible-couchbase-cbhealthagent.git
```

## Example

### Playbook

Use it in a playbook as follows:

```yaml
- hosts: all
  roles:
    - couchbaselabs.cbhealthagent
```

## Useful Commands

### Manage the Service

It may be necessary from time to time to start, stop, restart, or get the status of the `cbhealthagent` service.

```bash
sudo systemctl start cbhealthagent
sudo systemctl status cbhealthagent
sudo systemctl restart cbhealthagent
```

### Viewing Standard Out

Since the exporter writes to stdout for logging, those messages can be viewed through `journalctl`

##### Most Recent Messages

```bash
sudo journalctl -r -t cbhealthagent
```

##### Follow Messages

```bash
sudo journalctl -f -t cbhealthagent
```

## License

This project is provided as is with no support and is licensed under Apache 2.0. See [LICENSE](/LICENSE) for more details.
