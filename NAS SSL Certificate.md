## Create CERT from PEM
1. Download OpenSSL software.
2. Run OpenSSL console
3. Use command: `openssl x509 -outform der -in arthaqs-synology-me.pem -out your-cert.crt`

## Skip Certificate issue when accessing NAS
1. Open hosts `c:\Windows\System32\drivers\etc\hosts`
2. Add `192.168.0.12 arthaqs.synology.me`
3. Enjoy!