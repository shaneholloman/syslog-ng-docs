---
title: Configuring {{ site.product.short_name }} clients with mutual authentication
id: adm-tls-client-conf-mutual
description: >-
    This chapter describes how to configure TLS on {{ site.product.short_name }} clients with mutual authentication.
---

## Purpose

Complete the following steps on every {{ site.product.short_name }} client host. Examples
are provided using both the legacy BSD-syslog protocol (using the
network() driver) and the new IETF-syslog protocol standard (using the
syslog() driver):

## Steps

1. Create an X.509 certificate for the {{ site.product.short_name }} client.

2. Copy the certificate (for example, client_cert.pem) and the
    matching private key (for example, client.key) to the {{ site.product.short_name }}
    client host, for example, into the
    /opt/syslog-ng/etc/syslog-ng/cert.d directory. The certificate must
    be a valid X.509 certificate in PEM format. The key must be in PEM format.
    If you want to use a password-protected key, see Password-protected keys.

3. Copy the CA certificate (for example, cacert.pem) of the Certificate
    Authority that issued the certificate of the {{ site.product.short_name }} server
    (or the self-signed certificate of the {{ site.product.short_name }} server) to the
    {{ site.product.short_name }} client host, for example, into the
    /opt/syslog-ng/etc/syslog-ng/ca.d directory.

    If you wish to use the ca-dir() option, instead of the ca-file(), in the
    {{ site.product.short_name }} configuration file (step 4.) then
    - issue the following command on the certificate:\
    `openssl x509 -noout -hash -in cacert.pem`\
    The result is a hash (for example,
    6d2962a8), a series of alphanumeric characters based on the
    Distinguished Name of the certificate.
    - issue the following command to create a symbolic link to the
    certificate that uses the hash returned by the previous command and
    the **.0** suffix:\
    `ln -s cacert.pem 6d2962a8.0`

4. Add a destination statement to the {{ site.product.short_name }} configuration file that
    uses the tls( ca-dir(path_to_ca_directory) ) option and specify
    the directory using the CA certificate. The destination must use the
    network() or the syslog() destination driver, and the IP address and
    port parameters of the driver must point to the {{ site.product.short_name }} server.
    Include the client\'s certificate and private key in the tls()
    options.

    For the details of the available tls() options, see
    TLS options.

    Example: A destination statement using mutual authentication

    The following destination encrypts the log messages using TLS and
    sends them to the 1999/TCP port of the {{ site.product.short_name }} server having the
    10.1.2.3 IP address. The private key and the certificate file
    authenticating the client is also specified.

    ```config
    destination demo_tls_destination {
        network(
            "10.1.2.3" port(1999)
            transport("tls")
            tls(
                ca-dir("/opt/syslog-ng/etc/syslog-ng/ca.d")
                key-file("/opt/syslog-ng/etc/syslog-ng/key.d/client.key")
                cert-file("/opt/syslog-ng/etc/syslog-ng/cert.d/client_cert.pem")
            )
        );
    };

    destination demo_tls_syslog_destination {
        syslog(
            "10.1.2.3" port(1999)
            transport("tls")
            tls(
                ca-dir("/opt/syslog-ng/etc/syslog-ng/ca.d")
                key-file("/opt/syslog-ng/etc/syslog-ng/key.d/client.key")
                cert-file("/opt/syslog-ng/etc/syslog-ng/cert.d/client_cert.pem")
            )
        ); 
    };
    ```

5. Include the destination created in Step 2 in a log statement.

{% include doc/admin-guide/warnings/tls-cert.md %}
