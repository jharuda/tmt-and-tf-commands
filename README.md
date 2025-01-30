# Tmt Commands

[[_TOC_]]

## tmt-try

```
tmt try rhel-8.10@minute
tmt try -l rhel-8.10@minute
```

for logging in VM before test execution (like -n in 1mt)

```
tmt try -p /plans/basic rhel-8.10@minute
```


## tmt-run

### run preparation from plan before test execution

```
tmt run -vvv --all execute --how tmt -i provision --how minute -i <1MT_IMAGE> tests --name .
```

### Login after test execution

```
tmt -vvv run --all plans --name basic execute --how tmt -i provision -h minute -i 1MT-RHEL-7.9-updates-20230531.0 tests --name . login
```

### Execute test in 1minutetip on existing machine

```
tmt run -vvv --all provision --how connect --guest=10.0.186.112 --user=root --password=root --become tests --name .
```



## tmt-tests

### Execute test on existing machine

Print tests and information in the whole repository

```
tmt tests show
```

Print tests and information based on filter condition - in this case test execution 15 minutes in fmf. Can be used for all fmf properties (enabled, contact, summary)

```
tmt tests show --filter duration:15m
```

### tmt-lint

Linter for all files (few test to check metadata validity)
The second parameter (...TC…) is optional. If it is not set, then it lints all fmf files

```
tmt lint [/Sanity/Upstream-testsuite/TC#0604124]
```

### Generate test ID

tmt test id .

tmt tests export --format json show . | jq

### Export information about tests to JSON format

```
tmt --root "${tmt_tests_root}/${component}" run -v discover plan --name "$plan"
````

tmt_tests_root is a path to tests repository
component is rhel-system-roles
plan to filter - e.g. basic



## License
[MIT](LICENSE)

