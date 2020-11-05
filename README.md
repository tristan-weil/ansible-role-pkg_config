# Ansible Role: OpenBSD_pkg_config

An Ansible Role that configures the package manager.

[![Actions Status](https://github.com/tristan-weil/ansible-role-pkg_config_config/workflows/molecule/badge.svg?branch=master)](https://github.com/tristan-weil/ansible-role-pkg/actions)

## Role Variables

Available variables are listed below, (see also `defaults/main.yml`).

### OpenBSD

Mandatory variables:

| Variable      | Description |
| :------------ | :---------- |

Optional variables:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |
| pkg_config_pkg_add_installurl | https://cdn.openbsd.org/pub/OpenBSD | the url of the OpenBSD repository |

### Debian

Except for `/etc/apt/sources.list`, all files use the DEB822-style format.

Mandatory variables:

| Variable      | Description |
| :------------ | :---------- |
| apt_config_sources_list_path | the path of the source file |
| apt_config_sources_list_repos | a list of <*debian_repository*> |
| apt_config_preferences_name | the name of the preference file |
| apt_config_preferences_pkgs | a list of <*debian_package_preferences*> |

Optional variables:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |
| apt_config_sources_list_state | present | *present / absent* : the state of the source file |
| apt_config_preferences_state | present | *present / absent* : the state of the preference file |  
| apt_config_key_urls | [] | a list of <*debian_keys_urls*> |
| apt_config_key_server | [] | a list of <*debian_keyservers_and_ids*> |

#### <*debian_repository*>

Mandatory variables:

| Variable      | Description |
| :------------ | :---------- |
| uri           | the uri of the repository |
| suites        | the distribution |
| components    | the components |  

Optional variables:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |
| architectures | | |
| languages | | |
| targets | | |
| pdiffs | | |
| by_hash | | |
| allow_insecure | | |
| allow_weak | | |
| allow_downgrade_to_insecure | | |
| trusted | | |
| signed_by | | |
| check_valid_until | | |
| valid_until_min | | |
| valid_until_max | | |

#### <*debian_package_preferences*>

Mandatory variables:

| Variable      | Description |
| :------------ | :---------- |
| uri           | the uri of the repository |
| suites        | the distribution |
| components    | the components |  

Optional variables:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |
| pin_origin | | |
| origin | | | 
| codename | | |
| label | | |
| component | | |
| priority | | |

#### <*debian_keys_urls*>

Some repositories need to be trusted by using a key fetched from an URL or from a file.

Mandatory variables:

| Variable      | Description |
| :------------ | :---------- |
| url           | the url of the key file |

Optional variables:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |
| state         | present | *present / absent* : the state of the key |

#### <*debian_keyservers_and_ids*>

Some repositories need to be trusted by using a key fetched from a keyserver.

Mandatory variables:

| Variable      | Description |
| :------------ | :---------- |
| keyserver     | the keyserver uri |
| id            | the ID of the key |

Optional variables:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |

## Example Playbook

    - hosts: 'webservers'
      roles:
        - role: 'ansible-role-pkg_config_config_config'
          pkg_config_apt_sources_list_path: '/etc/apt/sources.list'
          pkg_config_apt_sources_list_repos:
            - uri: 'http://deb.debian.org/debian/'
              suites: "{{ ansible_facts['distribution_release'] }}"
              components: 'main contrib non-free'
        
            - uri: 'http://deb.debian.org/debian/'
              suites: "{{ ansible_facts['distribution_release'] }}-updates"
              components: 'main contrib non-free'
        
            - uri: 'http://security.debian.org/'
              suites: "{{ ansible_facts['distribution_release'] }}/updates"
              components: 'main contrib non-free'
        
            - uri: 'http://deb.debian.org/debian/'
              suites: "{{ ansible_facts['distribution_release'] }}-backports"
              components: 'main contrib non-free'

        - role: 'ansible-role-pkg_config_config_config'
          pkg_config_apt_sources_list_path: '/etc/apt/sources.list.d/haproxy.sources'
          pkg_config_apt_sources_list_repos:
            - uri: 'http://haproxy.debian.net/'
              suites: "stretch-backports-2.0"
              components: 'main'
          pkg_config_apt_key_urls:
            - url: 'https://haproxy.debian.net/bernat.debian.org.gpg'

## Todo

None.

## Dependencies

None.

## Supported platforms

See [meta/main.yml](https://github.com/tristan-weil/ansible-role-pkg_config/blob/master/meta/main.yml)

## License

See [LICENSE.md](LICENSE.md)
