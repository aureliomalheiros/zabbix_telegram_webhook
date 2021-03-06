# Sending notifications from Zabbix 4.4 to Telegram via Webhook using HTTP proxies

Using HTTP proxies in Webhook will be possible only in Zabbix v5.0, but I backported the patch (ZBXNEXT-5554) for version 4.4

If you are an experienced user, you can rebuild Zabbix from the source and apply this patch.

Link to the patch [itself](https://github.com/CHERTS/zabbix_telegram_webhook/tree/master/patch)

Below are instructions for rebuilding Zabbix v4.4 from source code for different OS.

[Build on Oracle Linux 7 with MySQL (MariaDB) support](#oracle-linux-7)

[Build on Red Hat Enterprise Linux 8 with MySQL (MariaDB) support](#red-hat-enterprise-linux-8)

[Build on Ubuntu 18.04 LTS (Bionic Beaver) with MySQL (MariaDB) support](#ubuntu)

# Oracle Linux 7
## Build on Oracle Linux 7 with MySQL (MariaDB) support

### 1. To prepare for build on Oracle Linux 7, you need to install additional packages:

~~~~
yum group install "Development Tools"
yum-config-manager --enable ol7_optional_latest
yum-config-manager --enable ol7_developer
yum install -y wget unzip gettext java-1.8.0-openjdk libxml2-devel openssl-devel libcurl-devel net-snmp-devel libevent-devel sqlite-devel pcre-devel libssh2-devel OpenIPMI-devel unixODBC-devel openldap-devel
yum install -y MariaDB-client MariaDB-devel MariaDB-shared
~~~~

### 2. Download and unzip the latest version of the source code:

~~~~
wget https://cdn.zabbix.com/stable/4.4.8/zabbix-4.4.8.tar.gz
tar -zxf zabbix-4.4.8.tar.gz
cd zabbix-4.4.8
wget https://raw.githubusercontent.com/CHERTS/zabbix_telegram_webhook/master/patch/ZBXNEXT-5554.patch
patch -p0 < ZBXNEXT-5554.patch
~~~~

### 3. Build all Zabbix components with MySQL (MariaDB) support:

~~~~
./configure --with-libpthread --with-libpcre --with-libcurl --with-libxml2 --with-net-snmp --with-openssl --enable-ipv6 --with-ssh2 --with-jabber --with-openipmi --with-unixodbc --with-ldap --enable-server --sysconfdir=/etc/zabbix --with-mysql
make
~~~~

### 4. After successful build, in step 3 you can use zabbix binaries, copy them to the current directory:

~~~~
cp src/zabbix_server/zabbix_server zabbix_server_mysql_v444
~~~~

Check for the presence of binary files:

~~~~
# ls -l | grep 'zabbix_'
-rwxr-xr-x  1 root root 6405704 Oct  9 21:28 zabbix_server_mysql_v444
~~~~

Congratulations! Now you can stop your zabbix-server and replace it with this assembly.

You also need to replace 1 file in the web-frontend:
~~~~
# cp frontends/php/app/controllers/CControllerMediatypeEdit.php /your-web-frontend-root-directory/app/controllers
~~~~

# Red Hat Enterprise Linux 8
## Build on Red Hat Enterprise Linux 8 with MySQL (MariaDB) support

### 1. To prepare for build on Red Hat Enterprise Linux 8, you need to install additional packages:

~~~~
dnf group install "Development Tools"
dnf install -y wget unzip gettext java-1.8.0-openjdk java-1.8.0-openjdk-devel libxml2-devel openssl-devel libcurl-devel net-snmp-devel libevent-devel sqlite-devel pcre-devel unixODBC-devel openldap-devel
dnf install -y mariadb-devel
subscription-manager repos --enable=codeready-builder-for-rhel-8-x86_64-rpms 
yum module enable -y virt-devel 
dnf install -y libssh2-devel OpenIPMI-devel
~~~~

### 2. Download and unzip the latest version of the source code:

~~~~
wget https://cdn.zabbix.com/stable/4.4.8/zabbix-4.4.8.tar.gz
tar -zxf zabbix-4.4.8.tar.gz
cd zabbix-4.4.8
wget https://raw.githubusercontent.com/CHERTS/zabbix_telegram_webhook/master/patch/ZBXNEXT-5554.patch
patch -p0 < ZBXNEXT-5554.patch
~~~~

### 3. Build all Zabbix components with MySQL (MariaDB) support:

~~~~
./configure --with-libpthread --with-libpcre --with-libcurl --with-libxml2 --with-net-snmp --with-openssl --enable-ipv6 --with-ssh2 --with-openipmi --with-unixodbc --with-ldap --enable-server --sysconfdir=/etc/zabbix --with-mysql
make
~~~~

### 4. After successful build, in step 3 you can use zabbix binaries, copy them to the current directory:

~~~~
cp src/zabbix_server/zabbix_server zabbix_server_mysql_v444
~~~~

Check for the presence of binary files:

~~~~
# ls -l | grep 'zabbix_'
-rwxr-xr-x.  1 root root 7140712 Oct 14 23:37 zabbix_server_mysql_v444
~~~~

Congratulations! Now you can stop your zabbix-server and replace it with this assembly.

You also need to replace 1 file in the web-frontend:
~~~~
# cp frontends/php/app/controllers/CControllerMediatypeEdit.php /your-web-frontend-root-directory/app/controllers
~~~~

# Ubuntu
## Build on Ubuntu 18.04 LTS (Bionic Beaver) with MySQL (MariaDB) support

### 1. To prepare for build on Ubuntu 18.04, you need to install additional packages:

~~~~
sudo apt-get update
sudo apt-get install -y autoconf automake gcc make wget unzip gettext default-jdk libxml2-dev libssl-dev libcurl4-openssl-dev libsnmp-dev libevent-dev libsqlite3-dev libpcre2-dev libssh2-1-dev libmariadbclient-dev-compat libopenipmi-dev unixodbc-dev libldap2-dev
~~~~

### 2. Download and unzip the latest version of the source code:

~~~~
wget https://cdn.zabbix.com/stable/4.4.8/zabbix-4.4.8.tar.gz
tar -zxf zabbix-4.4.8.tar.gz
cd zabbix-4.4.8
wget https://raw.githubusercontent.com/CHERTS/zabbix_telegram_webhook/master/patch/ZBXNEXT-5554.patch
patch -p0 < ZBXNEXT-5554.patch
~~~~

### 3. Build all Zabbix components with MySQL (MariaDB) support:

~~~~
./configure --with-libpthread --with-libpcre --with-libcurl --with-libxml2 --with-net-snmp --with-openssl --enable-ipv6 --with-ssh2 --with-jabber --with-openipmi --with-unixodbc --with-ldap --enable-server --sysconfdir=/etc/zabbix --with-mysql
make
~~~~

### 4. After successful build, in step 3 you can use zabbix binaries, copy them to the current directory:

~~~~
cp src/zabbix_server/zabbix_server zabbix_server_mysql_v444
~~~~

Check for the presence of binary files:

~~~~
# ls -l | grep 'zabbix_'
-rwxr-xr-x  1 root root 6405704 Oct  9 21:28 zabbix_server_mysql_v444
~~~~

Congratulations! Now you can stop your zabbix-server and replace it with this assembly.

You also need to replace 1 file in the web-frontend:
~~~~
# cp frontends/php/app/controllers/CControllerMediatypeEdit.php /your-web-frontend-root-directory/app/controllers
~~~~
