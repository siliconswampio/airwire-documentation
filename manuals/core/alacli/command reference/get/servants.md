# Servants
## Description

Retrieve accounts which are servants of a given account
## Info

**Command**

    alacli get servants

**Output**

```
Usage: alacli get servants account

Positionals:
  account TEXT                The name of the controlling account
```

## Command

    alacli get servants inita

## Output

```
{
  "controlled_accounts": [
    "tester"
  ]
}
```