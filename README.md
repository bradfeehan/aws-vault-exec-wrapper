aws-vault-exec-wrapper
======================

This is a simple wrapper around [`aws-vault`] and similar tools, which
allows defining simple aliases to run specific commands with AWS
credentials.

Examples
--------

```bash
# Before:
aws-vault exec production -- aws s3 ls

# After:
vaws production s3 ls
```

Installation
------------

This script can be installed on macOS with [Homebrew]:

```bash
brew tap bradfeehan/formulae
brew install aws-vault-exec-wrapper
```

It's also just a simple shell script, so you can download the latest
release, put it somewhere on your `$PATH` and make it executable with
`chmod +x aws-vault-exec-wrapper` or similar.

[Homebrew]: <https://brew.sh>

Usage
-----

```bash
aws-vault-exec-wrapper <VAULT_PROGRAM> <COMMAND> <PROFILE> [ARGUMENTS]
```

For example:

```bash
aws-vault-exec-wrapper aws-vault aws production s3 ls
=> aws-vault exec production -- aws s3 ls
```

This isn't really any shorter, but the power comes from defining
aliases in your `.bashrc` or `.profile`:

```bash
alias vaws='aws-vault-exec-wrapper aws-vault aws'
alias avtf='aws-vault-exec-wrapper aws-vault terraform'
```

Then, you can run the alias, followed by the AWS profile name to use,
with any command-line arguments trailing that. For example:

```bash
$ vaws production s3 ls
=> aws-vault exec production -- aws s3 ls
# ...

$ avtf production-admin apply
=> aws-vault exec production-admin -- terraform apply
Refreshing Terraform state in-memory prior to plan...
# ...
```

You can avoid typing the profile name by setting a default using the
`AWS_PROFILE` environment variable:

```bash
# YOLO
$ export AWS_PROFILE=production
$ vaws s3 ls
=> aws-vault exec production -- aws s3 ls
```

If this environment variable is set, it won't look for a profile name
on the command line. Set it in a particular shell window to set the
profile for that window, or in your `~/.bashrc` or `~/.profile` to take
effect everywhere.

Compatibility
-------------

Tested with:

* [`aws-vault`] (written by [99designs])
* [`aws-okta`] for [Okta] integration (written by [Segment.io])

[`aws-vault`]: <https://github.com/99designs/aws-vault>
[99designs]: <https://99designs.com.au/tech-blog/blog/2015/10/26/aws-vault/>
[`aws-okta`]: <https://github.com/segmentio/aws-okta>
[Okta]: <https://www.okta.com>
[Segment.io]: <https://segment.com/blog/secure-access-to-100-aws-accounts/>
