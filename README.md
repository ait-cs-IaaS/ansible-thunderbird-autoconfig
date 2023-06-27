# Ansible Role: thunderbird-autoconfig

Installs and makes thunderbird configurable through environment variables. It is possible to setup E-Mail accounts, contacts, smime certs, etc.

## Role Variables
| Variable name                           | Type            | Default                  | Description                                                                                                                                                                                                                                                                                |
| --------------------------------------- | --------------- | ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| thunderbird_syspref                     | file path       | syspref.js               | Can be used to replace the default syspref file                                                                                                                                                                                                                                            |
| thunderbird_autoconfig                  | file path       | thunderbird.cfg          | Can be used to replace the default autoconfig                                                                                                                                                                                                                                              |
| thunderbird_mail_full_name              | string          |                          | Defines the fullname of the thunderbird mail account                                                                                                                                                                                                                                       |
| thunderbird_mail_address                | email           |                          | Defines the email of the thunderbird mail account                                                                                                                                                                                                                                          |
| thunderbird_mail_user                   | string          | `thunderbird_mail_address` | Defines the user used to log into the mail servers, defaults to the email                                                                                                                                                                                                                  |
| thunderbird_mail_password               | string          |                          | Defines the user password (optional)                                                                                                                                                                                                                                                       |
| thunderbird_mail_cert                   | file path       |                          | Defines the smime certificate file used for signing (optional)                                                                                                                                                                                                                             |
| thunderbird_mail_cert_password          | string          |                          | The password to use for the certificate import ( not functional due to thunderbird bug)                                                                                                                                                                                                    |
| thunderbird_mail_imap                   | hostname/ip     |                          | Defines the address of the imap server                                                                                                                                                                                                                                                     |
| thunderbird_mail_imap_port              | int             | 143                      | Defines the port used for imap                                                                                                                                                                                                                                                             |
| thunderbird_mail_imap_sock              | int             | 2                        | Defines the encryption type used for imap (0: no encryption, 2: STARTSSL, 3: SSL/TLS )                                                                                                                                                                                                     |
| thunderbird_mail_imap_auth              | int             | 3                        | Defines the authentication method used for imap (1: no authentication (smtp only), 3: normal password, 4: encrypted password, 5: Kerberos/GSSAPI, 6: NTLM, 7: TLS Certificate, 8: OAuth2)                                                                                                  |
| thunderbird_mail_pop3                   | hostname/ip     |                          | Defines the address of the pop3 server                                                                                                                                                                                                                                                     |
| thunderbird_mail_pop3_port              | int             | 143                      | Defines the port used for pop3                                                                                                                                                                                                                                                             |
| thunderbird_mail_pop3_sock              | int             | 2                        | Defines the encryption type used for pop3 (0: no encryption, 2: STARTSSL, 3: SSL/TLS )                                                                                                                                                                                                     |
| thunderbird_mail_pop3_auth              | int             | 3                        | Defines the authentication method used for pop3 (1: no authentication (smtp only), 3: normal password, 4: encrypted password, 5: Kerberos/GSSAPI, 6: NTLM, 7: TLS Certificate, 8: OAuth2)                                                                                                  |
| thunderbird_mail_smtp                   | hostname/ip     |                          | Defines the address of the smtp server                                                                                                                                                                                                                                                     |
| thunderbird_mail_smtp_port              | int             | 143                      | Defines the port used for smtp                                                                                                                                                                                                                                                             |
| thunderbird_mail_smtp_sock              | int             | 2                        | Defines the encryption type used for smtp (0: no encryption, 2: STARTSSL, 3: SSL/TLS )                                                                                                                                                                                                     |
| thunderbird_mail_smtp_auth              | int             | 3                        | Defines the authentication method used for smtp (1: no authentication (smtp only), 3: normal password, 4: encrypted password, 5: Kerberos/GSSAPI, 6: NTLM, 7: TLS Certificate, 8: OAuth2)                                                                                                  |
| thunderbird_mail_mode                   | string          | imap                     | Defines the email retrieval mode i.e. imap or pop3                                                                                                                                                                                                                                         |
| thunderbird_config                      | list[dict]      |                          | Configures the mail accounts using the supplied dict. This supersedes the other variables i.e. if it is set all other `thunderbird_mail_*` variables (excluding `thunderbird_mail_lists`) will be ignored.                                                                                 |
| &nbsp;&nbsp;&nbsp;&nbsp;∟.name          | string          |                          | Fullname of the account user                                                                                                                                                                                                                                                               |
| &nbsp;&nbsp;&nbsp;&nbsp;∟.email         | email           |                          | E-mail address                                                                                                                                                                                                                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;∟.user          | string          |  | Username used for the mail servers                                                                                                                                                                                                                                                         |
| &nbsp;&nbsp;&nbsp;&nbsp;∟.password      | string          |                          | User Password (optional)                                                                                                                                                                                                                                                                   |
| &nbsp;&nbsp;&nbsp;&nbsp;∟.cert          | file path       |                          | Path to the certificate used for signing (optional)                                                                                                                                                                                                                                        |
| &nbsp;&nbsp;&nbsp;&nbsp;∟.cert_pw       | string          |                          | The password to use for the certificate import ( not functional due to thunderbird bug)                                                                                                                                                                                                    |
| &nbsp;&nbsp;&nbsp;&nbsp;∟.retrievServer | hostname/ip     |                          | IMAP/POP3 server address                                                                                                                                                                                                                                                                   |
| &nbsp;&nbsp;&nbsp;&nbsp;∟.retrievPort   | int             |                          | IMAP/POP3 port number                                                                                                                                                                                                                                                                      |
| &nbsp;&nbsp;&nbsp;&nbsp;∟.retrievSock   | int             |                          | Defines the encryption type used for imap/pop3 (0: no encryption, 2: STARTSSL, 3: SSL/TLS                                                                                                                                                                                                  |
| &nbsp;&nbsp;&nbsp;&nbsp;∟.retrievAuth   | int             |                          | Defines the authentication method used for imap/pop3 (1: no authentication (smtp only), 3: normal password, 4: encrypted password, 5: Kerberos/GSSAPI, 6: NTLM, 7: TLS Certificate, 8: OAuth2)                                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;∟.mode          | imap/pop3       |                          | Defines retrieval server type                                                                                                                                                                                                                                                              |
| &nbsp;&nbsp;&nbsp;&nbsp;∟.smtpServer    | hostname/ip     |                          | SMTP server address                                                                                                                                                                                                                                                                        |
| &nbsp;&nbsp;&nbsp;&nbsp;∟.smtpPort      | int             |                          | SMTP port number                                                                                                                                                                                                                                                                           |
| &nbsp;&nbsp;&nbsp;&nbsp;∟.smtpSock      | int             |                          | Defines the encryption type used for smtp (0: no encryption, 2: STARTSSL, 3: SSL/TLS                                                                                                                                                                                                       |
| &nbsp;&nbsp;&nbsp;&nbsp;∟.smtpAuth      | int             |                          | Defines the authentication method used for smtp (1: no authentication (smtp only), 3: normal password, 4: encrypted password, 5: Kerberos/GSSAPI, 6: NTLM, 7: TLS Certificate, 8: OAuth2)                                                                                                  |
| thunderbird_contacts                    | list[dict]      |                          | Configures the thunderbird address book. Expects a list of dictionaries containing thunderbird nsIAbCard properties. See https://developer.mozilla.org/en-US/docs/Mozilla/Tech/XPCOM/Reference/nsIAbCard_(Tb3)                                                                             |
| thunderbird_mail_lists                  | list[dict]      |                          | Configures thunderbird mailing lists.                                                                                                                                                                                                                                                      |
| &nbsp;&nbsp;&nbsp;&nbsp;∟.name          | string          |                          | Name of the Mailing List                                                                                                                                                                                                                                                                   |
| &nbsp;&nbsp;&nbsp;&nbsp;∟.nickname      | string          |                          | Nickname used for the mailing list (optional)                                                                                                                                                                                                                                              |
| &nbsp;&nbsp;&nbsp;&nbsp;∟.description   | string          |                          | Description for the mailling list (optional)                                                                                                                                                                                                                                               |
| &nbsp;&nbsp;&nbsp;&nbsp;∟.contacts      | list[dict]      |                          | List of contacts part of the mailing list (same format as `thunderbird_contacts`). Note that contacts defined here overwrite contacts defined in `thunderbird_contacts` if they use the same primary email. This behavior can be avoided by only providing the primary email in this list. |
| thunderbird_cert_files                  | list[file path] |                          | List of CA certificate file locations to be installed in thunderbird.                                                                                                                                                                                                                      |
### Example configs

Setup configuring a single E-Mail account through the `thunderbird_mail_*`` variables:


```yaml
- hosts: "{{ test_host | default('localhost') }}"
  roles:
    - role: thunderbird-autoconfig
      vars:
        thunderbird_mail_full_name: Example Name
        thunderbird_mail_address: test@example.com
        thunderbird_mail_password: test
        thunderbird_mail_imap: 127.0.0.1
        thunderbird_mail_imap_port: 143
        thunderbird_mail_imap_sock: 2
        thunderbird_mail_imap_auth: 3
        thunderbird_mail_smtp: 127.0.0.1
        thunderbird_mail_smtp_port: 587
        thunderbird_mail_smtp_sock: 2
        thunderbird_mail_smtp_auth: 3
```


Setup configuring two E-Mail accounts through thunderbird_config:


```yaml
- hosts: "{{ test_host | default('localhost') }}"
  roles:
    - role: thunderbird-autoconfig
      vars:
       thunderbird_config:
         - name: Test
           email: test@local
           user: test@local
           smtpServer: 127.0.0.1
           smtpPort: 433
           smtpSock: 2
           smtpAuth: 3
           retrievServer: 127.0.0.1
           retrievPort: 533
           retrievSock: 2
           retrievAuth: 3
           mode: imap
           cert: /home/test/smime_test_user.p12
           cert_pw: test
         - name: Test2
           email: test2@local
           user: test2@local
           smtpServer: 127.0.0.1
           smtpPort: 433
           smtpSock: 2
           smtpAuth: 3
           retrievServer: 127.0.0.1
           retrievPort: 533
           retrievSock: 2
           retrievAuth: 3
           mode: imap
           cert: /home/test/smime_test_user2.p12
```


Setup configuring a single E-Mail account through thunderbird_config, two contacts, and a mailing list:

```yaml
- hosts: "{{ test_host | default('localhost') }}"
  roles:
    - role: thunderbird-autoconfig
      vars:
        thunderbird_config:
          - name: Test
            email: test@local
            user: test@local
            smtpServer: 127.0.0.1
            smtpPort: 433
            smtpSock: 2
            smtpAuth: 3
            retrievServer: 127.0.0.1
            retrievPort: 533
            retrievSock: 2
            retrievAuth: 3
            mode: imap
            cert: /home/test/smime_test_user.p12
            cert_pw: test
        thunderbird_contacts:
          - FirstName: Bob
            LastName: Example
            PrimaryEmail: test@example.com
            SecondEmail: test@example2.com
            DisplayName: TestBob
            NickName: testy
          - FirstName: Alice
            LastName: Example
            PrimaryEmail: alice@example.com
        thunderbird_mail_lists:
          - name: Example Liste
            nickname: example
            description: This is a example mailing list
            contacts:
            - PrimaryEmail: test@example.com
            - PrimaryEmail: alice@example.com
```

# Licence

 GPL-3.0

# Author information

 This role was created in 2019 by [Maximilian Frank](https://frank-maximilian.at)
