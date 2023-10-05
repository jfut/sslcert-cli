# sslcert-compose

![Tag](https://img.shields.io/github/tag/jfut/sslcert-compose.svg)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)

sslcert-compose is a command line tool that create SSL certificate files such as a private key, CSR, and CRT.

## Usage

```bash
sslcert-compose 1.0.0

Create SSL certificate files such as a private key, CSR, and CRT.

Usage:
    sslcert-compose [-f] [-o OUTPUT_DIR] [-l KEY_BIT_LENGTH] [-p KEY_PASS_PHRASE] [-s SUBJECT] [-S] [-A SUBJECT_ALT_NAMES] [-D EXPIRE_DAYS] -n FQDN
    sslcert-compose -c FQDN_CSR_FILE
    sslcert-compose -c FQDN_CRT_FILE

Default options:
    -o Output directory (default: FQDN/YYYY-MM-DD/)
    -l Key bit length (default: 2048)
    -d Expiration date of self-signed certificate file (default: 365)

Examples:
    Create private key and CSR files:
        sslcert-compose -n example.org

    Create private key with pass phrase and CSR files:
        KEY_PASS_PHRASE=$(read -s -p "KEY_PASS_PHRASE: " KEY_PASS_PHRASE; echo ${KEY_PASS_PHRASE})
        sslcert-compose -p "${KEY_PASS_PHRASE}" -n example.org

    Create private key and CSR files with a specified request subject:
        sslcert-compose -n example.org \
          -s "/C=JP/ST=Tokyo/L=Shinjuku-ku/O=Example Corporation/OU=Example Group/CN=example.org/emailAddress=ssladmin@example.org"

    Create private key and CSR files in a specific output directory:
        sslcert-compose -o /path/to/output/ -n example.org

    Create private key and CSR files in a specific output directory with overwrite:
        sslcert-compose -o /path/to/output/ -n example.org -f

    Create private key, CSR and self-signed CRT files with an expiration date of 3650 days:
        sslcert-compose -n example.org
          -S -A "*.example.org,example.com" \
          -D 3650 \
          -s "/C=JP/ST=Tokyo/L=Shinjuku-ku/O=Example Corporation/OU=Example Group/CN=example.org/emailAddress=ssladmin@example.org"

    Check CSR file:
        sslcert-compose -c example.org.csr

    Check CRT file:
        sslcert-compose -c example.org.crt

Server configuration examples:
    Apache:
        SSLCertificateFile /path/to/example.org.crt
        SSLCertificateKeyFile /path/to/example.org.key
        SSLCertificateChainFile /path/to/example.org.ca.crt

    nginx:
        ssl_certificate /path/to/example.org.crt;
        ssl_certificate_key /path/to/example.org.key;
```

## Release tag

e.g.:

```bash
git tag -a v1.0.0 -m "v1.0.0"
git push origin refs/tags/v1.0.0
```

## License

MIT

## Author

Jun Futagawa (jfut)
