# Docker Registry

After reading, we can learn that how to use [Caddy](https://caddyserver.com/) with ssl to proxy docker registry web and docker registry.

## Prequisite

* installed docker
* installed docker-compose
* one public ip and domain

## Authentication

Two kinds of HTTPs authentication methods are described in this article.

### Authentication with self-signed certificate

Generate private key and certificate (also can use public certificate)

```sh
openssl req -new -newkey rsa:4096 -days 3650 -nodes -x509 -keyout certs/private.key -out certs/public.crt
```

Entering your domain to `Common Name` in the certificate.

Modify docker-compose.yml, uncomment out `REGISTRY_TRUST_ANY_SSL` of `environment` in registry-web

```sh
# ...
registry-web:
  # ...
  # If using self-signed cert, must use this
  environment:
    - REGISTRY_TRUST_ANY_SSL=true
# ...
```

and uncomment out the Caddy service in docker-compose.yml.

### Authentication with let's encrypt by caddy (Recommended)

In file `confg/caddy/Caddyfile`, change `private.registry.domain.com` to `your-domain` and `your-mail` to `your-mail`.

## Settings

Change `example.com` to `your-domain` following config

* conf/registry/config.yml
* conf/registry-web/config.yml

_Note: You can change the issuers of `conf/registry/config.yml` and `conf/registry-web/config.yml`_

## Run

```sh
docker-compose up -d
```

## Login

Use browser entering `https://your-domain/`, input `admin/admin`, and change password.

Create user accounts to allow pull and push. (giving UI_USER and write_all)

### Role

In docker-register-web, have following roles:

| Role      | Description                                            |
|-----------|--------------------------------------------------------|
| UI_ADMIN  | administrator, can manager users but not push and pull |
| UI_USER   | user, can lool event and status, but not push and pull |
| UI_DELETE | can delete image in web                                |
| read-all  | only grants pull                                       |
| write-all | grant pull and push                                    |

## Push and Pull

If using self-signed certificate, we need copy cert of registry to each docker host `/etc/docker/certs.d/example.com/ca.crt`.

For example, docker registry at `example.com`

    copy the cert of registry to the /etc/docker/certs.d/example.com/ca.crt on each docker host

## More

More infomation about docker-register-web can follow [hyper/docker-registry-web](https://hub.docker.com/r/hyper/docker-registry-web/)
