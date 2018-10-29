# ait.phusion.thunderbird

Installs and makes thunderbird configurable through environment variables. It is possible to setup E-Mail accounts, contacts, smime certs, etc.
## Requirements

Requires the init process provided by the [phusion/baseimage](https://hub.docker.com/r/phusion/baseimage/) and assumes unity desktop is installed.

## Role Variables

Variables available as per cyberrange base client docker image:
- `default_user`: ubuntu  
   The default user made available by the baseimage
- `user`: ubuntu  
   The user to be used
- `img_home`: "/opt/unity-vnc"  
   Directory containing script and config file specific to the cyberrange base image
- `img_templates`: "/opt/unity-vnc/templates"  
   Directory that should be used to store templates, which are rendered on startup

## Environment Variables

- `MAIL_FULL_NAME`
    Defines the fullname of the thunderbird mail account

- `MAIL_ADDRESS`
    Defines the email of the thunderbird mail account

- `MAIL_USER`
    Defines the user used to log into the mail servers (defaults to `MAIL_ADDRESS`)

- `MAIL_PASSWORD`
    Defines the user password (optional)

- `MAIL_EMAIL_CERT`
    Defines the smime certificate file used for signing (optional)

- `MAIL_IMAP`
    Defines the address of the imap server

- `MAIL_IMAP_PORT`
    Defines the port used for imap (default: 143)    

- `MAIL_IMAP_SOCK`
    Defines the encryption type used for imap
    (default: STARTTLS, see `MAIL_JSON` for details) overrides `MAIL_SOCK`

- `MAIL_IMAP_AUTH`
    Defines the authentication method used for imap  
    (default: normal password, see `MAIL_JSON` for details) ovverides `MAIL_AUTH`

- `MAIL_POP3`
    Defines the address of the pop3 server

- `MAIL_POP3_PORT`
    Defines the port used for pop3 (default: 110)

- `MAIL_POP3_SOCK`
    Defines the encryption type used for pop3
    (default: STARTTLS, see `MAIL_JSON` for details) overrides `MAIL_SOCK`

- `MAIL_POP3_AUTH`
    Defines the authentication method used for pop3
    (default: normal password, see `MAIL_JSON` for details) ovverides `MAIL_AUTH`

- `MAIL_MODE`
    Defines the email retrieval mode i.e. imap or pop3

- `MAIL_SMTP`
    Defines the address of the smtp server

- `MAIL_SMTP_PORT`
    Defines the port used for smtp (default: 587)

- `MAIL_SMTP_SOCK`
    Defines the encryption type used for smtp
    (default: STARTTLS, see `MAIL_JSON` for details) overrides `MAIL_SOCK`

- `MAIL_SMTP_AUTH`
    Defines the authentication method used for smtp
    (default: normal password, see `MAIL_JSON` for details) ovverides `MAIL_AUTH`

- `MAIL_JSON`
    Configures the mail accounts using the supplied json string.
    This supersedes the other variables i.e. if `MAIL_JSON` is set all other `MAIL_*`
    variables (excluding `MAIL_CONTACTS_JSON`) will be ignored.
    `MAIL_JSON` expects a list of dictionaries containing the following key value pairs:
        - name: string
            Fullname of the account user
        - email: string
            E-mail address
        - user: string
            Username used for the mail servers
        - password: string
            User Password (optional)
        - cert: string
            Path to the certificate used for signing (optional)
        - smtpServer: string
            SMTP server address
        - smtpPort: int
            SMTP port number
        - smtpSock: int
            SMTP encryption type see below for values
        - smtpAuth: int
            SMTP authentication type see below for values
        - retrievServer: string
            IMAP/POP3 server address
        - retrievPort: int
            IMAP/POP3 port number
        - retrievSock: int
            IMAP/POP3 encryption type see below for values
        - retrievAuth: int
            IMAP/POP3 authentication type see below for values
        - mode: "imap" or "pop3"
            Defines retrieval server type

        smtpSock and retrievSock values can be the following:
            - 0: no encryption
            - 2: STARTTLS
            - 3: SSL/TLS
        smtpAuth and retrievAuth values can be the following:
            - 1: no authentication (smtp only)
            - 3: normal password
            - 4: encrypted password
            - 5: Kerberos/GSSAPI
            - 6: NTLM
            - 7: TLS Certificate
            - 8: OAuth2

- `MAIL_CONTACTS_JSON`
    Configures the thunderbird address book using the supplied json string.
    `MAIL_CONTACTS_JSON` expects a list of dictionaries containing thunderbird nsIAbCard properties.
    See https://developer.mozilla.org/en-US/docs/Mozilla/Tech/XPCOM/Reference/nsIAbCard_(Tb3)

- `MAIL_LISTS_JSON`
    Configures thunderbird mailing lists.
    `MAIL_LISTS_JSON` expects a list of dictionaries with the following key value pairs:
        - name: string
        - nickname: string (optional)
        - description: string (optional)
        - contacts: list of contacts
            Same type of list of contacts as `MAIL_CONTACTS_JSON`.
            Note that contacts defined here can overwrite contacts defined by `MAIL_CONTACTS_JSON`,
            if they have the same primary email. It is also possible to add an existing contact
            to a mailing list by only defining the primary email property.
- `MAIL_CERT_FILES`
  A json list of CA certificate file locations to be installed in thunderbird.

#### Example docker run commands

Setup configuring a single E-Mail account through the MAIL_* environment variables:

```
docker run -d --rm -p 5901:5901 -e PASSWORD=test -e SUDO=yes \
    -e MAIL_FULL_NAME="Example Name" -e MAIL_ADDRESS="test@example.com" -e MAIL_PASSWORD="password" \
    -e MAIL_IMAP="imap.example.com" -e MAIL_IMAP_PORT=143 -e MAIL_IMAP_SOCK=2 -e MAIL_IMAP_AUTH=3   \
    -e MAIL_SMTP="smtp.example.com" -e MAIL_SMPT_PORT=587 -e MAIL_SMTP_SOCK=2 -e MAIL_SMTP_AUTH=3   \
    clients/ubuntu_unity
```

Setup configuring two E-Mail accounts throguh MAIL_JSON:

```
docker run -d --rm -p 5901:5901 -e PASSWORD=test -e SUDO=yes \
    -e MAIL_JSON='[{"name": "Test1", "email": "test1@test.com", "user": "test1@test.com", "smtpServer": "test.com", "smtpPort": 433, "smtpSock": 2, "smtpAuth": 3, "retrievServer": "test.com", "retrievPort": 533, "retrievSock": 2, "retrievAuth": 3, "mode": "imap"},{"name": "Test2", "email": "test2@test.com", "user": "test2@test.com", "smtpServer": "test.com", "smtpPort": 433, "smtpSock": 2, "smtpAuth": 3, "retrievServer": "test.com", "retrievPort": 533, "retrievSock": 2, "retrievAuth": 3, "mode": "imap"}]' \
    clients/ubuntu_unity    
```

Setup configuring a single E-Mail account through MAIL_JSON and a single contact:

```
docker run -d --rm -p 5901:5901 -e PASSWORD=test -e SUDO=yes \
    -e MAIL_JSON='[{"name": "Test", "email": "test@test.com", "user": "test@test.com", "smtpServer": "test.com", "smtpPort": 433, "smtpSock": 2, "smtpAuth": 3, "retrievServer": "test.com", "retrievPort": 533, "retrievSock": 2, "retrievAuth": 3, "mode": "imap"}]' \
    -e MAIL_CONTACTS_JSON='[{"FirstName": "Bob", "LastName": "Example", "PrimaryEmail": "test@example.com", "SecondEmail": "test@example2.com", "DisplayName": "TestBob", "NickName": "testy"}]' \
    clients/ubuntu_unity    
```

Setup configuring a single E-Mail account, two contacts and a mailing list:

```
docker run -d --rm -p 5901:5901 -e PASSWORD=test -e SUDO=yes \
    -e MAIL_JSON='[{"name": "Test", "email": "test@test.com", "user": "test@test.com", "smtpServer": "test.com", "smtpPort": 433, "smtpSock": 2, "smtpAuth": 3, "retrievServer": "test.com", "retrievPort": 533, "retrievSock": 2, "retrievAuth": 3, "mode": "imap"}]' \
    -e MAIL_CONTACTS_JSON='[{"FirstName": "Bob", "LastName": "Example", "PrimaryEmail": "test@example.com", "SecondEmail": "test@example2.com", "DisplayName": "TestBob", "NickName": "testy"},{"FirstName": "Alice", "LastName": "Example", "PrimaryEmail": "alice@example.com"}]' \
    -e MAIL_LISTS_JSON='[{"name": "Example Liste", "nickname": "example", "description": "This is a example mailing list", "contacts": [{"PrimaryEmail": "test@example.com"},{"PrimaryEmail": "alice@example.com"}]}]' \
    clients/ubuntu_unity  
```
