---
layout: post
title: How to secure your Apache2 webserver with SSL for free
date: 2013-06-20 19:02
comments: true
categories:
  - ssl
  - apache
  - certificate
  - command line
  - webserver
---

Protecting your website with TLS (SSL) is generaly a good idea, as soon as you have a admin backend or an internal statistik tool installed.

One way to do it is to sign your own SSL certificate. This offers encryption but unfortunately no verification that you really talk to the right server. Additionally all your guests will receive warnings and have to click through warnings. 

Thats why a [Ceritficate Authority (CA)][5] needs to sign your cerificate. [StartSSL][1] does this for free.

In this article I show you how you can get a SSL certificate and how you configure your Apache 2 webserver to use it.

## Prerequisites

In order for you to obtain a certificate you need a domain under your control. Which means you need one of the following email adresses and you need to be able to read these mails.

* postmaster@example.com
* hostmaster@example.com
* webmaster@example.com

You can install the certificate on any modern webserver. However in this article I describe how you install it with Apache2 and [ModSSL][6].

## Obtaining the certificate

### Register with [StartSSL][1]

Visit [StartSSL.com][1] and click on the Sign-Up button.

{% img /images/uploads/2013-06/Click-on-Sign-up-1.jpg %}

Fill in your personal details. Provide true informations here. [StartSSL][1] will check your data.

{% img /images/uploads/2013-06/Fill-out-the-registration-form-1.jpg %}

After having supplied your details, you will receive a verification code via email, that you have to fill in.

{% img /images/uploads/2013-06/Confirm-your-email-address.jpg %}

Now you will get a client certificate, that you can use to verify your identity to StartSSL in the future.

### Validate your domain name

Log into the Controll Panel and click onto the _Validations Wizard_ tab.

Choose _Domain Name Validation_.

{% img /images/uploads/2013-06/Choose-Domain-Name-Validation.jpg %}

Enter the name of your domain.

{% img /images/uploads/2013-06/Pick-your-domain.jpg %}

Now you have to select one of the email adresses I mentioned. [StartSSL][1] uses this adress to verify, that you own the domain.

{% img /images/uploads/2013-06/Pick-the-admin-email-address.jpg %}

Now you will remove a confirmation code via email, that you have to enter into the next form. 

### Creating a [Certificate Signin Request (CSR)][4]

Now you have established, that the domain belongs to you and StartSSL is ready to sign a _[Cerificate Signing Request][2]_ for you. So let's create a CSR!

Log into your webserver and do the following:

    $ openssl req -new -newkey rsa:2048 -nodes -keyout example.com.key -out example.com.csr

Enter the name of your domain, when you are asked for the common name. Here is a how the dialoge looks like:


```
$ openssl req -new -newkey rsa:2048 -nodes -keyout example.com.key -out example.com.csr

Generating a 2048 bit RSA private key
................+++
...................+++
writing new private key to 'example.com.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:DE
State or Province Name (full name) [Some-State]:Bavaria
Locality Name (eg, city) []:Munich
Organization Name (eg, company) [Internet Widgits Pty Ltd]:
Organizational Unit Name (eg, section) []:
Common Name (eg, YOUR name) []:example.com
Email Address []:robert@example.com

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```

Now copy the content of the CSR into your clipboard

    cat example.com.csr

### Let StartSSL sign your CSR

Visit the "Certificates Wizard" tab and choose Webserver SSL/TLS Certificate and click on continue.

Click on skip and paste the contents of the csr file you just created into the text field.

On the next step add www. as the subdomain (or another subdomain if you do not have www).

Now save your certificate. If your domain is example.com I would save it as example.com.crt

## Configure Apache2 to use the certificate

Become root or execute the following stuff with __sudo__

First we have to activate [ModSSL][6]

	a2enmod ssl

Create a directory for your certificates

    mkdir /etc/apache2/ssl

Move the .crt and the .key into the new folder

    mv example.com.* /etc/apache2/ssl

We also need a ca.pem file. This file contains both the intermediate and the root certificate. You can download a finished one.

    curl -L https://mug.im/x/5s > ca.pem
 
Now configure the Virtual Host for SSL

    vim /etc/apache2/sites-enabled/example.com.ssl

```
<VirtualHost *:443>
DocumentRoot /var/www/vhosts/example.com/
ServerName example.com
ServerAlias www.example.com

SSLEngine on
SSLCertificateFile /etc/apache2/ssl/example.com.crt
SSLCertificateKeyFile /etc/apache2/ssl/example.com.key
SSLCertificateChainFile /etc/apache2/ssl/ca.pem
</VirtualHost>
```

Test your Apache config before restarting

    apachectl configtest

Now restart your webserver

    sudo /etc/init.d/apache2 restart

You're website can be viewed with SSL now.

## Final words

The process at [StartSSL][1] is anti-intuitive. I hope you were able to follow this tutorial and that your website is SSL secured now.

Depending on your use case, there are more steps that you have to take. For example you should load all assets from HTTPS or your users will get mixed content warnings. 

Another good idea is to redirect your visitors to HTTPS and to activate [Strict Transport Security][3].

If you have user other then yourself at the website, you should also follow SSL best pratices. [Here is a tool][7] that you can use to find out, what you can improve.

I leave all this as an excercise for the reader. Please leave me a comment if you have any questions or corrections.

[1]: https://www.startssl.com/
[2]: http://en.wikipedia.org/wiki/Certificate_signing_request
[3]: http://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security
[4]: http://en.wikipedia.org/wiki/Certificate_signing_request
[5]: http://en.wikipedia.org/wiki/Certificate_authority
[6]: http://www.modssl.org/
[7]: https://www.ssllabs.com/
