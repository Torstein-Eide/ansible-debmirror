# Debmirror 


A brief description of the role goes here.

## Requirements

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

## Role Variables

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. 


Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

### Globle variables
| Variables               | Description                                                                                                                               |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| `debmirror_apt_keys`    | source of PGP keys Default: `files/PGP_keys/*`                                                                                            |
| `debmirror_keyring`     | If PGP use a different file for keyring than  `trustedkeys.kbx`                                                                           |
| `debmirror_config_pat`  | Folder for config file Default: `/etc/debmirror`                                                                                          |
| `debmirror_mirror_path` | Folder for downloading of mirror Default: `/opt/mirror`                                                                                   |
| `debmirror_packages`    | Package that are required. Default: `['debmirror' , 'gnupg' , 'xz-utils']`                                                           |
| `debmirror_nice_lvl`    | [Nice level](https://en.wikipedia.org/wiki/Nice_\(Unix\))                                                                                 |
| `debmirror_clock`       | System timer for running of service. Default: `0,8,16:30`                                                                                 |
| `debmirror_user`        | User for service and folder Default: `apt_mirror`                                                                                         |
| `debmirror_group`       | Group for service and folder Default:  `apt_mirror`                                                                                       |
| `debmirror_slow_cpu`       | Enable slow CPU mode `--slow-cpu`  Default: `False`                                            |
| `debmirror_mount`       | Sets it as [requirermet systemd](https://www.freedesktop.org/software/systemd/man/systemd.unit.html#Requires=)   Example: `volume1.mount` |

### `debmirror_config`, config variables

Example with config tree for `debian`, `raspbian` and `ubuntu`, under 
```` 
debmirror_config
├── debian 
├── raspbian
└── ubuntu
```` 
#### **Required** 

| Ansible variable | Debmirror argument | Desciption                                 |
|------------------|--------------------|--------------------------------------------|
| `mirrordir`      | `mirrordir`        | If not set `debmirror_mirror_path` is used |
| `remote_host`    | `-h, --host`       | Remote host.                               |
| `remote_root`    | `-r, --root`       | Remote root                                |
| `release`        | `-d, --dist`       | List convert to array                      |
| `sections`       | `-s, --section`    | List convert to array                      |
| `architectures`  | `-a, --arch`       | List convert to array                      |


#### Optional

| Ansible variable       | Debmirror argument             | Desciption                                                |
|------------------------|--------------------------------|-----------------------------------------------------------|
| `verbose`              | `-v, --verbose`                | Default: `False`                                          |
| `progress`             | `-p, --progress`               | Default: `False`                                          |
| `debug`                | `--debug`                      | Default: `False`                                          |
| `dry_run`              | `--dry-run`                    | Default: `False`                                          |
| `user`                 | `-u, --user`                   | Remote username. Defaults to `anonymous`                  |
| `passwd`               | `--passwd`                     | Remote user password. defaults to `anonymous@`            |
| `download_method`      | `--method`                     | Accepted `ftp\|http\|https\|rsync\|file`.Defaults: `http` |
| `ignores`              | `--ignore`                     | List convert to array. Default: emtpy                     |
| `excludes`             | `--exclude`                    | List convert to array. Default: emtpy                     |
| `includes`             | `--include`                    | List convert to array. Default: emtpy                     |
| `excludes_deb_section` | `--exclude-deb-section`        | List convert to array. Default: emtpy                     |
| `limit_priority`       | `--limit-priority`             | List convert to array. Default: emtpy                     |
| `omit_suite_symlinks`  | `--omit-suite-symlinks`        | Default: `False`                                          |
| `skippackages`         | `--skippackages`               | Default: `False`                                          |
| `i18n`                 | `--i18n`                       | Default: `False`                                          |
| `getcontents`          | `--getcontents`                | Default: `False`                                          |
| `do_source`            | `--source, --nosource`         | Default: `True`                                           |
| `max_batch`            | `--max-batch`                  | Default: not enabled                                      |
| `rsync_extra`          | `--rsync-extra`                | List convert to array. Default: emtpy                     |
| `di_dists`             | `--di-dist`                    | List convert to array. Default: emtpy                     |
| `di_archs`             | `--di-arch`                    | List convert to array. Default: emtpy                     |
| `state_cache_days`     | `--state-cache-days`           | Default: `0`                                              |
| `check_gpg`            | `--check-gpg, --no-check-gpg ` | Default: `True`                                           |
| `ignore_release_gpg`   | `--ignore-release-gpg`         | Default: `False`                                          |
| `ignore_release`       | `--ignore-missing-release`     | Default: `False`                                          |
| `check_md5sums`        | `-checksums`                   | Default: `False`                                          |
| `pre_cleanup`          | `--precleanup, --cleanup`      | Default: `False`                                          |
| `post_cleanup`         | `--postcleanup`                | Default: True                                             |
| `timeout`              | `-t, --timeout`                | Default: `300`                                            |
| `rsync_batch`          | `--rsync-batch`                | Default: `200`                                            |
| `rsync_options`        | `--rsync-options`              | Default: `-aIL –partial`                                  |
| `passive`              | `--passive`                    | Default: `False`                                          |
| `proxy`                | `--proxy`                      | Default: not enabled                                      |
| `diff_mode`            | `--diff`                       | Accepted `use\|mirror\|none`. Default: `use`              |
### Sources:

- [debmirror man page](https://linux.die.net/man/1/debmirror)
- [ubuntu Wiki: debmirror](https://help.ubuntu.com/community/Debmirror)

## Dependencies



## Example Playbook

````yml
    - hosts: servers
      roles:
         - ansible-deb-mirror
      vars:
        debmirror_mirror_path: /volume1/debmirror
        debmirror_config:
          ubuntu:
            progress: false
            remote_host: no.archive.ubuntu.com
            remote_root: /ubuntu
            i18n: true
            do_source: true
            max_batch: 200
            state_cache_days: 14
            post_cleanup: true
            timeout: 300
            download_method: http
            diff_mode: use  # use|mirror|none
            architectures:
              - amd64
            release:
              - focal
              - focal-security
              - focal-updates
            sections:
              - main
              - restricted
              - universe
              - multiverse
            excludes:
              - '/Translation-.*'
              - '/Translation-en.gz'
              - '/language-pack-.*'
              - '(thunderbird|firefox)-locale-.*'
              - '/libreoffice-l10n.*'
              - '/manpages-(\w{2}|\w{2}-extra|\w{2}-dev|\w{2}-extra-dev)_.*'
              - '/myspell-(\w{2}|\w{2}-\w{2,3}|)[-_].*'
              - '/unidic-mecab.*'
              - '/libreoffice-dev-doc.*'
              - '/thunderbird-dbg.*'
              - '/firefox-dbg.*'
              - '/texlive-fonts-extra.*'
            includes:
              - '/myspell-(nb|nn).*'
              - '/libreoffice-l10n-(nb|nn).*'
              - '(thunderbird|firefox)-locale-(nb|nn).*'
              - '/language-pack-?(gnome|kde)-(no|en).*'
              - '/Translation-(no|en\.|en_GB|en_US).*'
````

## License

BSD

## Author Information

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
