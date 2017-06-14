 This project is used for generating [LetsEncrypt](https://letsencrypt.org/) SSL certificates and importing them into [Amazon's Certificate Manager](https://aws.amazon.com/certificate-manager/). This project is based on [letsencrypt-aws](https://github.com/alex/letsencrypt-aws) by Alex Gaynor.  

## Configuration
Configuration is read in via the `LETSENCRYPT_AWS_CONFIG`
environment variable. This should be a JSON object with the following schema:

```json
{
    "domains": [
        {
            "certificate_arn": "AWS ACM ARN (string)",
            "hosts": ["list of hosts you want on the certificate (strings)"],
            "key_type": "rsa or ecdsa, optional, defaults to rsa (string)"
        }
    ],
    "acme_account_key": "location of the account private key (string)",
    "acme_directory_url": "optional, defaults to Let's Encrypt production (string)"
}
```

The `acme_account_key` should be in S3 and is configured in the format `"s3://bucket-name/object-name"`. The key should be a PEM formatted RSA private key.

## Using Docker

**Basic usage with AWS_PROFILE set**

```
docker run \
  --rm \
  -e AWS_PROFILE=$AWS_PROFILE \
  -e LETSENCRYPT_AWS_CONFIG="$(cat $(pwd)/config.json)" \
  -v ${HOME}/.aws:/root/.aws \
  bartlettc/letsencrypt-acm \
  update-certificates
```

### Saving certificates as file

If you want the cert saved as a file as well, simply mount the `/certs` directory. i.e. `-v $(pwd)/certs:/certs`.  For example:

```
docker run \
  --rm \
  -e AWS_PROFILE=$AWS_PROFILE \
  -e LETSENCRYPT_AWS_CONFIG="$(cat $(pwd)/config.json)" \
  -v ${HOME}/.aws:/root/.aws \
  -v $(pwd)/certs:/certs \
  bartlettc/letsencrypt-acm \
  update-certificates
```
