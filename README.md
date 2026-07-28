# OpenLDAP Development Stack

A containerised [OpenLDAP](https://www.openldap.org/) directory server with a
phpLDAPadmin web UI, pre-seeded with sample users. Useful as a local directory
backend when developing or testing LDAP authentication.

## Stack

| Service | Image | Purpose |
| :--- | :--- | :--- |
| `openldap` | `osixia/openldap:1.5.0` | LDAP directory server |
| `phpldapadmin` | `osixia/phpldapadmin:latest` | Web UI for browsing the directory |

## Requirements

- Docker
- Docker Compose

## Getting started

Bring the stack up:

```bash
docker compose up -d
```

Load the sample directory entries:

```bash
docker exec -i openldap ldapadd -x -D "cn=admin,dc=example,dc=org" -w admin < init_users.ldif
```

Then open **http://localhost:8080** and log in with:

- **Login DN:** `cn=admin,dc=example,dc=org`
- **Password:** `admin`

## Connection details

| Setting | Value |
| :--- | :--- |
| Host | `localhost` |
| Port | `389` (LDAP) / `636` (LDAPS) |
| Base DN | `dc=example,dc=org` |
| Admin DN | `cn=admin,dc=example,dc=org` |

## Sample users

`init_users.ldif` seeds eight `inetOrgPerson` entries, all with the password
`password`:

`riemann`, `gauss`, `euler`, `euclid`, `einstein`, `newton`, `galileo`, `tesla`

Bind as any of them, for example:

```bash
ldapsearch -x -H ldap://localhost:389 \
  -D "uid=einstein,dc=example,dc=org" -w password \
  -b "dc=example,dc=org" "(uid=einstein)"
```

## Shutting down

```bash
docker compose down        # stop the containers
docker compose down -v     # stop and delete the directory data
```

> **Note**
> The credentials in this repository are intentionally weak sample values for
> local development. Do not reuse this configuration in a deployed environment.
