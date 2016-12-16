このPlaybookについて
====================

これは、Ansibleで比較的よく使われると思われる変数定義の優先順位を確認する為のPlaybookです。

各変数は2ヶ所で値を定義しており、そのどちらが優先されたかが /tmp/variables.txt に出力されます。

ansible-playbook 2.0.0.2 では、各変数は以下のように上書きされました。

|変数名|優先度高い|優先度低い|
|:--|:--|:--|
|var_role_defaults| roles/foo/defaults/main.yml |
|var_inventory_group_vars| hostsのグループ変数行 | roles/foo/defaults/main.yml |
|var_inventory_host_vars | hostsのホスト名行  | hostsのグループ変数行 |
|var_group_vars_hosts | hostsのホスト名行 | group_vars/foo/main.yml |
|var_group_vars_group|group_vars/foo/main.yml | hostsのグループ変数行|
|var_host_vars_group | host_vars/localhost/main.yml | group_vars/foo/main.yml |
|var_host_vars_hosts | host_vars/localhost/main.yml | hostsのホスト名行|
|var_role_vars | roles/foo/vars/main.yml | host_vars/localhost/main.yml |

優先度順に定義場所を並べると次のようになります。

1. `roles/ROLENAME/defaults/` 以下のyaml (優先度低)
2. inventory(hosts)のグループ変数行 (`[group:vars]`セクションの内側)
3. `group_vars/GROUPNAME/` 以下のyaml
4. inventory(hosts)のホスト名行(`hostname varname=value`の行)
5. `host_vars/HOSTNAME/` 以下のyaml
6. `roles/ROLENAME/vars/` 以下のyaml (優先度高)

