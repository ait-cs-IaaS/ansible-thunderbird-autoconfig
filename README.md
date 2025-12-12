# Ansible Role: thunderbird-autoconfig

Provision Thunderbird with a locked auto-config, pre-seeded accounts/contacts, optional CA certificates, and multi-profile support (VNC-friendly by default).

## What this role does
- Stops Thunderbird and removes the existing profile directory before applying changes.
- Installs `syspref.js` and `thunderbird.cfg` under `/etc/thunderbird` and `/usr/lib/thunderbird`.
- Creates `profiles.ini`, `installs.ini`, and profile directories for `profile.default` plus every entry in `thunderbird_profile_names`.
- Patches `/usr/lib/thunderbird/thunderbird.sh` so VNC sessions use `vnc<DISPLAY>` profile names, matching the defaults in `thunderbird_profile_names`.
- Imports trusted CA certificates and seeds accounts/address book entries on first start when provided.

## Role Variables

### Profile and file layout
| Variable | Default | Description |
| --- | --- | --- |
| `thunderbird_user` | auto-detected first user with UID >= 1000 | User whose Thunderbird config is managed; set explicitly to override detection. |
| `thunderbird_config_dir` | `/home/{{ thunderbird_user }}/.thunderbird` | Thunderbird configuration directory that will be replaced. |
| `thunderbird_syspref` | `syspref.js` | Source file copied to `/etc/thunderbird/syspref.js` to activate autoconfig. |
| `thunderbird_autoconfig` | `thunderbird.cfg.j2` | Template/file rendered to `/usr/lib/thunderbird/thunderbird.cfg`. |
| `thunderbird_profiles_ini` | `profiles.ini` | Template used to write `profiles.ini` inside `thunderbird_config_dir`. |
| `thunderbird_installs_ini` | `installs.ini` | Template used to write `installs.ini` inside `thunderbird_config_dir`. |
| `thunderbird_profiles_version` | `FDC34C9F024745EB` | Install identifier used in the ini files. |
| `thunderbird_profile_names` | `['vnc0', 'vnc1', 'vnc2']` | Extra profiles created alongside `profile.default`; the patched launcher will pick `vnc<DISPLAY>` automatically, so keep names in sync with expected display numbers. |

### Account and data setup
| Variable | Default | Description |
| --- | --- | --- |
| `thunderbird_config` | `[]` | List of mail accounts to create (see account fields below). |
| `thunderbird_contacts` | `[]` | List of address-book entries to add on first start. |
| `thunderbird_trusted_certs` | `[]` | List of CA certificate file paths that will be imported into Thunderbird. |

### `thunderbird_config` account fields
| Field | Default | Description |
| --- | --- | --- |
| `name` | falls back to `email` | Display name for the account. |
| `email` | required | Account email address. |
| `user` | required | Username for IMAP/POP3 and SMTP. |
| `password` | `null` | Password stored in the login manager when provided. |
| `retrievServer` | required | IMAP/POP3 server hostname. |
| `retrievPort` | `143` | Retrieval port. |
| `retrievSock` | `2` | Retrieval socket option (0 none, 2 STARTTLS, 3 SSL/TLS). |
| `retrievAuth` | `3` | Retrieval auth method (1 none, 3 normal password, 4 encrypted password, 5 Kerberos/GSSAPI, 6 NTLM, 7 TLS cert, 8 OAuth2). |
| `mode` | `imap` | Retrieval mode placeholder (currently stored but not consumed by the template). |
| `smtpServer` | required | SMTP server hostname. |
| `smtpPort` | `587` | SMTP port. |
| `smtpSock` | `2` | SMTP socket option (0 none, 2 STARTTLS, 3 SSL/TLS). |
| `smtpAuth` | `3` | SMTP auth method (values as above). |
| `cert` | `null` | Optional S/MIME certificate file (import depends on Thunderbird support). |
| `cert_pw` | `null` | Password used for `cert` when required. |

## Example playbook

Provision multiple VNC profiles with one configured account, seeded contacts, and imported CA certs:

```yaml
- hosts: "{{ test_host | default('localhost') }}"
  roles:
    - role: thunderbird-autoconfig
      vars:
        thunderbird_profile_names:
          - vnc0
          - vnc1
          - vnc2
          - vnc3
        thunderbird_trusted_certs:
          - files/ca.crt
        thunderbird_config:
          - name: Test User
            email: test@local
            user: test@local
            password: test
            smtpServer: 127.0.0.1
            smtpPort: 587
            smtpSock: 2
            smtpAuth: 3
            retrievServer: 127.0.0.1
            retrievPort: 993
            retrievSock: 3
            retrievAuth: 3
            mode: imap
        thunderbird_contacts:
          - FirstName: Bob
            LastName: Example
            PrimaryEmail: test@example.com
            DisplayName: Bob Example
          - FirstName: Alice
            LastName: Example
            PrimaryEmail: alice@example.com
```

# Licence

 GPL-3.0

# Author information

 This role was created in 2019 by [Maximilian Frank](https://frank-maximilian.at) adapted by Lenhard Reuter (AIT)
