## Decoding certificate data and key in kubeconfig file

The certificate data and key in kube config file is in base64 encoded format. To view the actual certificate contents in human readable format,

<pre><code>cat .kube/config</code></pre>

Copy out the base64 encoded certificate data and save in a text file as kubernetes-admin.pem

<pre><code>vagrant@master-node:~$ cat kubernetes-admin.pem
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURFekNDQWZ1Z0F3SUJBZ0lJSlpnVW1hL3FvVDR3RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB5TVRBMk1qQXdOek14TkRsYUZ3MHlNakEyTWpBd056TXhOVEphTURReApGekFWQmdOVkJBb1REbk41YzNSbGJUcHRZWE4wWlhKek1Sa3dGd1lEVlFRREV4QnJkV0psY201bGRHVnpMV0ZrCmJXbHVNSUlCSWpBTkJna3Foa2lHOXcwQkFRRUZBQU9DQVE4QU1JSUJDZ0tDQVFFQW9NS1ZIaE5jcXVsaTFrRXkKUlNKRWoyMU01clg3SnplSzJZVERyd3VNcEQvcEl0L0JvOS9aK1VjZ1Z6ZEUwbDVJZHlBTFhnY2EzcXdqT0NPZgpZK2hLbFRMUzN6L1J4Q2E3cVM4SFFadVNTeElGVFVlS2toWVRFWGFYUnJ3RVg0OEhrREx5c29oTzZFZHhUQXVjCjVLekVhRU9mbVEzLzBjQkg1dndwODduVTJoMFlyMWFEdFF6cGI4aThweWJsMC9sWlp2WGNtWVNQeDJSTDdPSmQKQytVY1B5TXBWQ0hUeFAxYzdzYUc3cmVhMGxUUnNuNmJyQ0ozL2NzZEI2UGRNRHFla1ZDUGovVHZFajVjYVdTUApQT1J0K2VhTFpnci9OaFZVYXBvQWE0Y3lWeWY0MFRuQVpaSm9KS2ZlQVh4UkU4bGFucXA0c1hHc0ZkeGRHRnZuClczT2ttd0lEQVFBQm8wZ3dSakFPQmdOVkhROEJBZjhFQkFNQ0JhQXdFd1lEVlIwbEJBd3dDZ1lJS3dZQkJRVUgKQXdJd0h3WURWUjBqQkJnd0ZvQVVrWVhiUlErNGxhV0trMklGaDJXYXdwTUlOemt3RFFZSktvWklodmNOQVFFTApCUUFEZ2dFQkFDQUNwZllTZzYvTk8xRFRyLzlyS0p0enFPL1NpZ3hPMzFobVRheFh1M1VvbUNYNEdFTURQd0NECkpIK0xYOTVqK05DQnNZbGh6RXcyc2hBVGc0a1ExSVNaYWpLVjNCK2doVFE1dTZuZ0p2dW9OdjJhT2FNd0ZaU2sKZ2pVVGtieHhaMVFnc1RxbjB6SElYZWJ5Q0h3TnZ0M0hCRFNwQzJyM2NVMG5UaWYwY2xwaVZvTXdwdDhOOW9DMQowM2tVNGQzaFpiOE1VaTRkUFZMclI4RmpaZkVOeGdzZjBVUW5lbU91T3JPdUhNWWZDMEx2cy9YdUFuQStVTzAxCld5bmZlcHB1Q3RPZWdDT0VwcDVvSHgzQlRpdFdLZkpoZnVSZzVWL3dhenRzbUtaTlAwam1MN2IxcVI5ZlRCZHAKWTZnb0xES25udnRFays5VURkaWIwSjVYa1lTSUlmZz0KLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=</code></pre>




<pre><code>vagrant@master-node:~$ cat kubernetes-admin.pem | base64 -d
-----BEGIN CERTIFICATE-----
MIIDEzCCAfugAwIBAgIIJZgUma/qoT4wDQYJKoZIhvcNAQELBQAwFTETMBEGA1UE
AxMKa3ViZXJuZXRlczAeFw0yMTA2MjAwNzMxNDlaFw0yMjA2MjAwNzMxNTJaMDQx
FzAVBgNVBAoTDnN5c3RlbTptYXN0ZXJzMRkwFwYDVQQDExBrdWJlcm5ldGVzLWFk
bWluMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAoMKVHhNcquli1kEy
RSJEj21M5rX7JzeK2YTDrwuMpD/pIt/Bo9/Z+UcgVzdE0l5IdyALXgca3qwjOCOf
Y+hKlTLS3z/RxCa7qS8HQZuSSxIFTUeKkhYTEXaXRrwEX48HkDLysohO6EdxTAuc
5KzEaEOfmQ3/0cBH5vwp87nU2h0Yr1aDtQzpb8i8pybl0/lZZvXcmYSPx2RL7OJd
C+UcPyMpVCHTxP1c7saG7rea0lTRsn6brCJ3/csdB6PdMDqekVCPj/TvEj5caWSP
PORt+eaLZgr/NhVUapoAa4cyVyf40TnAZZJoJKfeAXxRE8lanqp4sXGsFdxdGFvn
W3OkmwIDAQABo0gwRjAOBgNVHQ8BAf8EBAMCBaAwEwYDVR0lBAwwCgYIKwYBBQUH
AwIwHwYDVR0jBBgwFoAUkYXbRQ+4laWKk2IFh2WawpMINzkwDQYJKoZIhvcNAQEL
BQADggEBACACpfYSg6/NO1DTr/9rKJtzqO/SigxO31hmTaxXu3UomCX4GEMDPwCD
JH+LX95j+NCBsYlhzEw2shATg4kQ1ISZajKV3B+ghTQ5u6ngJvuoNv2aOaMwFZSk
gjUTkbxxZ1QgsTqn0zHIXebyCHwNvt3HBDSpC2r3cU0nTif0clpiVoMwpt8N9oC1
03kU4d3hZb8MUi4dPVLrR8FjZfENxgsf0UQnemOuOrOuHMYfC0Lvs/XuAnA+UO01
WynfeppuCtOegCOEpp5oHx3BTitWKfJhfuRg5V/waztsmKZNP0jmL7b1qR9fTBdp
Y6goLDKnnvtEk+9UDdib0J5XkYSIIfg=
-----END CERTIFICATE-----</code></pre>



Copy out the text above into another file kubernetes-admin.crt and then use openssl to decode it into certificate data that is human readable. The below output has been truncated for brevity.


<pre><code>
openssl x509 -in kubernetes-admin.crt -noout -text

Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number: 2708937826177294654 (0x25981499afeaa13e)
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: CN = kubernetes
        Validity
            Not Before: Jun 20 07:31:49 2021 GMT
            Not After : Jun 20 07:31:52 2022 GMT
        Subject: O = system:masters, CN = kubernetes-admin
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                RSA Public-Key: (2048 bit)
<<< output truncated >>>                
</code></pre>
                
