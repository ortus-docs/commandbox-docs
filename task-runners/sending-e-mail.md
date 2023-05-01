# Sending E-mail

To send email, use `cfmail` tag making sure you set `asyc=false` (see below). Not setting this flag to `false` may result in undelivered email because mail may still exist in Lucee spooler (Lucee tasks) when your task runner exits.

```javascript
mail 
    subject=subject 
    from=from 
    to=to
    cc=cc
    bcc=bcc
    replyTo=replyTo
    type=type
    server=server
    port=port
    username=username
    password=password
    useTls=tls
    usessl=ssl
    async=false {
writeOutput(emailBody);
};
```

### Connecting to SMTP Services Using SSL or TLS

If you cannot connect to a SMTP server that requires `SSL` or `TLS`, like [Amazon SES](https://aws.amazon.com/ses/), one workaround is to install a local SMTP server and configure it as a relay to your SMTP server. This has been done successfully on Windows servers using [hMailServer](https://www.hmailserver.com/) (free, opensource), which is fairly easy to install and configure as an SMTP relay.
