Ansible role to manage password complexity, history and expiration

[![Build Status](https://travis-ci.org/riponbanik/ansible-role-password-policy.svg?branch=master)](https://travis-ci.org/riponbanik/ansible-role-password-policy)

It will install the required package and configure policy files to ensure password complexity, history and expiration are met on Nix systems for local users

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

# password complexity
`minlen: 8
lcredit: -1
ucredit: -1
dcredit: -1
ocredit: -1`

# password history
remember: 12

# password expiration
PASS_MAX_DAYS: 60
PASS_MIN_DAYS: 0
PASS_MIN_LEN: 8
PASS_WARN_AGE: 7

## Dependencies

None.

## Example Playbook

    - hosts: servers
      vars_files:
        - vars/main.yml
      roles:
        - { role: riponbanik.password-policy }

*Inside `vars/main.yml`*:

    remember: 24

## License

MIT / BSD

## Author Information

This role was created in 2018 by [Ripon Banik ](https://www.linkedin.com/in/ripon-banik-79956b23/)
