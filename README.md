# cbhealthagent Ansible Role

<p align="center">

  <a href="https://github.com/couchbaselabs/ansible-couchbase-cbhealthagent/graphs/contributors" alt="Contributors">
    <img src="https://img.shields.io/github/license/couchbaselabs/ansible-couchbase-cbhealthagent" />
  </a>

  <!--
  <a href="https://galaxy.ansible.com/couchbaselabs/cbhealthagent" alt="Ansible Role">
    <img src="https://img.shields.io/ansible/role/{roleId}" />
  </a>

  <a href="https://galaxy.ansible.com/couchbaselabs/cbhealthagent" alt="Ansible Quality Score">
    <img src="https://img.shields.io/ansible/quality/{roleId}" />
  </a>

  <a href="https://galaxy.ansible.com/couchbaselabs/cbhealthagent" alt="Downloads">
    <img src="https://img.shields.io/ansible/role/d/{roleId}" />
  </a>
  -->

  <a href="https://github.com/couchbaselabs/ansible-couchbase-cbhealthagent/releases" alt="GitHub tag (latest by date)">
    <img src="https://img.shields.io/github/v/tag/couchbaselabs/ansible-couchbase-cbhealthagent" />
  </a>

  <a href="https://github.com/couchbaselabs/ansible-couchbase-cbhealthagent/actions" alt="GitHub Workflow Status">
    <img src="https://img.shields.io/github/workflow/status/couchbaselabs/ansible-couchbase-cbhealthagent/Lint" />
  </a>

  <a href="https://github.com/couchbaselabs/ansible-couchbase-cbhealthagent/commits/main" alt="GitHub last commit">
    <img src="https://img.shields.io/github/last-commit/couchbaselabs/ansible-couchbase-cbhealthagent" />
  </a>

  <a href="https://github.com/couchbaselabs/ansible-couchbase-exporter/graphs/contributors" alt="GitHub last commit">
    <img src="https://img.shields.io/github/contributors/couchbaselabs/ansible-couchbase-cbhealthagent" />
  </a>

</p>

## Description

Deploy [cbhealthagent](https://docs.couchbase.com/cmos/current/index.html) Couchbase health checking agent - A basic integrated health checking agent for Couchbase Server.

## Disclaimer

This ansible role and the sub-components configured are not officially supported under Couchbase Enterprise Subscriptions. Please contact Couchbase on any details for your particular environment. Contents here and sub-components are provided as is, it is maintained through community contributions.  Any issues should be reported as an [Issue](https://github.com/couchbaselabs/ansible-couchbase-cbhealthagent/issues) on Github.  Pull Requests are welcomed!

## Requirements

-   Ansible >= 2.10 (It might work on previous versions, but we cannot guarantee it)
-   jmespath on deployer machine. If you are using Ansible from a Python virtualenv, install _jmespath_ to the same virtualenv via pip.


## Role Variables

All variables which can be overridden are stored in [defaults/main.yml](defaults/main.yml) file as well as in table below.  The only values necessary to change would be the `couchbase_username` and `couchbase_password`.

| **Name**           | **Default Value** | **Description**                    |
| :-------------- | :------------- | :-----------------------------------|
| `cbhealthagent_version` | `0.3.0` | The version of cbhealthagent to install |
| `cbhealthagent_user` | `cbhealthagent` | The name of the cbhealthagent user on the OS. |
| `cbhealthagent_user_group` | `cbhealthagent` | The name of the cbhealthagent user group on the OS. |
| `cbhealthagent_user_shell` | `/usr/sbin/nologin` | By default `/usr/sbin/nologin` is used to prevent the user from logging in, if you're using an existing user account this should be `/bin/bash` |
`cbhealthagent_user_createhome` | `false` | Whether or not to create the home directory for the user |
| `cbhealthagent_couchbase_user_group` | `couchbase` | The user group that Couchbase is using. |
| `cbhealthagent_install_dir` | `/opt/cbhealthagent/bin` | The directory for the binary to be placed in |
| `cbhealthagent_binary` | `cbhealthagent` | The name of the binary to use |
| `cbhealthagent_conf_dir` | `/etc/cbhealthagent` | The configuration directory  |
| `cbhealthagent_env_file` | `service.env` | The name of the environment file the process will load when it starts |
| `cbhealthagent_local_tmp_dir` |  `/tmp/cbhealthagent` | path to where the binary will be downloaded to on the controller |
| `cbhealthagent_local_binary_path` | `""` | Full path to the local binary if already downloaded on the controller.  This should only be set, if ansible is not downloading the binary and you have
| `cbhealthagent_log_level` | `info` | Level to log at.  Can be: debug, info, warn, error |
| `cbhealthagent_couchbase_install_path` | `/opt/couchbase` | The path to where Couchbase Server was installed |
| `cbhealthagent_check_interval` | `10m` | How often to refresh health check data. |
| `cbhealthagent_log_analyzer_janitor_interval` | `10m` | How often to clean up stale log alerts. |
| `cbhealthagent_log_analyzer_alert_duration` | `1h` | How long will log alerts fire before they are cleaned up, if no matching message is seen in the meantime. |
| `cbhealthagent_features_health_agent_enabled` | `true` | Whether or not to enable the health agent |
| `cbhealthagent_features_log_analyzer_enabled` | `true` | Whether or not to enable the log analyzer |
| `cbhealthagent_features_fluent_bit_enabled` | `false` | Whether or not to enable fluent bit |
| `cbhealthagent_features_prometheus_exporter_enabled` | `false`  | Whether or not to enable the prometheus exporter |
| `couchbase_username` | `Administrator` | The Couchbase user to use. |
| `couchbase_password` | `password` | The Couchbase password to use. |
| `cbhealthagent_http_port` | `9082` | Port that exposes the agent REST API |

### Features

Currently the following features are available:

| **Name**      | **Description**                    |
| :------------ | :-----------------------------------|
| `health-agent` | Performs health checks against the local node |
| `log-analyzer` | Parses log messages sent from FluentBit |
| `fluent-bit` | The rpm for `cbhealthagent` will install fluent-bit and the necessary configuration files.  However, it is instead preferred to install FluentBit separately using the [`couchbaselabs.couchbase_fluent_bit`](https://github.com/couchbaselabs/ansible-couchbase-fluent-bit) role which has additional configuration for slow query logging as well as support for sending log messages to `cbhealthagent` |
| `prometheus-exporter` | `cbhealthagent` can optionally start a prometheus exporters.  However, it is preferred to install the exporter separately using the [`couchbaselabs.couchbase_exporter`](https://github.com/couchbaselabs/ansible-couchbase-fluent-bit) role, which has additional configuration options.

## Installation

You can install this role with the `ansible-galaxy` command, and can run it
directly from the git repository.:

```
ansible-galaxy role install git+https://github.com/couchbaselabs/ansible-couchbase-cbhealthagent.git,,couchbaselabs.cbhealthagent
```

It can also be added to a `requirements.yml` file

```yaml
roles:
- name: couchbaselabs.cbmultimanager
  src: https://github.com/couchbaselabs/ansible-couchbase-cbhealthagent.git
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
