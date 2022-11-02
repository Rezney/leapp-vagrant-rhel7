RHEL 7 Vagrant box for development and testing purposes
======================================================

## Getting Started

If you want to setup development environment:

```bash
git clone https://github.com/Rezney/leapp-vagrant-rhel7
cd leapp-vagrant-rhel7
vagrant up
vagrant ssh
```

Currently, the default source for getting RHEL content is RHSM (SRC=rhsm), for using this option you need to provide the following variables: "rhsm_account", "rhsm_password", and optionally 
"rhsm_pool_ids".

In case you want to use custom repositories as a source you can run:

```bash
SRC=custom vagrant up
```

In this case, you need to provide RHEL 7 repositories into the "rhel-source.repo" file and RHEL 8 repositories into the "leapp_upgrade_repositories.repo" file. On top of that, you also need to get the Leapp metadata as per "https://access.redhat.com/articles/3664871".

If you want to create the vagrant box using system session instead of user
session in libvirt, use the `SYSTEM_SESSION` envvar (doesn't matter what value
you set):

```bash
SYSTEM_SESSION=1 vagrant up
```

If you want to test a specific PR from "leapp-repository" repo you can run:

```bash
RPR=<your PR#> vagrant up
```

In case of testing Leapp framework PR you can run:

```bash
FPR=<your PR#> vagrant up
```

The above environment variables can be combined.

## Running tests

To run unit/component tests, run the following commands:
1. `cd ~/leapp-repository`
1. `make install-deps`
1. `make test` (for all tests) or `ACTOR=my-actor make test`

pls check the most recent docs here
<https://leapp.readthedocs.io/en/latest/unit-testing.html#running-the-tests>

