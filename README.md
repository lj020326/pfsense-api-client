# pfsense-api-client

Provides python methods to call the pfsense API endpoint provided by the package at https://github.com/jaredhendrickson13/pfsense-api

The pfSense REST API package is an unofficial, open-source REST and GraphQL API for pfSense CE and pfSense Plus
firewalls. It is designed to be light-weight, fast, and easy to use. This guide will help you get started with the REST
API package and provide you with the information you need to configure and use the package effectively.

## Key Features

- 200+ REST endpoints available for managing your firewall and associated services
- A GraphQL API for flexible data retrieval and mutation
- Easy to use querying and filtering
- Configurable security settings
- Supports HATEOAS driven development
- Customizable authentication options
- Built-in Swagger documentation

## Getting Started

- [Installation and Configuration](https://pfrest.org/INSTALL_AND_CONFIG/)
- [Authentication and Authorization](https://pfrest.org/AUTHENTICATION_AND_AUTHORIZATION/)
- [Swagger and OpenAPI](https://pfrest.org/SWAGGER_AND_OPENAPI/)
- [Queries, Filters, and Sorting](https://pfrest.org/QUERIES_FILTERS_AND_SORTING/)

## Quickstart

For new users, it is recommended to refer to the links in the [Getting Started section](#getting-started) to begin. Otherwise, the installation
commands are included below for quick reference.

Install on pfSense CE:

```bash
pkg-static add https://github.com/jaredhendrickson13/pfsense-api/releases/latest/download/pfSense-2.8.0-pkg-RESTAPI.pkg
```

Install on pfSense Plus:

```bash
pkg-static -C /dev/null add https://github.com/jaredhendrickson13/pfsense-api/releases/latest/download/pfSense-24.11-pkg-RESTAPI.pkg
```

> [!WARNING]
> Before installing the package, always ensure your pfSense version is supported! Supported versions are listed [here](https://pfrest.org/INSTALL_AND_CONFIG/#supported-pfsense-versions).
> Installation of the package on unsupported versions of pfSense may result in unexpected behavior and/or system instability.

> [!TIP]
> You may need to customize the installation command to reference the package built for your pfSense version. Check
> the [releases page](https://github.com/jaredhendrickson13/pfsense-api/releases) to find the package built for
> your version of pfSense.


### Configuring authentication

Configure it one of two ways 

1. Pass the `config_filename` which is a JSON file with username/password/host (and optionally port).
2. Directly pass username/password/hostname (and optionally port).

At the moment it only supports the "Local Database" authentication method of username/password, but it should be relatively trivial to add JWT/API Token support at a later time.

(If you're going to do this, modify init to have a "mode" setting, which `call()` checks, then modifies things as needed.)

### Example JSON file

```json
{
    "username" : "me",
    "password" : "mysupersecretpassword",
    "hostname" : "example.com",
    "port" : 8443
}
```

## Ignoring Certificate validation

You can specify a `requests_session` option on construction, which one could configure to ignore verification.

```python
import requests

from pfsense_api_client import PFSenseAPIClient

session = requests.Session()
session.verify = False
api = PFSenseAPIClient(
        config_filename="~/.config/pfsense.json",
        requests_session=session,
        )
```