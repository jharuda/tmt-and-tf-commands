# tf_commands.md

## Common commands

### Show composes

```
testing-farm composes
```

## Examples

### Run single test

```
testing-farm request --git-url  https://pkgs.devel.redhat.com/git/tests/net-snmp \
                     --compose RHEL-10-Beta-Nightly \
                     --plan "basic$" --git-ref master \
                     --arch ppc64le \
                     --redhat-brew-build=63220862 \
                     --context='distro=rhel-10' \
                     --test "/Regression/bz848155-package-net-snmp-5-5-41-el6-3-1-x86-64-always-fail"
```

### Run test with filter

```
testing-farm request --git-url https://github.com/RedHat-SP-Security/keylime-tests.git \
                     --plan-filter 'name: .*upstream-keylime-swtpm-dev' \
                     --test-filter 'name: /setup.*|name: /functional.*' \
                     --compose RHEL-9-Nightly \
                     --arch x86_64 \
                     --context arch=x86_64
```

### Run the whole plan

```
testing-farm request --git-url https://gitlab.cee.redhat.com/toolchain-qe/tests/pcp-container.git \
                     --git-ref master \
                     --arch x86_64,s390x,ppc64le,aarch64 \
                     --compose RHEL-9-Nightly --environment IMAGE_NAME=${IMAGE_NAME} \
                     --plan /plans/gating
```

### Run Image mode testing

```
testing-farm request --context 'distro=rhel-10.0' --compose RHEL-10.0-image-mode --arch x86_64 --git-url https://pkgs.devel.redhat.com/git/tests/giflib --git-ref master  --plan basic
```

### Run Upgradability test (CUT)

```
testing-farm request --git-url  https://pkgs.devel.redhat.com/git/tests/rhel-system-roles --compose RHEL-8.10.0-Nightly --plan "upgrade-el8toel9$" --git-ref master --context='distro=rhel-9' --no-wait
```

