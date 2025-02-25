# Setting up the Solidworks PDM Web 2

These are the major components required when installing and operating PDM Web to:

* Static IP address OR DynDNS subscription for dynamic IP address resolution
* TLS Certificate
* Web2 API server
* Web2 PDM server (optional)

### Static IP or DynDNS

A static IP address means that an address for a machine (your PDM server) stays the same. If you don't own a static IP address (preferable), then a DynDNS update client is preferred. Documentation on DynDNS clients can be found here [https://help.dyn.com/update-client-faqs/](https://help.dyn.com/update-client-faqs/)

A domain name points to a static ip address (your pdm server). A domain name is not strictly necessary but can be more convenient to use than an ip address. A domain name is something that your company would use on the internet to host your own website at e.g.

> https://yourcompany.com.

### TLS Certificate

A TLS certificate is something that you use to encrypt the traffic between `https://yourcompany.com` and `https://app.sharpsync.net`. A TLS certificate is used to change the _type_ of traffic from `http` => `https` A TLS certificate may be obtained from a certificate authority such as DigiCert or GoDaddy

> [https://www.digicert.com/tls-ssl/compare-single-domain-certificates](https://www.digicert.com/tls-ssl/compare-single-domain-certificates)

OR

> [https://www.godaddy.com/en-ca/web-security/ssl-certificate](https://www.godaddy.com/en-ca/web-security/ssl-certificate)

For the more adventurous amongst you there are free TLS certificates available from Let's encrypt, there processes may be reviewed here

### SW PDM Web2 API Server

#### Installing the Web2 API server

* [x] Install the Web2 API server according to the installation instructions
* [ ] Host the web API server in IIS, then add the API to the List of WebAPI servers in the Administration tool.
*
*
[ ] 
    <figure><img src="../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Expose the API to the public internet. The API must be exposed and visible (using CURL login, see below) from computers outside your network
{% endhint %}

* [x] Take note of the port number that is visible in the IIS configuration options (or if being forwarded from a reverse proxy, then take note of that port)
* [ ] Test the connection from an _external machine (e.g. your phone or another computer outside your network)_ by connecting to the API using the following curl request&#x20;

```bash
curl -L 'https://{yourdomainOrStaticIpAndPort}/api/{vaultName}/authenticate' \
-d '{
  "username": "{registeredVaultUser}",
  "password": "{registeredVaultUserPassword}"
}'
```

This must return a 200 OK Response with a token

{% hint style="danger" %}
WARNING! DO NOT, under any circumstances, leave the default username of 'Admin' and password of 'Admin' enabled. This is a serious security vulnerability
{% endhint %}



* [x] In SharpSync,  copy the URL of the Web2 API and enter it in the configuration options for SWPDM
*
[ ] 
    <figure><img src="../../.gitbook/assets/swpdm_module_auth_port_config.png" alt=""><figcaption></figcaption></figure>

###

### Test the connection



### Installing the Web2 PDM Server (Optional)

The web 2 server allows an organization to navigate and view their files over the internet. This component is optional, but allows for navigation links to be generated in SharpSync for easy navigation to files in the vault.

\[If you need more documentation or assistance, please engage us for more information]
