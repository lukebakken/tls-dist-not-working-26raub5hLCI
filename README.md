# 3.8.3 Setup

Note: tested with Erlang `21.3`

## Base directory

```
/home/lbakken/issues/rabbitmq-users/tls-dist-not-working-26raub5hLCI
```

## "Long" host name / IP

```
$ ping shostakovich.localdomain
PING shostakovich.localdomain (192.168.1.5) 56(84) bytes of data.
64 bytes from shostakovich.localdomain (192.168.1.5): icmp_seq=1 ttl=64 time=0.049 ms
64 bytes from shostakovich.localdomain (192.168.1.5): icmp_seq=2 ttl=64 time=0.070 ms
^C
--- shostakovich.localdomain ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1008ms
rtt min/avg/max/mdev = 0.049/0.059/0.070/0.010 ms
```

## Certificates

```
cd "$HOME/development/michaelklishin"
git clone https://github.com/michaelklishin/tls-gen.git
cd tls-gen/basic
make CN=shostakovich.localdomain
```

## Setup 3.8.3 `generic-unix` package `etc` dir

```
cd "$HOME/issues/rmq-generic-unix/rabbitmq_server-3.8.3"

$ ln -vsf /home/lbakken/issues/rabbitmq-users/tls-dist-not-working-26raub5hLCI/rabbitmq.conf /home/lbakken/issues/rmq-generic-unix/rabbitmq_server-3.8.3/etc/rabbitmq/rabbitmq.conf
'/home/lbakken/issues/rmq-generic-unix/rabbitmq_server-3.8.3/etc/rabbitmq/rabbitmq.conf' -> '/home/lbakken/issues/rabbitmq-users/tls-dist-not-working-26raub5hLCI/rabbitmq.conf'

$ ln -vsf /home/lbakken/issues/rabbitmq-users/tls-dist-not-working-26raub5hLCI/rabbitmq-env.conf /home/lbakken/issues/rmq-generic-unix/rabbitmq_server-3.8.3/etc/rabbitmq/rabbitmq-env.conf
'/home/lbakken/issues/rmq-generic-unix/rabbitmq_server-3.8.3/etc/rabbitmq/rabbitmq-env.conf' -> '/home/lbakken/issues/rabbitmq-users/tls-dist-not-working-26raub5hLCI/rabbitmq-env.conf'
```

## Starting RabbitMQ

```
cd "$HOME/issues/rmq-generic-unix/rabbitmq_server-3.8.3"
RABBITMQ_ALLOW_INPUT=true ./sbin/rabbitmq-server
```

## Testing TLS

The following works correctly:

```
openssl s_client -connect localhost:5671 -CAfile ./certs/ca_certificate.pem -cert ./certs/client_certificate.pem -key ./certs/client_key.pem -servername shostakovich.localdomain

cd "$HOME/issues/rmq-generic-unix/rabbitmq_server-3.8.3"
./sbin/rabbitmqctl status
```

Correct result for the following command. Note that port `25672` is being tested, which is the TLS-encrypted distributed Erlang port:

```
openssl s_client -connect localhost:25672 -CAfile ./certs/ca_certificate.pem -cert ./certs/client_certificate.pem -key ./certs/client_key.pem -servername shostakovich.localdomain
```

# 3.8.5 Setup

Note: tested with Erlang `21.3` and `23.0.2`

## Setup 3.8.5 `generic-unix` package `etc` dir

```
cd "$HOME/issues/rmq-generic-unix/rabbitmq_server-3.8.5"

$ ln -vsf /home/lbakken/issues/rabbitmq-users/tls-dist-not-working-26raub5hLCI/rabbitmq-env.conf /home/lbakken/issues/rmq-generic-unix/rabbitmq_server-3.8.5/etc/rabbitmq/rabbitmq-env.conf
'/home/lbakken/issues/rmq-generic-unix/rabbitmq_server-3.8.5/etc/rabbitmq/rabbitmq-env.conf' -> '/home/lbakken/issues/rabbitmq-users/tls-dist-not-working-26raub5hLCI/rabbitmq-env.conf'

$ ln -vsf /home/lbakken/issues/rabbitmq-users/tls-dist-not-working-26raub5hLCI/rabbitmq.conf /home/lbakken/issues/rmq-generic-unix/rabbitmq_server-3.8.5/etc/rabbitmq/rabbitmq.conf
'/home/lbakken/issues/rmq-generic-unix/rabbitmq_server-3.8.5/etc/rabbitmq/rabbitmq.conf' -> '/home/lbakken/issues/rabbitmq-users/tls-dist-not-working-26raub5hLCI/rabbitmq.conf'
```

## Testing TLS

```
openssl s_client -connect localhost:5671 -CAfile ./certs/ca_certificate.pem -cert ./certs/client_certificate.pem -key ./certs/client_key.pem -servername shostakovich.localdomain

depth=1 CN = TLSGenSelfSignedtRootCA, L = $$$$
verify return:1
depth=0 CN = shostakovich.localdomain, O = server
verify return:1
CONNECTED(00000003)
---
Certificate chain
 0 s:CN = shostakovich.localdomain, O = server
   i:CN = TLSGenSelfSignedtRootCA, L = $$$$
 1 s:CN = TLSGenSelfSignedtRootCA, L = $$$$
   i:CN = TLSGenSelfSignedtRootCA, L = $$$$
---
Server certificate
-----BEGIN CERTIFICATE-----
-----END CERTIFICATE-----
subject=CN = shostakovich.localdomain, O = server

issuer=CN = TLSGenSelfSignedtRootCA, L = $$$$

---
Acceptable client certificate CA names
CN = TLSGenSelfSignedtRootCA, L = $$$$
Client Certificate Types: ECDSA sign, RSA sign, DSA sign
Requested Signature Algorithms: ECDSA+SHA512:RSA+SHA512:ECDSA+SHA384:RSA+SHA384:ECDSA+SHA256:RSA+SHA256:ECDSA+SHA224:RSA+SHA224:ECDSA+SHA1:RSA+SHA1:DSA+SHA1
Shared Requested Signature Algorithms: ECDSA+SHA512:RSA+SHA512:ECDSA+SHA384:RSA+SHA384:ECDSA+SHA256:RSA+SHA256:ECDSA+SHA224:RSA+SHA224:ECDSA+SHA1:RSA+SHA1:DSA+SHA1
Peer signing digest: SHA256
Peer signature type: RSA
Server Temp Key: ECDH, P-256, 256 bits
---
SSL handshake has read 2367 bytes and written 2440 bytes
Verification: OK
---
New, TLSv1.2, Cipher is ECDHE-RSA-AES256-GCM-SHA384
Server public key is 2048 bit
Secure Renegotiation IS supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
SSL-Session:
    Protocol  : TLSv1.2
    Cipher    : ECDHE-RSA-AES256-GCM-SHA384
    Session-ID: 53542D672FA10587367026524B13338B574087F0EC63E99935362D0D116C23B0
    Session-ID-ctx: 
    Master-Key: C1FA36E19C62E4D0CBE9FB3D51B3E901293BBA96609F31A4ADE33D627CDB75335DB5B4BF383C3365C65A0138ADAD0A2F
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    Start Time: 1594227461
    Timeout   : 7200 (sec)
    Verify return code: 0 (ok)
    Extended master secret: no
---
```

Same result as above for the following command. Note that port `25672` is being tested, which is the TLS-encrypted distributed Erlang port:

```
openssl s_client -connect localhost:25672 -CAfile ./certs/ca_certificate.pem -cert ./certs/client_certificate.pem -key ./certs/client_key.pem -servername shostakovich.localdomain
```

```
cd "$HOME/issues/rmq-generic-unix/rabbitmq_server-3.8.3"
./sbin/rabbitmqctl status

Status of node rabbit@shostakovich.localdomain ...
[1mRuntime[0m

OS PID: 74732
OS: Linux
Uptime (seconds): 225
RabbitMQ version: 3.8.5
Node name: rabbit@shostakovich.localdomain
Erlang configuration: Erlang/OTP 21 [erts-10.3] [source] [64-bit] [smp:8:8] [ds:8:8:10] [async-threads:128] [hipe]
Erlang processes: 465 used, 1048576 limit
Scheduler run queue: 1
Cluster heartbeat timeout (net_ticktime): 60
```
