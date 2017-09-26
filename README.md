# certcheck

Tools to check expiration and renew TLS certificates. This tool checks 
that the certificate presented by the specified hosts is valid and that 
it's expiration is at least 30 days away.

I recommend running `certcheck` from cron, like this:

```
23 */4 * * * /home/alice/certcheck/certcheck
```

You may need to set up your path and environment a bit, in which case a wrapper as `scripts/certcheck.cron-example` might help.

See `certcheck.conf.example` for an example of the config file which should be placed in `$HOME/.certcheck.conf`.

To check a certificate for expiration, set `check

```
[cert "example.com"]
	check = true
	renew-via = route53
	deploy-via = appengine
	extra-hosts = www.example.com
    aws-access-key-id = AKXXXX
    aws-secret-access-key = XXXX
    aws-region = us-east-1
	project = placeholder-site-166500
	account = alice@example.net
```

If `check` is true, then the script will complain to stdout if the expiration of the certificate is within
30 days.

If `renew-via` is set, then to renew the certificate a program called `certcheck-renew-$via` to renew the certificate. See `certcheck-renew-route53` for an example.

If `deploy-via` is set, then to deploy a newly issued certificate a program called `chertcheck-deploy-$via`
will be invoked.
