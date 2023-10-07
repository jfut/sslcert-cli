# sslcert-cli

![Tag](https://img.shields.io/github/tag/jfut/sslcert-cli.svg)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)

sslcert-cli is a command line tool that create SSL certificate files such as a private key, CSR, and CRT.

## Usage

```bash
sslcert-cli 1.1.0

Create SSL certificate files such as a private key, CSR, and CRT.

Usage:
    sslcert-cli [-f] [-o OUTPUT_DIR] [-l KEY_BIT_LENGTH] [-p KEY_PASS_PHRASE] [-s SUBJECT] [-S] [-A SUBJECT_ALT_NAMES] [-D EXPIRE_DAYS] -n FQDN
    sslcert-cli -c FQDN_CSR_FILE
    sslcert-cli -c FQDN_CRT_FILE

Default options:
    -o Output directory (default: FQDN/YYYY-MM-DD/)
    -l Key bit length (default: 2048)
    -d Expiration date of self-signed certificate file (default: 365)

Examples:
    Create private key and CSR files:
        sslcert-cli -n example.org

    Create private key with pass phrase and CSR files:
        KEY_PASS_PHRASE=$(read -s -p "KEY_PASS_PHRASE: " KEY_PASS_PHRASE; echo ${KEY_PASS_PHRASE})
        sslcert-cli -p "${KEY_PASS_PHRASE}" -n example.org

    Create private key and CSR files with a specified request subject:
        sslcert-cli -n example.org \
          -s "/C=JP/ST=Tokyo/L=Shinjuku-ku/O=Example Corporation/OU=Example Group/CN=example.org/emailAddress=ssladmin@example.org"

    Create private key and CSR files in a specific output directory:
        sslcert-cli -o /path/to/output/ -n example.org

    Create private key and CSR files in a specific output directory with overwrite:
        sslcert-cli -o /path/to/output/ -n example.org -f

    Create private key, CSR and self-signed CRT files with an expiration date of 3650 days:
        sslcert-cli -n example.org
          -s "/C=JP/ST=Tokyo/L=Shinjuku-ku/O=Example Corporation/OU=Example Group/CN=example.org/emailAddress=ssladmin@example.org" \
          -S -A "*.example.org,example.com" \
          -D 3650

    Check CSR file:
        sslcert-cli -c example.org.csr

    Check CRT file:
        sslcert-cli -c example.org.crt

Server configuration examples:
    Apache:
        SSLCertificateFile /path/to/example.org.crt
        SSLCertificateKeyFile /path/to/example.org.key
        SSLCertificateChainFile /path/to/example.org.ca.crt

    nginx:
        ssl_certificate /path/to/example.org.crt;
        ssl_certificate_key /path/to/example.org.key;
```

## License

MIT

## Author

Jun Futagawa (jfut)
