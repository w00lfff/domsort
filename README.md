# domsort

A simple and powerful command-line tool to hierarchically sort and scope domains. It automatically discovers parent domains and filters them based on a specified scope, making it perfect for cleaning up subdomain lists for reconnaissance.

## Features

* **Hierarchical Sorting**: Sorts domains from the root down (e.g., `example.com` before `sub.example.com`).
* **Parent Discovery**: Automatically finds and adds parent domains (e.g., finds `example.com` from `sub.example.com`).
* **Scope Filtering**: Use the `-d` flag to only show domains that belong to a specific scope.
* **Flexible Input**: Reads from a file (`-f`) or from stdin.

## Installation
```bash
go install -v github.com/w00lfff/domsort@latest
```
or
```bash
# Clone or download the domsort.go file, then run:
go build domsort.go

# You can move the generated 'domsort' binary to your path, e.g.:
# sudo mv domsort /usr/local/bin/
```
# Usage
```
# Basic usage from a file
./domsort -f <file>

# Usage with scope filtering
./domsort -f <file> -d <domain-scope>

# Usage with stdin
cat <file> | ./domsort -d <domain-scope>
```

## Examples
Let's use the following example domains.txt file for all scenarios:
```
$cat domains.txt
api.test.target.com
www.target.com
assets.other.com
test.target.com
```
## 1. Basic Sorting and Discovery
This example shows how domsort automatically finds all parent domains and sorts the combined list hierarchically.

```
$./domsort -f domains.txt
other.com
assets.other.com
target.com
test.target.com
api.test.target.com
www.target.com
```
Notice how other.com and target.com were automatically added and the entire list is sorted.

## 2. Scoping to a Root Domain
Here, we only want domains that belong to target.com.
```
$./domsort -f domains.txt -d target.com
target.com
test.target.com
api.test.target.com
www.target.com
```
The output is now filtered to only show assets belonging to target.com.

## 3. Scoping to a Specific Subdomain
This is the most powerful feature. We only want assets belonging to the test.target.com scope, and we want to exclude the parent target.com.
```
$./domsort -f domains.txt -d test.target.com
test.target.com
api.test.target.com
```
