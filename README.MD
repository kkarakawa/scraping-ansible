# 実行環境
+ Applicationサーバ
 + 192.168.33.11
+ Databaseサーバ
 + 192.168.33.12

# Databaseサーバのセットアップ
## 疎通確認
```
$ ansible database -i hosts -m ping -k
SSH password: vagrant
192.168.33.12 | success >> {
    "changed": false, 
    "ping": "pong"
}
```

## 実行対象の確認
```
$ ansible-playbook -l database database.yml -i hosts --list-host

playbook: database.yml

  play #1 (database): host count=1
    192.168.33.12
```

## playbook実行
```
$ ansible-playbook -l database database.yml -i hosts -k -K
SSH password: vagrant
SUDO password[defaults to SSH password]: vagrant

PLAY [database] *************************************************************** 

GATHERING FACTS *************************************************************** 
ok: [192.168.33.12]

TASK: [mysql | check mysql is installed] ************************************** 
ok: [192.168.33.12]

TASK: [mysql | mysql install] ************************************************* 
ok: [192.168.33.12] => (item=mysql,mysql-server,mysql-devel,MySQL-python)

TASK: [mysql | mysqld is running and enabled] ********************************* 
ok: [192.168.33.12]

TASK: [mysql | get mysql password] ******************************************** 
skipping: [192.168.33.12]

TASK: [mysql | set root password file] **************************************** 
skipping: [192.168.33.12]

TASK: [mysql | set mysql password] ******************************************** 
skipping: [192.168.33.12]

TASK: [mysql | remove root password file] ************************************* 
skipping: [192.168.33.12]

TASK: [mysql | set mysql config] ********************************************** 
ok: [192.168.33.12]

TASK: [mysql | delete test database] ****************************************** 
ok: [192.168.33.12]

TASK: [mysql | create application user] *************************************** 
ok: [192.168.33.12] => (item=192.168.33.11)

PLAY RECAP ******************************************************************** 
192.168.33.12              : ok=7    changed=0    unreachable=0    failed=0   
```

# Applicationサーバのセットアップ
## 疎通確認
```
$ ansible application -i hosts -m ping -k
SSH password: vagrant
192.168.33.11 | success >> {
    "changed": false, 
    "ping": "pong"
}
```

## 実行対象の確認
```
$ ansible-playbook -l application application.yml -i hosts --list-host

playbook: application.yml

  play #1 (application): host count=1
    192.168.33.11
```

## playbook実行
```
$ ansible-playbook -l application application.yml -i hosts -k -K
SSH password: vagrant
SUDO password[defaults to SSH password]: vagrant

PLAY [application] ************************************************************ 

GATHERING FACTS *************************************************************** 
ok: [192.168.33.11]

TASK: [common | yum install] ************************************************** 
ok: [192.168.33.11] => (item=gcc,gcc-c++,zlib-devel,openssl-devel,git,libcurl-devel,httpd-devel,apr-devel,apr-util-devel,libxml2,libxml2-devel,libxslt,libxslt-devel,mysql-devel)

TASK: [httpd | httpd install] ************************************************* 
ok: [192.168.33.11]

TASK: [httpd | service httpd start] ******************************************* 
ok: [192.168.33.11]

TASK: [httpd | set httpd.conf] ************************************************ 
ok: [192.168.33.11]

TASK: [ruby | check ruby is installed] **************************************** 
ok: [192.168.33.11]

TASK: [ruby | get ruby source package] **************************************** 
skipping: [192.168.33.11]

TASK: [ruby | expand ruby source package] ************************************* 
skipping: [192.168.33.11]

TASK: [ruby | configure ruby] ************************************************* 
skipping: [192.168.33.11]

TASK: [ruby | make ruby] ****************************************************** 
skipping: [192.168.33.11]

TASK: [ruby | install ruby] *************************************************** 
skipping: [192.168.33.11]

TASK: [ruby | install nokogiri] *********************************************** 
ok: [192.168.33.11]

TASK: [ruby | gem install] **************************************************** 
ok: [192.168.33.11] => (item=rails)
ok: [192.168.33.11] => (item=passenger)

TASK: [passenger | passenger install] ***************************************** 
ok: [192.168.33.11]

TASK: [passenger | passenger-module install] ********************************** 
skipping: [192.168.33.11]

TASK: [scraping | Make Scraping Directory] ************************************ 
ok: [192.168.33.11]

TASK: [scraping | Checkout Scraping] ****************************************** 
ok: [192.168.33.11]

TASK: [scraping | bundle config nokogiri] ************************************* 
ok: [192.168.33.11]

TASK: [scraping | bundle install] ********************************************* 
ok: [192.168.33.11]

TASK: [scraping | set database config] **************************************** 
ok: [192.168.33.11]

TASK: [scraping | create database] ******************************************** 
ok: [192.168.33.11]

TASK: [scraping | db migrate] ************************************************* 
ok: [192.168.33.11]

TASK: [scraping | crawl videos] *********************************************** 
ok: [192.168.33.11]

PLAY RECAP ******************************************************************** 
192.168.33.11              : ok=17   changed=0    unreachable=0    failed=0 
```

# Webアクセス
http://192.168.33.11/
