# Ansible role for Element (element-web)

This role installs and configures [Element](https://github.com/vector-im/element-web) on an APT-based
system.
This role does **not** install/configure a reverse proxy in order to serve Element.
The resulting Element installation resides in `/opt/element-web` and you have to install a reverse proxy
yourself or using another role (i.e. [this one](https://github.com/stuvusIT/nginx)).

## Example Playbook

The following is an example playbook which installs and configures Element.

```yml
- hosts: matrix01
  become: true
  roles:
    - role: element-web
    vars:
      global_cache_dir: "{{ lookup('env', 'HOME') }}/.cache/stuvus"
      element_web_archive_url: https://github.com/vector-im/element-web/releases/download/v1.7.15/element-v1.7.15.tar.gz
      element_web_checksum: sha256:6528b438b0397723d79f53656649210a65a1ad2af27f28ff3ba531d653b5da7b
      element_web_write_config_to_path: /var/www/config.json
      element_web_config:
        # Content of the config.json goes here.
        #
        # ...
```

Note that this example playbook puts the `config.json` into `/var/www/config.json` (instead of
`/opt/element-web/config.json`), so your reverse proxy needs additional configuration in order to
serve it at `GET /config.json`.

## Source specification

`element_web_archive_url` must contain the url where to download an archive that contains the Element
version to install.
`element_web_checksum` must contain a checksum (for instance the SHA256 checksum) of that archive.

## Client Configuration

You have to make yourself familiar with
[how to configure Element](https://github.com/vector-im/element-web/blob/develop/docs/config.md) in
order to set the role variable `element_web_config`.
With this role you can, if you like, specify it as YAML and it will be converted to JSON (as needed
by the client).
If you like to specify it as JSON, that's not an issue because any JSON is valid YAML.

## Role Variable Defaults

| Name                               | Default                          | Description                                                                         |
| :--------------------------------- | :------------------------------- | :---------------------------------------------------------------------------------- |
| `global_cache_dir`                 | *required*                       | Path to a directory on the localhost where the element-web archive is downloaded to |
| `element_web_archive_url`          | *required*                       | [#source-specification](#source-specification)                                      |
| `element_web_checksum`             | *required*                       | [#source-specification](#source-specification)                                      |
| `element_web_config`               | *required*                       | [#client-configuration](#client-configuration)                                      |
| `element_web_write_config_to_path` | `"/opt/element-web/config.json"` | The content of `element_web_config` (converted to JSON) is written to this path     |
