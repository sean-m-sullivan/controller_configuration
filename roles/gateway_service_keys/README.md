# Ansible Role infra.aap_configuration.service_keys

## Description

An Ansible Role to configure Service Keys on Ansible Automation gateway.

## Variables

Detailed description of variables are provided in the [top-level README](../../README.md)

Variables specific for this role are following:

| Variable Name                                         |                    Default Value                    | Required | Description                                                                                                                                                        |                                                      |
|:------------------------------------------------------|:---------------------------------------------------:|:--------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------:|
| `service_keys_list` (Alias: `service_keys`)        |           [below](#service-key-arguments)           |   yes    | Data structure describing your service_key entries described below.                                                                                                |                |
| `service_keys_secure_logging`   |  `aap_configuration_secure_logging` OR `false`  |    no    | Whether or not to include the sensitive service_key role tasks in the log. Set this value to `True` if you will be providing your sensitive values from elsewhere. |      |
| `service_keys_enforce_defaults` | `aap_configuration_enforce_defaults` OR `false` |    no    | Whether or not to enforce default option values on only the service key role.                                                                                      |      README.md#enforcing-defaults)      |
| `service_keys_async_retries`    |    `aap_configuration_async_retries` OR `30`    |    no    | This variable sets the number of retries to attempt for the role.                                                                                                  |  |
| `service_keys_async_delay`      |     `aap_configuration_async_delay` OR `1`      |    no    | This sets the delay between retries for the role.                                                                                                                  |  |

## Data Structure

### Service Key Arguments

Options for the `service_keys_list` variable:

| Variable Name            |   Default Value    | Required | Type | Description                                                                      |
|:-------------------------|:------------------:|:--------:|:----:|:---------------------------------------------------------------------------------|
| `name`                   |        N/A         |   yes    | str  | The name of the resource                                                         |
| `new_name`               |        N/A         |    no    | str  | Setting this option will change the existing name (looked up via the name field) |
| `is_active`              | N/A (true by API)  |    no    | bool | Flag for setting the active state of the Service Key                             |
| `service_cluster`        |        N/A         |    no    | str  | ID or name referencing the [Service Cluster](../gateway_service_clusters/README.md)      |
| `algorithm`              | N/A (HS256 by API) |    no    | str  | Algorithm to use for this Service Key. Choices: ["HS256", "HS384", "HS512"]      |
| `secret`                 |        N/A         |    no    | str  | A secret to use for this Service Key. Non-editable                               |
| `secret_length`          |        N/A         |    no    | int  | The number of random bytes in the secret                                         |
| `mark_previous_inactive` |        N/A         |    no    | bool | If true any other secret keys for this service will become inactive              |
| `state`                  |     `present`      |    no    | str  | README.md#state-variable)                                           |

**Unique value:**

- `name`

## Usage

### Json Example

- Check the service key exists (in the database):
- Create a service key

```json
{
  "service_keys_list": [
    {
      "name": "Key 1",
      "state": "exists"
    },
    {
      "name": "Key 2",
      "algorithm": "HS512",
      "secret": "this-is-secret",
      "service_cluster": "Automation Controller"
    }
  ]
}
```

### Yaml Example

- Create inactive key for Controller service
- Delete key (if exists)

File name: `data/gateway_service_keys.yml`

```yaml
---
service_keys_list:
- name: "Controller Key 1"
  is_active: false
  service_cluster: controller
  secret: "gateway-secret"
- name: "Some secret key"
  state: absent
```

### Run Playbook

File name: [manage_data.yml](../../README.md#example-ansible-playbook) can be found in the top-level README.

```shell
ansible-playbook manage_data.yml -e @data/gateway_service_keys.yml
```

## License

[GPL-3.0](https://github.com/redhat-cop/aap_configuration#licensing)
