# jupyterhub-container

Adds a Jupyter Hub service to your [Ansible Container](https://github.com/ansible/ansible-container) project. Run the following commands
to install the service:

```
# Set the working directory to your Ansible Container project root
$ cd myproject

# Install the service
$ ansible-container install marcusianlevine.jupyterhub-container
```

## Requirements

- [Ansible Container](https://github.com/ansible/ansible-container)
- An existing Ansible Container project. To create a project, simply run the following:
    ```
    # Create an empty project directory
    $ mkdir myproject

    # Set the working directory to the new directory
    $ cd myproject

    # Initialize the project
    $ ansible-contiainer init
    ```

## Role Variables

* `vars_files`
    * Optional list of paths relative to `/src` on the Conductor with extra variable YAML files
* `extra_pip_packages`: `[dockerspawner]`
    * List of extra pip packages to install alongside JupyterHub

### Files & Directories

* `jupyterhub_config_dir`: `/etc/jupyterhub/`
    * Absolute path to directory where JupyterHub config file will be placed
* `jupyterhub_config_path`: `"{{ jupyterhub_config_dir }}/jupyterhub_config.py"`
    * Absolute path of the JupyterHub config file
* `jupyterhub_srv_dir`: `/srv/jupyterhub`
* `jupyterhub_pip_version`: `0.8.1`
* `jupyter_config_dir`: `/etc/jupyter`
    * Absolute path to directory where Jupyter notebook config file will be placed
* `jupyter_share_dir`: `/usr/local/share/jupyter`
* `jupyter_templates_dir`: `"{{ jupyter_config_dir }}/templates"`
* `ipython_config_dir`: `/etc/ipython`

### Spawner config

* `use_helm`: `no`
    * Target [kubespawner](https://github.com/jupyterhub/kubespawner) via the [Zero-to-JupyterHub Helm Chart](https://github.com/jupyterhub/zero-to-jupyterhub-k8s)
* `allow_sudo`: `no`
    * Spawn single user notebooks with sudo permissions
    * WARNING: using this option with containerized Spawners will introduce breakout security risk
* `mem_limit`: `yes`
    * No limit by default. Set to a valid Docker or Kubernetes `mem_limit` e.g. 10G

### User authentication

* `use_oauth`: `no`
    * Use GitHub OAuth for user authentication
* `use_ldap`: `no`
    * Use [ldapauthenticator](https://github.com/jupyterhub/ldapauthenticator) for user authentication
    * See docs for details on following settings
* `ldap_domain`
* `ldap_bind_dn_template`
* `ldap_allowed_group`
* `ldap_user_search_base`
* `ldap_search_user`
* `ldap_user_attribute`
* `user_list`: `[]`
    * Whitelist of users to allow
* `admin_users`: `[]`
    * List of users to be given admin privileges

### Miniconda settings

* `miniconda_version`
    * Specify a particular version of [Miniconda](https://conda.io/miniconda.html) to install
* `conda_checksum`
    * REQUIRED IF `miniconda_version` DIFFERENT FROM DEFAULT!
* `conda_installer`: `Miniconda3-{{miniconda_version}}-Linux-x86_64.sh`
    * Templated name of installer script. Only needs to be changed if building on non-standard architecture.
* `conda_prefix`: `/opt/conda`
    * Absolute path to the directory in which Miniconda will be installed
* `conda_config`
    * Specify arbitrary conda config , e.g. default conda channels to search
    * See `vars/main.yml` for an example


## Dependencies

If targeting the default [dockerspawner](https://github.com/jupyterhub/dockerspawner) the host must have Docker installed e.g. via the role [mongrelion.docker](https://galaxy.ansible.com/mongrelion/docker/)

## License

BSD

## Author Information

Created by Marcus Levine for CKM Advisors.
