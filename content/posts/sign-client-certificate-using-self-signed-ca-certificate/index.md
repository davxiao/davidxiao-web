---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Sign Client Certificate Using Self Signed CA Certificate"
subtitle: "with openssl"
summary: "In a cluster setting where TLS mutual authentication is required, it's not uncommon to see client certificates signed by either self-signed root CA or private CA."
authors:
  - david-xiao
categories:
  - CyberSecurity
tags:
  - tls
  - digitalcertificate
  - x509
  - selfsign
date: 2020-10-10
lastmod: 2020-10-10
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

TL;DR

When TLS mutual authentication is put in place between controller node and work nodes in a cluster setting, it's not uncommon to see client certificates signed by either self-signed CA or private CA.

This post covers how to generate a self-signed root CA and to sign a client certificate using `openssl`. In cases when private CA is employed instead, the client certificate signing portion is still relevant.

{{% alert note %}}

The configuration described in this post is for testing only. It is NOT secure for production use.

{{% /alert %}}

## Self-signed CA Certificate

Before we start, let's create a root directory called `tls-cert`.

Under the root directory, we create a sub directory and some files:

```text
[tls-cert] mkdir ca
[tls-cert] mkdir ca/ca.db.certs
[tls-cert] touch ca/ca.db.index
[tls-cert] echo "123456" > ca/ca.db.serial
```

Next step is to generate a CA key: in this example it's 2048-bit RSA:

```text
[tls-cert] openssl genrsa -out ca/ca.key 2048
```

Next is to create a self-signed X509 certificate out of the CA key. The client keys will be signed with it later.

```text
[tls-cert] openssl req -new -x509 -days 365 -key ca/ca.key -out ca/ca.crt
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) []:.
State or Province Name (full name) []:.
Locality Name (eg, city) []:.
Organization Name (eg, company) []:ca-org-name
Organizational Unit Name (eg, section) []:ca-orgunit-name
Common Name (eg, fully qualified host name) []:ca-common-name
Email Address []:admin@ca.com
[tls-cert]
```

## Client certificate

Now that we've generated CA certificate, it's time to generate a private key for the client and create a CSR (Certificate Signing Request)for the key:

```text
[tls-cert] openssl req -new -newkey rsa:2048 -nodes -keyout client.key -out client-key-sign-req.pem
Generating a 2048 bit RSA private key
...................................................................................................+++
........+++
writing new private key to 'client.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) []:.
State or Province Name (full name) []:.
Locality Name (eg, city) []:.
Organization Name (eg, company) []:client-org-name
Organizational Unit Name (eg, section) []:client-orgunit-name
Common Name (eg, fully qualified host name) []:client-common-name
Email Address []:admin@client.com

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
[tls-cert]
```

openssl needs to know a few parameters in order to sign the CSR. Let's put all the parameters into a file and save it as `ca.conf`:

{{< gist davxiao ac12f7ebe4e37079ed279f3ce9bc85a9 >}}

With that sorted out, the last step is to sign the CSR file `client-key-sign-req.pem`:

```text
[tls-cert] openssl ca -config ca.conf -out client-certificate.pem.crt -infiles client-key-sign-req.pem
Using configuration from ca.conf
Check that the request matches the signature
Signature ok
The Subject's Distinguished Name is as follows
organizationName      :ASN.1 12:'client-org-name'
organizationalUnitName:ASN.1 12:'client-orgunit-name'
commonName            :ASN.1 12:'client-common-name'
emailAddress          :IA5STRING:'admin@client.com'
Certificate is to be certified until Oct 11 02:08:05 2021 GMT (365 days)
Sign the certificate? [y/n]:y
1 out of 1 certificate requests certified, commit? [y/n]y
Write out database with 1 new entries
Data Base Updated
[tls-cert]
```

## Conclusion

We've produced the following files in this example:

`ca/ca.key`: private key of the self-signed root CA. It must be kept secret at all times.

`ca/ca.crt`: CA certificate of the root CA. It should be made available to whichever party that needs to verify the certificates that were signed by the root CA.

`client.key`: private key of the client certificate. It must be kept secret except during the client authentication process.

`client-certificate.pem.crt`: CA certificate of the client. It should be made available to whichever party that needs to verify the identity of the client.
