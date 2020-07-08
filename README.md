# 3.8.3 Setup

## Base directory

```
/home/lbakken/issues/rabbitmq-users/tls-dist-not-working-26raub5hLCI
```

## `generic-unix`

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

```
openssl s_client -connect localhost:5671 -CAfile ./certs/ca_certificate.pem -cert ./certs/client_certificate.pem -key ./certs/client_key.pem -servername shostakovich.localdomain
```

# 3.8.5 Setup

```
cd "$HOME/issues/rmq-generic-unix/rabbitmq_server-3.8.5"

$ ln -vsf /home/lbakken/issues/rabbitmq-users/tls-dist-not-working-26raub5hLCI/rabbitmq-env.conf /home/lbakken/issues/rmq-generic-unix/rabbitmq_server-3.8.5/etc/rabbitmq/rabbitmq-env.conf
'/home/lbakken/issues/rmq-generic-unix/rabbitmq_server-3.8.5/etc/rabbitmq/rabbitmq-env.conf' -> '/home/lbakken/issues/rabbitmq-users/tls-dist-not-working-26raub5hLCI/rabbitmq-env.conf'

$ ln -vsf /home/lbakken/issues/rabbitmq-users/tls-dist-not-working-26raub5hLCI/rabbitmq.conf /home/lbakken/issues/rmq-generic-unix/rabbitmq_server-3.8.5/etc/rabbitmq/rabbitmq.conf
'/home/lbakken/issues/rmq-generic-unix/rabbitmq_server-3.8.5/etc/rabbitmq/rabbitmq.conf' -> '/home/lbakken/issues/rabbitmq-users/tls-dist-not-working-26raub5hLCI/rabbitmq.conf'
```
