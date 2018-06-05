# Dockerized IMAP server#

SMTP / IMAP / POP3 server for debugging and automated testing.

**IMPORTANT**: This image is **ONLY** for developing/debugging proposes

This docker image is based on https://github.com/tomav/docker-mailserver
If you look for a docker image for production environment, then go here:
https://hub.docker.com/r/tvial/docker-mailserver/

This image is even simpler than `tvial` docker image. Includes only 
Postfix (SMTP) and Dovecot (IMAP, POP3) servers with one catchall mailbox 
`debug@example.org` for all emails. So, it's very useful for debugging. Optionally, you can define another normal mailbox.

Every email received via SMTP will be delivered locally to `debug@example.org`, so it's safe for testing a web application sending emails with a production list of emails.

Using your favorite email client you can connect via IMAP protocol to see emails like original recipient would received them


## Run container with docker compose

```bash
cp docker-compose.yml.dist docker-compose.yml
```

Edit ```docker-compose.yml``` for set these environment variables:

- MAILNAME: Mail domain (by default, `localdomain.test`)
- MAIL_ADDRESS: Normal user mailbox email address (optional)
- MAIL_PASS: Normal user mailbox password

```bash
docker-compose up
```

Configure your email client with these parameters and test it sending 
any email to any email address 

## GitLab-CI integration as service
```yaml
services:
  - name: virtua-sa/docker-mail-devel
    alias: mail
```

### Catch all debug mailbox

- **IMAP server:** `mail`
- **IMAP encryption:** `SSL` or `No`
- **IMAP port:** `993` or `143`
- **IMAP username:** `debug@localdomain.test` (change `localdomain.test` by your `MAILNAME`)
- **IMAP password:** `debug`

- **POP3 server:** `mail`
- **POP3 encryption:** `SSL` or `No`
- **POP3 port:** `995` or `110`
- **POP3 username:** `debug@localdomain.test` (change `localdomain.test` by your `MAILNAME`)
- **POP3 password:** `debug`

- **SMTP server:** `mail`
- **SMTP encryption:** `No`
- **SMTP port:** `25`
- **SMTP authentication:** `none`

### Normal user mailbox (Optional)

- **IMAP server:** `mail`
- **IMAP encryption:** `SSL` or `No`
- **IMAP port:** `993` or `143`
- **IMAP username:** `address@localdomain.test` (change `address@localdomain.test` by your `MAIL_ADDRESS`)
- **IMAP password:** `pass` (change `pass` by your `MAIL_PASS`)

- **POP3 server:** `mail`
- **POP3 encryption:** `SSL` or `No`
- **POP3 port:** `995` or `110`
- **POP3 username:** `address@localdomain.test` (change `address@localdomain.test` by your `MAIL_ADDRESS`)
- **POP3 password:** `pass` (change `pass` by your `MAIL_PASS`)

- **SMTP server:** `mail`
- **SMTP encryption:** `No`
- **SMTP port:** `25`
- **SMTP authentication:** `none`
