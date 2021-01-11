# Fragnet (Ansible)

![lint](https://github.com/InformaticsMatters/fragnet-ansible/workflows/lint/badge.svg)

![GitHub tag (latest SemVer)](https://img.shields.io/github/v/tag/informaticsmatters/fragnet-ansible)

[![CodeFactor](https://www.codefactor.io/repository/github/informaticsmatters/fragnet-ansible/badge)](https://www.codefactor.io/repository/github/informaticsmatters/fragnet-ansible)

Ansible Roles (and playbooks) for the Kubernetes-based Fragnet Search/UI
projects, suitable for execution by [AWX].

This project contains two Ansible roles:-

*   **fragnet** (which is used for Depict, Search or UI deployment)
*   **fragnet-test**

>   Note: The Roles are designed to be executed from within our AWX server
    where credentials for the cluster (Kubernetes), Keycloak and Graph reside.
    If you're not running from AWX (discouraged) then you will need to provide
    values for the variables that would be injected by AWX.
 
As with all of our playbooks you can find the common user-defined variables
in each role's `defaults/main.yaml` and less common variables in
`vars/main.yaml`.

## Ansible Vault
The project uses files encrypted by Ansible Vault. AWX will inject
the vault secret but if you want to edit or inspect the secret file
(`roles/fragnet/vars/pull-secrets.vault`) you will need
the vault key, which can be found in our **KeePass** application under
`fragnet-search -> Ansible Vault Password`: -

    $ ansible-vault edit roles/fragnet/vars/pull-secrets.vault

## fragnet role
As the search depends on a privately-hosted container image
(`registry.gitlab.com/informaticsmatters/fragnet-ui`) the playbook
creates a **Secret** object, used by the UI **Deployment** in its
**imagePullSecrets**. This secret allows Kubernetes to pull from the
GitLab container registry.

Refer to the [Fragnet UI] repository for details of how to pull
the image yourself and to our [documentation] on generating
secrets suitable for use as `imagePullSecrets`.

The Role has a number of playbooks: -

1.  `site-fragnet.yaml` (The main application deployment)
2.  `site-fragnet_add-route.yaml`
    (legacy as main playbook deploys an Ingress but still of some use)
3.  `site-fragnet_remove-route.yaml` 
    (legacy as main playbook deploys an Ingress but still of some use)

The main role deploys: -

-   A namespace/project and ServiceAccount
-   Various ConfigMaps
-   Roles and RoleBindings
-   Deployment, Service and Ingress

...and depends on our [infrastructure] project's Kubernetes
**Certificate Manager** to generate https certificates.

## fragnet-test role
The test playbook uses the Fragnet UI's API to run a number of queries
based on the Graph combination that's been deployed. So, if your
Fragnet-UI is deployed against a graph that was built with our
**combination 10** you'd run the tests with
`fragnet_deployed_graph_id: combination_10`.

The Role has one playbook: -

1.  `site-fragnet-test.yaml`

There are basic tests for the following fragment combinations: -

-   `combination_5`
-   `combination_6`
-   `combination_10`

### Creating new tests
If you deploy a new Graph combination you should prepare
a set of test results for the Availability Query the test runs.
This is simple and just requires you to create a file called
`roles/fragnet-saerch-test/vars/test_combination_<N>.yaml`
where `<N>` is the combination. The file should define two variables: -

-   `fragnet_expected_suppliers` to list all the expected suppliers in the
    database
-   `availability_tests` which is a map of molecules and results.

Use `test_combination_10.yaml` as the starting point of your new test.  

---

[awx]: https://github.com/ansible/awx
[documentation]: https://informaticsmatters.gitlab.io/documentation/process/dev-image-pull-secrets.html
[fragnet ui]: https://gitlab.com/informaticsmatters/fragnet-ui
[infrastructure]: https://github.com/InformaticsMatters/ansible-infrastructure
