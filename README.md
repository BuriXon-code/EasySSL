# EasySSL

**EasySSL** is a Bash script that easily generates private and public SSL certificates. It is useful for encrypting communication between two or more computers using tools like `socat`, `stunnel`, and others that support SSL/TLS.

---

## About

This script allows you to quickly generate private keys and self-signed certificates for SSL/TLS encryption. It's perfect for testing or setting up encrypted communication between devices or applications. The generated certificates can be used with tools like `socat`, `stunnel`, and others.

You can customize the size of the key, the certificate's validity period, and the common name (e.g., the domain name or host). The script can generate certificates in various formats, making it versatile for different use cases.

---

## Installation

To install and use **EasySSL**, follow these steps:

1. Clone the repository:
   ```
   git clone https://github.com/BuriXon-code/easySSL
   ```
   
2. Navigate to the cloned directory:
   ```
   cd easySSL
   ```

3. Make the script executable:
   ```
   chmod +x easyssl
   ```

4. Optionally, move the script to a directory included in your system's `PATH` for easier access:
   ```
   sudo mv easyssl /usr/bin/
   ```

5. If you're using **Termux**, you can also move it to the Termux `bin` directory:
   ```
   mv easyssl $PREFIX/bin/
   ```

---

## Usage

To use **EasySSL**, run the script with the appropriate options. Below is an explanation of the available options and how to use them.

### Options:

- `-h, --help`  
  Displays help information about the script and its options.

- `-v, --verbose`  
  Enables verbose output, showing more detailed information during the process.

- `-q, --quiet`  
  Suppresses verbose output, only showing essential information.

- `-b, --bits <bits>`  
  Specifies the number of bits for the generated RSA key (default: 2048 bits).

- `-ca, --common-name <common-name>`  
  Sets the common name (CN) of the certificate. This could be the hostname or IP address.

- `-d, --days <days>`  
  Specifies the number of days for which the certificate will be valid (default: 3653 days, or approximately 10 years).

- `-o, --output-directory <output-dir>`  
  Sets the output directory for the generated certificate files.

- `-f, --force`  
  Forces overwriting of existing certificate files without asking for confirmation.

- `--format <pem|crt>`  
  Specifies the format of the generated certificate. Options: `pem`, `crt`. Default format is `pem`.

### Example (generating):

1. To generate a basic certificate with default settings:
   ```
   easyssl server
   ```

2. To generate a certificate with a specific common name and validity period:
   ```
   easyssl -ca "example.com" -d 3650 client
   ```

3. To generate a certificate in `pem` format with verbose output:
   ```
   easyssl -v --format pem server
   ```

4. To force overwrite existing certificates:
   ```
   easyssl -f server
   ```

### Example (usage with socat):

1. The most basic example:

```
easyssl <name>
# [server side]
socat OPENSSL-LISTEN:<port>,cert=<name>.pem,cafile=<name>.crt STDIO
# [client side]
socat STDIO OPENSSL:localhost:<port>,cert=<name>.pem,cafile=<name>.crt
```

2. Example of a server/client with different certificates (<server-hostname> must be in the /etc/hosts file):

```
easyssl -ca <server-hostname> <server>
easyssl <client>
# [server side]
socat OPENSSL-LISTEN:<port>,cert=<server>.pem,cafile=<client>.crt STDIO
# [client side]
socat STDIO OPENSSL:<server-hostname>:<port>,cert=<client>.pem,cafile=<server>.crt
```

3. Example of a server/client with different certificates with tunneling (<server-hostname> must equal <tunnel-hostname> and be in the /etc/hosts file):

```
easyssl -ca <server-hostname> <server>
easyssl <client>
# [server side]
socat OPENSSL-LISTEN:<port>,cert=<server>.pem,cafile=<client>.crt STDIO
# [tunnel side]
socat TCP-LISTEN:<tunnel-port> TCP-CONNECT:<server-hostname>:<port
# [client side]
socat STDIO OPENSSL:<host>:<tunnel-port>,cert=<client>.pem,cafile=<server>.crt
```

---

## License

This script is released under the MIT License. See the [LICENSE](LICENSE) file for more details.

---

## Support

### Contact me:
For any issues, suggestions, or questions, reach out via:

- **Email:** support@burixon.com.pl
- **Email:** sensei@burixon.com.pl
- **Contact form:** [Click here](https://burixon.com.pl/contact.php)

### Support me:
If you find this script useful, consider supporting my work by making a donation:

[**DONATE HERE**](https://burixon.com.pl/donate/)
