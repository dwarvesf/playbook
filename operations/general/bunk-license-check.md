---
tags: 
  - operations
  - project
title: Bunk license check
date: 2020-01-01
description: Glice is a Golang license and dependency checker, providing detailed insights into dependencies and their licenses
authors: 
  - hnh
menu: playbook
type: null
hide_frontmatter: false
---

## License detector tool: Glice
Golang license and dependency checker. Prints list of all dependencies (both from std and 3rd party), number of times used, their license and saves all the license files in /licenses.

### Build
- Clone Glice to your local workspace: `$go get github.com/ribice/glice`
- Go install to your $GOBIN: `$ go install github.com/ribice/glice`

### Run
`$ glice`