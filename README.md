# Tmt and TF Commands

[[_TOC_]]

## tmt try

```
tmt try rhel-8.10@minute
tmt try -l rhel-8.10@minute
```

for logging in VM before test execution (like -n in 1mt)

```
tmt try -p /plans/basic rhel-8.10@minute
```


## tmt run

### run preparation from plan before test execution

```
tmt run -vvv --all execute --how tmt -i provision --how minute -i <1MT_IMAGE> tests --name .
```

It requires `tmt-redhat-provision-minute`

### Login after test execution

```
tmt -vvv run --all plans --name basic execute --how tmt -i provision -h minute -i 1MT-RHEL-7.9-updates-20230531.0 tests --name . login
```

### Execute test in 1minutetip on existing machine

```
tmt run -vvv --all provision --how connect --guest=10.0.186.112 --user=root --password=root --become tests --name .

# or run the whole plan

tmt run -vvv --all plans --name <test_plan_name> provision --how connect --guest=10.5.228.70 --user=root --password=root --become tests
```

### Running virtual machine

```
tmt run provision -h virtual --image ~/Downloads/rhel-guest-image-10.2.x86_64.qcow2 login
```

You can ssh to that machine when you go to tmt path (e.g. `/var/tmp/tmt/run-029`) and then list the
folder `/var/tmp/tmt/run-029/ssh-sockets/` and you will see the file `127.0.0.1-10033-root.socket`. The SSH private key
is stored in `/var/tmp/tmt/run-029/default/plan/provision/default-0/id_ecdsa`.

So to SSH to that machine just use:

```
ssh root@localhost -p 10033 -i /var/tmp/tmt/run-029/default/plan/provision/default-0/id_ecdsa
```

You can list running `libvirt/QEMU/KVM` machines with:

```
virsh list --all
```

and you can power off machine with:

```
virsh shutdown tmt-029-hSHvwlIr
```

### Running against localhost

Install requirements

```
yum config-manager --set-enabled rhel-CRB
yum config-manager --set-enabled epel
yum install -y tmt
```

And to run against localhost you can use

```
tmt -vvv run --all plans --name <test_plan_name> execute --how tmt -i provision -h local --feeling-safe tests --name . login

# or all test in test plan

tmt -vvv run --all plans --name <test_plan_name> execute  --how tmt -i provision -h local --feeling-safe tests
```

### Running tests in a different repo

We need to switch GitLab authentication URL.

```
git config --global url."git@gitlab.com:".insteadOf "https://gitlab.com/"

# and then

tmt -vvv run -a provision -h connect --guest=10.0.184.155 --user=root --password=root plan -n basic test -n /Security/CVE-2026-33416
```


## discover

Print tests in test plan.

```
tmt run discover -v plan --name <test_plan>
```


Inspect test including dynamic data (adjusts)

```
tmt -c distro=rhel-10.2 run --dry
```


## tmt tests

### Execute test on existing machine

Print tests and information in the whole repository

```
tmt tests show
```

Print tests and information based on filter condition - in this case test execution 15 minutes in fmf. Can be used for all fmf properties (enabled, contact, summary)

```
tmt tests show --filter duration:15m
```

## Lint

Linter for all files (few test to check metadata validity)
The second parameter (...TC…) is optional. If it is not set, then it lints all fmf files

```
tmt lint [/Sanity/Upstream-testsuite/TC#0604124]
```

### Generate test ID

```
tmt test id .
```

### Get tests in JSON format

```
tmt tests export --format json show . | jq
```

### Execute tmt command in different directory

```
tmt --root "${tmt_tests_root}/${component}" run -v discover plan --name "$plan"
````

tmt_tests_root is a path to tests repository
component is rhel-system-roles
plan to filter - e.g. basic

## Beakerlib integration

### Convert Beakerlib test

```
tmt tests import --no-nitrate
```

### Update TCMS metadata

```
tmt test export -h nitrate .
```


## fmf syntax

Limiting where the test is enabled

```
adjust:
  - enabled: false
    when: distro == rhel-6
    continue: false
  - enabled: false
    when: distro ~< rhel-7.4
    continue: false
```

## License
[MIT](LICENSE)

