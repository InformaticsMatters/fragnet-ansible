# fragnet-ansible

[![Build Status](https://travis-ci.com/InformaticsMatters/fragnet-ansible.svg?branch=master)](https://travis-ci.com/InformaticsMatters/fragnet-ansible)
![GitHub release (latest by date)](https://img.shields.io/github/v/release/informaticsmatters/fragnet-ansible)

Ansible Roles (and playbooks) for the fragnet project
suitable for execution by [AWX].

1.  For each **Role** you must provide a playbook in the project root
    called `site-<role>.yaml`.
1.  If there are playbooks _within_ a role's tasks that can be played then
    you must provide a playbook in the project that runs these individual plays
    called `site-<role>_<task>.yaml`

---

[awx]: https://github.com/ansible/awx
