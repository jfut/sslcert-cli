# sslcert-cli

![Tag](https://img.shields.io/github/tag/jfut/sslcert-cli.svg)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)

`sslcert-cli` is a command line tool that creates SSL certificate files such as a private key, CSR, and CRT, and also supports generating mTLS private CA and client certificates.

## Usage

```bash
sslcert-cli 1.2.0

Create SSL certificate files such as a private key, CSR, and CRT, and also support generating mTLS private CA and client certificates.

Usage:
    sslcert-cli [-f] [-o OUTPUT_DIR] [-l KEY_BIT_LENGTH] [-p KEY_PASS_PHRASE] [-s SUBJECT] [-S] [-A SUBJECT_ALT_NAMES] [-D EXPIRE_DAYS] -n FQDN

    sslcert-cli -m mtls [-f] [-o OUTPUT_DIR] [-l KEY_BIT_LENGTH] [-p KEY_PASS_PHRASE] [-s SUBJECT] [-D EXPIRE_DAYS] -n CA_NAME
    sslcert-cli -m mtls -i [-f] [-o OUTPUT_DIR] [-l KEY_BIT_LENGTH] [-p KEY_PASS_PHRASE] [-s SUBJECT] [-R CA_DIR] [-k CA_KEY_FILE] [-r CA_CRT_FILE] [-P MTLS_CA_KEY_PASS_PHRASE] [-A SUBJECT_ALT_NAMES] [-D EXPIRE_DAYS] -n CLIENT_NAME

    sslcert-cli -c FQDN_CSR_FILE
    sslcert-cli -C FQDN_CRT_FILE

Default options:
    -o Output directory (default: FQDN/YYYY-MM-DD/)
    -l Key bit length (default: 2048)
    -D Expiration days of certificate file (default: 365)

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

    Create private key and CSR files in a specific output directory with overwrite enabled:
        sslcert-cli -o /path/to/output/ -n example.org -f

    Create private key, CSR and self-signed CRT files with an expiration date of 3650 days:
        sslcert-cli -n example.org \
          -s "/C=JP/ST=Tokyo/L=Shinjuku-ku/O=Example Corporation/OU=Example Group/CN=example.org/emailAddress=ssladmin@example.org" \
          -S -A "*.example.org,example.com" \
          -D 3650

    # mTLS

    Create mTLS private CA only:
        MTLS_CA_KEY_PASS_PHRASE=$(read -s -p "MTLS_CA_KEY_PASS_PHRASE: " MTLS_CA_KEY_PASS_PHRASE; echo ${MTLS_CA_KEY_PASS_PHRASE})
        sslcert-cli -m mtls -P "${MTLS_CA_KEY_PASS_PHRASE}" -n mtls.example.com \
          -s "/C=JP/ST=Tokyo/L=Shinjuku-ku/O=Example Corporation/OU=Example Group/CN=mtls.example.com" -D 36500

    Create mTLS client certificate by auto-detecting CA files from directory:
        MTLS_CA_KEY_PASS_PHRASE=$(read -s -p "MTLS_CA_KEY_PASS_PHRASE: " MTLS_CA_KEY_PASS_PHRASE; echo ${MTLS_CA_KEY_PASS_PHRASE})
        MTLS_CLIENT_KEY_PASS_PHRASE=$(read -s -p "MTLS_CLIENT_KEY_PASS_PHRASE: " MTLS_CLIENT_KEY_PASS_PHRASE; echo ${MTLS_CLIENT_KEY_PASS_PHRASE})
        sslcert-cli -m mtls -i -n client1.mtls.example.com \
          -R mtls.example.com/YYYY-MM-DD/ \
          -P "${MTLS_CA_KEY_PASS_PHRASE}" \
          -p "${MTLS_CLIENT_KEY_PASS_PHRASE}" \
          -D 36500
        # If both client and CA pairs exist, *-mtls-ca.key/crt is preferred automatically.
        # If -o is specified together with -R, the -o path is used as the output directory.

    Create mTLS client certificate with existing private CA:
        MTLS_CA_KEY_PASS_PHRASE=$(read -s -p "MTLS_CA_KEY_PASS_PHRASE: " MTLS_CA_KEY_PASS_PHRASE; echo ${MTLS_CA_KEY_PASS_PHRASE})
        MTLS_CLIENT_KEY_PASS_PHRASE=$(read -s -p "MTLS_CLIENT_KEY_PASS_PHRASE: " MTLS_CLIENT_KEY_PASS_PHRASE; echo ${MTLS_CLIENT_KEY_PASS_PHRASE})
        sslcert-cli -m mtls -i -n client1.example.com \
          -k mtls.example.com/YYYY-MM-DD/private-ca.key -r /path/to/private-ca.crt \
          -P "${MTLS_CA_KEY_PASS_PHRASE}" \
          -p "${MTLS_CLIENT_KEY_PASS_PHRASE}" \
          -D 36500

    # Check

    Check CSR file:
        sslcert-cli -c example.org.csr

    Check CRT file:
        sslcert-cli -C example.org.crt

Server configuration examples:
    Apache:
        SSLCertificateFile /path/to/example.org.crt
        SSLCertificateKeyFile /path/to/example.org.key
        SSLCertificateChainFile /path/to/example.org.ca.crt

    nginx:
        ssl_certificate /path/to/example.org.fullchain.crt;
        ssl_certificate_key /path/to/example.org.key;
```

## Release

1. Edit the `Draft` on the release page.
2. Update the new version `name` and `tag` on the edit page.
3. Check `Set as a pre-release` and press the `Publish release` button.
4. Wait for the build by GitHub Actions to finish.
    - If the build fails due to errors such as download errors of source files, execute `Re-run failed jobs`.
5. Once all release files are automatically uploaded, check `Set as the latest release` and press the `Update release` button.

## License

MIT

## Author

Jun Futagawa (jfut)
