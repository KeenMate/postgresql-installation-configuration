# Installation of PostgreSQL 13 on RockyLinux 9 with Apache AGE compilation

> Apache AGE is a graph database extension that can be only run on PostgreSQL 11-13 (for now)
> More info: https://age.apache.org/age-manual/master/index.html

> Based on various sources
> - https://www.tecmint.com/install-postgresql-rocky-linux/
> - Rocky Linux Minimal installation is used

> This installation is for AGE compilation and extraction of compiled outputs of AGE that can be put on production server

## Installation and initialization

```
sudo dnf install -y tar wget

sudo dnf module list postgresql
sudo dnf install https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm -y
sudo dnf update -y
sudo dnf -qy module disable postgresql
sudo dnf install -y postgresql13-server

# INSTALL DEVELOPMENT HEADERS AND LIBRARIES
sudo dnf install -y epel-release
sudo dnf --enablerepo=crb install -y perl-IPC-Run
sudo dnf install -y postgresql13-devel.x86_64 redhat-rpm-config flex bison perl-FindBin perl-lib
sudo dnf install -y ccache

psql -V
sudo /usr/pgsql-13/bin/postgresql-13-setup initdb
```

## Get Apache AGE

```
wget https://github.com/apache/age/releases/download/PG13%2Fv1.3.0-rc0/apache-age-1.3.0-src.tar.gz
tar -xzf apache-age-1.3.0-src.tar.gz
cd apache-age-1.3.0
```

## Install Apache AGE

> Update password in SQL script below
```
export PATH=$PATH:/usr/pgsql-13/bin
make install
```


## Transfer output files to Production server

AGE files are in two folders `/usr/pgsql-13/lib` and `/usr/pgsql-13/share/extension`.

From `/usr/pgsql-13/lib` transfer these files and folders
```
./bitcode
age.so
```

From  `/usr/pgsql-13/share` transfer these files
```
age.control
age--1.3.0.sql
```

After you transfer these files to Production server don't forget to reset permissions and ownership


