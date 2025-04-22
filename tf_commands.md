# tf_commands.md

## Links

List of [Testing Farm composes](http://storage.tft.osci.redhat.com/composes-production.html)

## Examples

```
testing-farm request --git-url  https://pkgs.devel.redhat.com/git/tests/net-snmp \
                     --compose RHEL-10-Beta-Nightly \
                     --plan "basic$" --git-ref master \
                     --arch ppc64le \
                     --redhat-brew-build=63220862 \
                     --context='distro=rhel-10' \
                     --test "/Regression/bz848155-package-net-snmp-5-5-41-el6-3-1-x86-64-always-fail"
```

```
testing-farm request --git-url https://github.com/RedHat-SP-Security/keylime-tests.git \
                     --plan-filter 'name: .*upstream-keylime-swtpm-dev' \
                     --test-filter 'name: /setup.*|name: /functional.*' \
                     --compose RHEL-9-Nightly \
                     --arch x86_64 \
                     --context arch=x86_64
```

```
testing-farm request --git-url https://gitlab.cee.redhat.com/toolchain-qe/tests/pcp-container.git \
                     --git-ref master \
                     --arch x86_64,s390x,ppc64le,aarch64 \
                     --compose RHEL-9-Nightly --environment IMAGE_NAME=${IMAGE_NAME} \
                     --plan /plans/gating
```

