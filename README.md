# Ansible role for Riot (riot-web)

This role installs and configures [Riot](https://github.com/vector-im/riot-web) on an APT-based
system.
This role does **not** install/configure a reverse proxy in order to serve Riot.
The resulting Riot installation resides in `/opt/riot-web` and you have to install a reverse proxy
yourself or using another role (i.e. [this one](https://github.com/stuvusIT/nginx)).

## Example Playbook

The following is an example playbook which installs and configures Riot.

```yml
- hosts: matrix01
  become: true
  roles:
    - role: riot-web
    vars:
      global_cache_dir: "{{ lookup('env', 'HOME') }}/.cache/stuvus"
      riot_web_archive_url: https://github.com/vector-im/riot-web/releases/download/v1.6.2/riot-v1.6.2.tar.gz
      riot_web_checksum: sha256:e0367432c396d7b392a1c4b559daafd3c5d779fcbfa44c906526419ffae0d2b3
      riot_web_write_config_to_path: /var/www/config.json
      riot_web_config:
        # Content of the config.json goes here.
        #
        # ...
```

Note that this example playbook puts the `config.json` into `/var/www/config.json` (instead of
`/opt/riot-web/config.json`), so your reverse proxy needs additional configuration in order to
serve it at `GET /config.json`.

## Source specification

`riot_web_archive_url` must contain the url where to download an archive that contains the Riot
version to install.
`riot_web_checksum` must contain a checksum (for instance the SHA256 checksum) of that archive.

## Client Configuration

You have to make yourself familiar with
[how to configure Riot](https://github.com/vector-im/riot-web/blob/develop/docs/config.md) in order
to set the role variable `riot_web_config`.
With this role you can, if you like, specify it as YAML and it will be converted to JSON (as needed
by the client).
If you like to specify it as JSON, that's not an issue because any JSON is valid YAML.

## Role Variable Defaults

| Name                            | Default                       | Description                                                                      |
| :------------------------------ | :---------------------------- | :------------------------------------------------------------------------------- |
| `global_cache_dir`              | *required*                    | Path to a directory on the localhost where the riot-web archive is downloaded to |
| `riot_web_archive_url`          | *required*                    | [#source-specification](#source-specification)                                   |
| `riot_web_checksum`             | *required*                    | [#source-specification](#source-specification)                                   |
| `riot_web_config`               | *required*                    | [#client-configuration](#client-configuration)                                   |
| `riot_web_write_config_to_path` | `"/opt/riot-web/config.json"` | The content of `riot_web_config` (converted to JSON) is written to this path     |
