 Based on [letsencrypt-aws](https://github.com/alex/letsencrypt-aws) by Alex Gaynor


 ## Using Docker

*With AWS_PROFILE set*

```
 docker run \
   --rm \
   -e AWS_PROFILE=$AWS_PROFILE \
   -e LETSENCRYPT_AWS_CONFIG="$LETSENCRYPT_AWS_CONFIG" \
   -v ${HOME}/.aws:/root/.aws \
   -v $(pwd)/config.json:/letsencrypt_aws_config.json
   -v $(pwd)/acme_account_key.pem:/acme_account_key.pem
   -v $(pwd)/certs:/certs \
   bartlettc/letsencrypt-acm \
   update-certificates
```

If you want the cert saved as a file as well, simply mount the `/certs` directory: `-v $(pwd)/certs:/certs`
