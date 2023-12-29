# Ansible role [zeppelin](https://galaxy.ansible.com/ui/standalone/roles/buluma/zeppelin/documentation)

Ansible Role for zeppelin installation

|GitHub|Version|Issues|Pull Requests|Downloads|
|------|-------|------|-------------|---------|
|[![github](https://github.com/buluma/ansible-role-zeppelin/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-zeppelin/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-zeppelin.svg)](https://github.com/buluma/ansible-role-zeppelin/releases/)|[![Issues](https://img.shields.io/github/issues/buluma/ansible-role-zeppelin.svg)](https://github.com/buluma/ansible-role-zeppelin/issues/)|[![PullRequests](https://img.shields.io/github/issues-pr-closed-raw/buluma/ansible-role-zeppelin.svg)](https://github.com/buluma/ansible-role-zeppelin/pulls/)|[![Ansible Role](https://img.shields.io/ansible/role/d/buluma/zeppelin)](https://galaxy.ansible.com/ui/standalone/roles/buluma/zeppelin/documentation)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/buluma/ansible-role-zeppelin/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: true
  vars:
    - zeppelin_java_version: "{{ lookup('env','JAVA_VERSION') | default('11', True) }}"
    - zeppelin_working_directory: /zeppelin_working_directory
    - zeppelin_mem: "-Xmx512m -XX:MaxPermSize=256m"
  tasks:
    - name: "Include buluma.zeppelin"
      ansible.builtin.include_role:
        name: "buluma.zeppelin"
  post_tasks:
    - name: Restart any services before running the tests
      ansible.builtin.meta: flush_handlers
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/buluma/ansible-role-zeppelin/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  become: yes
  gather_facts: no

  roles:
    - role: buluma.bootstrap
    - role: buluma.python_pip
    - role: buluma.pip
```

Also see a [full explanation and example](https://buluma.github.io/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/buluma/ansible-role-zeppelin/blob/master/defaults/main.yml):

```yaml
---
# defaults file for zeppelin

zeppelin_version: 0.9.0
zeppelin_download_url: http://archive.apache.org/dist/zeppelin/zeppelin-{{ zeppelin_version }}/zeppelin-{{ zeppelin_version }}-bin-all.tgz
zeppelin_service_username: zeppelin
zeppelin_service_group: zeppelin
zeppelin_working_directory: /tmp/zeppelin_working_directory
zeppelin_java_home:
# Supported Java Versions:
# CentOS 7: latest, 11, 8, or 7
# CentOS 8: latest, 11, 8
# Ubuntu 18.04: 11 or 8
# Ubuntu 20.04: 14, 13, 11, or 8
# Ubuntu 22.04: 14, 13, 11, or 8
zeppelin_java_version: 11

# Config options for zeppelin-site.xml
# See templates/zeppelin-site.xml.j2 for defaults.
zeppelin_server_addr: 0.0.0.0
zeppelin_server_port: 8080
zeppelin_server_context_path: /
zeppelin_war_tempdir: webapps
zeppelin_notebook_dir: notebook
zeppelin_notebook_homescreen:
zeppelin_notebook_homescreen_hide: false

zeppelin_notebook_s3_storage_enabled: false
zeppelin_notebook_s3_user: user
zeppelin_notebook_s3_bucket: zeppelin
zeppelin_notebook_s3_endpoint: s3.amazonaws.com
zeppelin_notebook_s3_storage: org.apache.zeppelin.notebook.repo.S3NotebookRepo

zeppelin_notebook_s3_storage_kms: false
zeppelin_notebook_s3_kms_key_id: AWS-KMS-Key-UUID

zeppelin_notebook_s3_encryption_materials_provider: false
zeppelin_notebook_s3_encryption_materials_provider_class: provider implementation class name

zeppelin_notebook_azure_storage_enabled: false
zeppelin_notebook_azure_connection_string: DefaultEndpointsProtocol=https;AccountName=<accountName>;AccountKey=<accountKey>
zeppelin_notebook_azure_share: zeppelin
zeppelin_notebook_azure_user: user
zeppelin_notebook_azure_storage: org.apache.zeppelin.notebook.repo.AzureNotebookRepo

zeppelin_notebook_git_storage_enabled: false
zeppelin_notebook_git_storage: org.apache.zeppelin.notebook.repo.GitNotebookRepo

zeppelin_notebook_zeppelinhub_storage_enabled: false
zeppelin_notebook_zeppelinhub_storage: org.apache.zeppelin.notebook.repo.VFSNotebookRepo, org.apache.zeppelin.notebook.repo.zeppelinhub.ZeppelinHubRepo

zeppelin_notebook_local_storage_enabled: true
zeppelin_notebook_local_storage: org.apache.zeppelin.notebook.repo.VFSNotebookRepo

zeppelin_interpreter_dir: interpreter
zeppelin_interpreter_localRepo: local-repo
zeppelin_interpreters: org.apache.zeppelin.spark.SparkInterpreter,org.apache.zeppelin.spark.PySparkInterpreter,org.apache.zeppelin.rinterpreter.RRepl,org.apache.zeppelin.rinterpreter.KnitR,org.apache.zeppelin.spark.SparkRInterpreter,org.apache.zeppelin.spark.SparkSqlInterpreter,org.apache.zeppelin.spark.DepInterpreter,org.apache.zeppelin.markdown.Markdown,org.apache.zeppelin.angular.AngularInterpreter,org.apache.zeppelin.shell.ShellInterpreter,org.apache.zeppelin.file.HDFSFileInterpreter,org.apache.zeppelin.flink.FlinkInterpreter,,org.apache.zeppelin.python.PythonInterpreter,org.apache.zeppelin.lens.LensInterpreter,org.apache.zeppelin.ignite.IgniteInterpreter,org.apache.zeppelin.ignite.IgniteSqlInterpreter,org.apache.zeppelin.cassandra.CassandraInterpreter,org.apache.zeppelin.geode.GeodeOqlInterpreter,org.apache.zeppelin.postgresql.PostgreSqlInterpreter,org.apache.zeppelin.jdbc.JDBCInterpreter,org.apache.zeppelin.kylin.KylinInterpreter,org.apache.zeppelin.elasticsearch.ElasticsearchInterpreter,org.apache.zeppelin.scalding.ScaldingInterpreter,org.apache.zeppelin.alluxio.AlluxioInterpreter,org.apache.zeppelin.hbase.HbaseInterpreter,org.apache.zeppelin.livy.LivySparkInterpreter,org.apache.zeppelin.livy.LivyPySparkInterpreter,org.apache.zeppelin.livy.LivySparkRInterpreter,org.apache.zeppelin.livy.LivySparkSQLInterpreter
zeppelin_interpreter_connect_timeout: 30000

zeppelin_ssl: false
zeppelin_ssl_client_auth: false
zeppelin_ssl_keystore_path: keystore
zeppelin_ssl_keystore_type: JKS
zeppelin_ssl_keystore_password: change me

zeppelin_ssl_key_manager_password: change me

zeppelin_ssl_truststore_path: truststore
zeppelin_ssl_truststore_type: JKS

zeppelin_ssl_truststore_password: change me

zeppelin_server_allowed_origins: "*"
zeppelin_anonymous_allowed: true
zeppelin_websocket_max_text_message_size: 1024000

# Config options for zeppelin-env.sh
# If empty the Zeppelin defaults will be used, see templates/zeppelin-env.sh.j2 for defaults.
zeppelin_spark_master:
zeppelin_java_opts:
zeppelin_mem:
zeppelin_intp_java_opts:
zeppelin_intp_mem:

zeppelin_spark_home:
zeppelin_spark_submit_options:
zeppelin_spark_app_name:
zeppelin_hadoop_conf_dir:
zeppelin_pyspark_python:
zeppelin_pythonpath:

# Config options for shiro.ini
zeppelin_users:
  user: user
  password: password
zeppelin_roles:
  role: role
  perms: '*'
zeppelin_auth_active_directory_enabled: false
zeppelin_auth_active_directory_options: {}
zeppelin_auth_ldap_enabled: false
zeppelin_auth_ldap_options: {}
zeppelin_user_caching_enabled: false
zeppelin_global_session_timeout: 86400000
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-zeppelin/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | Version |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Ansible Molecule](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-bootstrap.svg)](https://github.com/shadowwalker/ansible-role-bootstrap)|
|[buluma.python_pip](https://galaxy.ansible.com/buluma/python_pip)|[![Ansible Molecule](https://github.com/buluma/ansible-role-python_pip/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-python_pip/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-python_pip.svg)](https://github.com/shadowwalker/ansible-role-python_pip)|
|[buluma.pip](https://galaxy.ansible.com/buluma/pip)|[![Ansible Molecule](https://github.com/buluma/ansible-role-pip/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-pip/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-pip.svg)](https://github.com/shadowwalker/ansible-role-pip)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:

![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-zeppelin/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|[Ubuntu](https://hub.docker.com/repository/docker/buluma/ubuntu/general)|all|
|[Debian](https://hub.docker.com/repository/docker/buluma/debian/general)|all|

The minimum version of Ansible required is 2.12, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/buluma/ansible-role-zeppelin/issues)

## [Changelog](#changelog)

[Role History](https://github.com/buluma/ansible-role-zeppelin/blob/master/CHANGELOG.md)

## [License](#license)

[Apache-2.0](https://github.com/buluma/ansible-role-zeppelin/blob/master/LICENSE)

## [Author Information](#author-information)

[Shadow Walker](https://buluma.github.io/)


Template inspired by [Robert de Bock](https://github.com/robertdebock)
