---
title: null
description: null
date: null
redirect:
  - /dNPWQw
---

# Best practices in backend development

- [N commandments](#n-commandments)
- [General points on guidelines](#general-points-on-guidelines)
- [Development environment setup in README.md](#development-environment-setup-in-readmemd)
- [Data persistence](#data-persistence)
  - [General considerations](#general-considerations)
  - [SaaS, cloud-hosted or self-hosted?](#saas-cloud-hosted-or-self-hosted)
  - [Persistence solutions](#persistence-solutions)
    - [RDBMS](#rdbms)
    - [NoSQL](#nosql)
      - [Document storage](#document-storage)
      - [Key-value store](#key-value-store)
      - [Graph database](#graph-database)
- [Bill of materials](#bill-of-materials)
- [Security](#security)
  - [Docker](#docker)
  - [Credentials](#credentials)
  - [Secrets](#secrets)
  - [Login throttling](#login-throttling)
  - [User password storage](#user-password-storage)
  - [Audit log](#audit-log)
  - [Suspicious action throttling and/or blocking](#suspicious-action-throttling-andor-blocking)
  - [Anonymized data](#anonymized-data)
  - [Temporary file storage](#temporary-file-storage)
  - [Dedicated vs shared server environment](#dedicated-vs-shared-server-environment)
- [Checklists](#checklists)
  - [Responsibility checklist](#responsibility-checklist)
  - [Release checklist](#release-checklist)
- [General questions to consider](#general-questions-to-consider)
- [Generally proven useful tools](#generally-proven-useful-tools)

# N commandments

1. README.md in the root of the repo is the docs
2. Single command run
3. Single command deploy
4. Repeatable and re-creatable builds
5. Build artifacts bundle a ["Bill of materials"](#bill-of-materials)
6. Use [UTC as the timezone](http://yellerapp.com/posts/2015-01-12-the-worst-server-setup-you-can-make.html) all around

# General points on guidelines

We do not want to limit ourselves to certain tech stacks or frameworks. Different problems require different solutions, and hence these guidelines are valid for various backend architectures.

# Development environment setup in README.md

Document all the parts of the development/server environment. Strive to use the same setup and versions on all environments, starting from developer laptops, and ending with the actual production environment. This includes the database, application server, proxy server (nginx, Apache, ...), SDK version(s), gems/libraries/modules.

Automate the setup process as much as possible. For example, [Docker Compose](https://docs.docker.com/compose/) could be used both in production and development to set up a complete environment, where [Dockerfiles](https://docs.docker.com/articles/dockerfile_best-practices/) fetch all parts of the software, and contain the necessary scripting to setup the environment and all the parts of it. Consider using archived copies of the installers, in case upstream packages later become unavailable. A minimum precaution is to keep a SHA-1 checksums of the packages, and to make sure that the checksum matches when the packages are installed.

Consider storing any relevant parts of the development environment and its dependencies in some persistent storage. If the environment can be built using Docker, one possible way to do this is to use [docker export](http://docs.docker.com/reference/commandline/cli/#export).

# Data persistence

## General considerations

Independent of the persistence solution your project uses, there are general considerations that you should follow:

- Have backups that are verified to work
- Have scripts or other tooling for copying persistent data from one env to another, e.g. from prod to staging in order to debug something
- Have plans in place for rolling out updates to the persistence solution (e.g. database server security updates)
- Have plans in place for scaling up the persistence solution
- Have plans or tooling for managing schema changes
- Have monitoring in place to verify health of the persistence solution

## SaaS, cloud-hosted or self-hosted?

An important choice regarding any solution is where to run it.

- SaaS -- fast to get started, easy to scale up, some infrastructure work required to allow access from everywhere etc.
- Self-hosted in the cloud -- allows tuning database more than SaaS and probably cheaper at scale in terms of hosting, but more labor-intensive
- Self-hosted on own hardware -- able to tweak everything and manage physical security, but most expensive and labor intensive

## Persistence solutions

This section aims to provide some guidance for selecting the type of persistence solution. The choice always needs to be tailored to the problem and none of these is a silver bullet, however.

### RDBMS

Pick a relational database system such as PostgreSQL when data and transaction integrity is a major concern or when lots of data analysis is required. The [ACID compliance](https://en.wikipedia.org/wiki/ACID), aggregation and transformation functions of the RDBMS will help.

### NoSQL

Pick a NoSQL database when you expect to scale horizontally and when you don't require ACID. Pick a system that fits your model.

#### Document storage

Stores documents that can be easily addressed and searched for by content or by inclusion in a collection. This is made possible because the database understands the storage format. Use for just that: storing large numbers of structured documents. Notable examples:

- CouchDB
- ElasticSearch

> Note that since 9.4, PostgreSQL can also be used to store JSON natively.

#### Key-value store

Stores values, or sometimes groups of key-value pairs, accessible by key. Considers the values to be simply blobs, so does not provide the query capabilities of document stores. Scalable to immense sizes. Notable examples:

- Cassandra
- Redis

#### Graph database

General graph databases store nodes and edges of a graph, providing index-free lookups of the neighbors of any node. For applications where graph-like queries like shortest path or diameter are crucial. Specialized graph databases also exist for storing e.g. [RDF triples](https://en.wikipedia.org/wiki/Resource_Description_Framework).

# Bill of materials

This document must be included in every build artifact and shall contain the following:

1. What version(s) of an SDK and critical tools were used to produce it
1. Which dependencies have been included
1. A globally unique revision number of the build (i.e. a git SHA-1 hash)
1. The environment and variables used when building the package
1. A list of failed tests or checks

# Security

Be aware of possible security threats and problems. You should at least be familiar with the [OWASP Top 10 vulnerabilities](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project), and you should of monitor vulnerabilities in any third party software you use.

Good generic security guidelines would be:

## Docker

**Using Docker will not make your service more secure.** Generally, you should consider at least following things if using Docker:

- Don't run any untrusted binaries inside Docker containers
- Create unprivileged users inside Docker containers and run binaries using unprivileged user instead of root whenever possible
- Periodically rebuild and redeploy your containers with updated libraries and dependencies
- Periodically update (or rebuild) your Docker hosts with latest security updates
- Multiple containers running on same host will by default have some level of access to other containers and the host itself. Properly secure all hosts, and run containers with a minimum set of capabilities, for example preventing network access if they don't need it.

## Credentials

Never send credentials unencrypted over public network. Always use encryption (such as HTTPS, SSL, etc.).

## Secrets

Never store secrets (passwords, keys, etc.) in the sources in version control! It is very easy to forget they are there and the project source tends to end up in many places (developer machines, development test servers, etc) which unnecessarily increases the risk of an important secret being compromised. Also, version control has the nasty feature of overwriting file permissions, so even if you secure your config file permissions, the next time you check out the source, the permissions would be overwritten to the default public-readable.

Probably the easiest way to handle secrets is to put them in a separate file on the servers that need them, and to be ignored by version control. You can keep e.g. a `.sample` file in the version control, with fake values to illustrate what should go there in the real file. In some cases, it is not easy to include a separate configuration file from the main configuration. If this happens, consider using environment variables, or writing the config file from a version-controlled template on deployment.

## Login throttling

Place limits on the amount of login attempts allowed per client per unit of time. Lock a user account for specific time after a given number of failed attempts (e.g. lock for 5 minutes after 20 failed login attempts).
The aim of these measures is make online brute-force attacks against usernames/passwords infeasible.

## User password storage

> Never EVER store passwords in plaintext!

Never store passwords in reversible encrypted form, unless absolutely required by the application / system. Here is a good article about what and what not to do: <https://crackstation.net/hashing-security.htm>

If you do need to be able to obtain plaintext passwords from the database, here are some suggestions to follow.

If passwords won't be converted back to plaintext often (e.g. special procedure is required), keep decryption keys away from the application that accesses the database regularly.

If passwords still need to be regularly decrypted, separate the decryption functionality from the main application as much as possible, e.g. a separate server accepts requests to decrypt a password, but enforces a higher level of control, like throttling, authorization, etc.

Whenever possible (it should be in a great majority of cases), store passwords using a good one-way hash with a good random salt. And, no, SHA-1 is not a good choice for a hashing function in this context. Hash functions that are designed with passwords in mind are deliberately slower, which makes offline brute-force attacks more time consuming, hence less feasible. See this post for more details: <http://security.stackexchange.com/questions/211/how-to-securely-hash-passwords/31846#31846>

## Audit log

For applications handling sensitive data, especially where certain users are allowed a relatively wide access or control, it's good to maintain some kind of audit logging, storing a sequence of actions / events that took place in the system, together with the event/source originator (user, automation job, etc). This can be, e.g:

    2012-09-13 03:00:05 Job "daily_job" performed action "delete old items".
    2012-09-13 12:47:23 User "admin_user" performed action "delete item 123".
    2012-09-13 12:48:12 User "admin_user" performed action "change password of user foobar".
    2012-09-13 13:02:11 User "sneaky_user" performed action "view confidential page 567".
    ...

The log may be a simple text file or stored in a database. At least these three items are good to have: an exact timestamp, the action/event originator (who did this), and the actual action/event (what was done). The exact actions to be logged depend on what is important for the application itself, of course.

The audit log may be a part of the normal application log, but the emphasis here is on logging who did what and not only that a certain action was performed. If possible, the audit log should be made tamper-proof, e.g. only be accessible by a dedicated logging process or user and not directly by the application.

## Suspicious action throttling and/or blocking

This can be seen as a generalization of the Login throttling, this time introducing similar mechanics for arbitrary actions that are deemed "suspicious" within the context of the application. For example, an ERP system which allows normal users access to a substantial amount of information, but expects users to be concerned only with a small subset of that information, may limit attempts to access larger than expected datasets too quickly. E.g. prevent users from downloading list of all customers, if users are supposed to work on one or two customers at a time. Note that this is different from limiting access completely, users are still allowed to retrieve information about any customer, just not all of them at once. Depending on the system, throttling might not be enough, e.g. when one invokes an action on all resources with a single request. Then blocking might be required. Note the difference between making 1000 requests in 10 seconds to retrieve full customer information, one customer at a time, and making a single request to retrieve that information at once.

What is suspicious here depends strongly on the expected use of the application. E.g. in one system, deleting 10000 records might be completely legitimate action, but not so in an another one.

## Anonymized data

Whenever large datasets are exported to third parties, data should be anonymized as much as possible, given the intended use of the data. For example, if a third party service will provide general statistical analysis on a customer database, it probably does not need to know the names, addresses or other personal information for individual customers. Even a generic customer ID number might be too revealing, depending on the data set. Take a look at this article: <http://arstechnica.com/tech-policy/2009/09/your-secrets-live-online-in-databases-of-ruin/>.

Avoid logging personally identifiable information, for example user’s name.

If your logs contain sensitive information, make sure you know how logs are protected and where they are located also in the case of cloud hosted log management systems.

If you must log sensitive information try hashing before logging so you can identify the same entity between different parts of the processing.

## Temporary file storage

Make sure you are aware where your application is storing temporary files. If you are using publicly accessible directories (which are most probably the default) like `/tmp` and `/var/tmp`, make sure you create your files with mode 600, so that they are readable only by the user your application is running as. Alternatively, have a protected directory for storing temporary files (directory accessible only by the application user).

## Dedicated vs shared server environment

The security threats can be quite different depending on whether the application is going to run in a shared or a dedicated environment. Shared here means that there are other (not necessarily 3rd party) applications running on the same server. In that case, having appropriate file permissions becomes critical, otherwise application source code, data files, temporary files, logs, etc might end up accessible by unintended users. Then a security breach in a 3rd party application might result in your application being compromised.

You can never be sure what kind of an environment your application will run for its entire life time, it may start on a dedicated server, but as time goes, 3rd party applications might be added to the same system. That is why it is best to plan from the very first moment that your application runs in a shared environment, and take all precautions. Here's a non-exhaustive list of the files/directories you need to think about:

- application source code
- data directories
- temporary storage directories (often by default the system wide /tmp might be used - see above)
- configuration files
- version control directories - .git, .hg, .svn, etc.
- startup scripts (may contain initialization variables, secrets, etc)
- log files
- crash dumps
- private keys (SSL, SSH, etc)
- etc.

Sometimes, some files need to be accessible by different users (e.g. static content served by apache). In that case, take care to allow only access to what is really needed.

Keep in mind that on a UNIX/Linux filesystem, write access to a directory is permission-wise very powerful, it allows you to delete files in that directory and recreate them (which results in a modified file). /tmp and /var/tmp are by default safe from this effect, because of the sticky bit that should be set on those.

Additionally, as mentioned in the secrets section, file permissions might not be preserved in version control, so even if you set them once, the next checkout/update/whatever may override them. A good idea is then to have a Makefile, a script, a version control hook or something similar that would set the correct permissions when updating the sources.

# General questions to consider

- What is the expected/required life-span of the project?

- Is the project one-off, or will there be continuous development?
- What is the release cycle for a version of the service?
- What environments (dev, test, staging, prod, ...) are going to be set up?
- How will downtime of the production service impact the value of the service?
- How mature is the technology? Is major changes that break backward compatibility to be expected?

# Generally proven useful tools

- [HTTPie](https://github.com/jakubroztocil/httpie) is a great tool for testing APIs on the command line. It's simple to pass in custom headers and cookies, and it even has session support.

- [jq](http://stedolan.github.io/jq/) is a CLI JSON processor. Massage JSON data coming in from cURL (or of course HTTPie!) at will. Another great tool for API testing or exploration.

### Acknowledgements

Thanks to [Futurice](https://github.com/futurice/backend-best-practices). We hard forked and made a revised version based on their guideline.
