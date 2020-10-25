# Server setup
## Requirements
  $ sudo apt-get update
  
  $ sudo apt-get install git make default-libmysqlclient-dev zlib1g-dev libpcre3-dev mysql-server
  
  $ sudo apt install build-essential
  > For gcc & g++
  
  > Below comands might not be needed
  $ echo "deb http://ftp.us.debian.org/debian unstable main contrib non-free" >> /etc/apt/sources.list.d/unstable.list
  
  $ apt-get install -t unstable gcc-5

  $ apt-get install -t unstable g++-5

  $ ln -s /usr/bin/gcc-5 /usr/bin/gcc

  $ ln -s /usr/bin/g++-5 /usr/bin/g++

## Project set up
Clone rathena project
$ git clone git@github.com:motizukilucas/rathena.git

Generate make file
  $ ./configure
> --enable-packetver=YYYYMMDD for configuring it to your client version
> --enable-prere=yes for classic

Run server compiler
  $ make server
> run make clean, if you've made any modifications then make server

Give below servers a+x permissions
$ chmod a+x login-server && chmod a+x char-server && chmod a+x map-server

## MySQL configuration
Secure mysql instalation
$ mysql_secure_installation

Create database
  $ create database ragzinho;

Create user
create user 'ragzinho'@'localhost' identified by 'secret';

Give permisions to user
GRANT ALL PRIVILEGES ON ragzinho . * TO 'ragzinho'@'localhost';

Do this because you need to
update `login` set `userid` = "ragzinho", `user_pass` = md5("secret") where `account_id` = 1;

Import database
mysql -u ragzinho -p ragzinho < /ragzinho/sql-files/main.sql

## Configuring rAthena
Access /conf/inter_athena.conf and fill up with your data
> All the defaults are set to ragnarok

### Go to conf/
#### char_athena.conf
  server_name: ragzinho
  login_ip: 127.0.0.1
  char_ip: 138.59.122.12

  userid: ragzinho
  passwd: secret

  pincode_enabled: no
  pincode_force: no

#### inter_athena.conf
  login_server_id: ragzinho
  login_server_pw: secret
  login_server_db: ragzinho

  ipban_db_id: ragzinho
  ipban_db_pw: secret
  ipban_db_db: ragzinho

  char_server_id: ragzinho
  char_server_pw: secret
  char_server_db: ragzinho

  map_server_id: ragzinho
  map_server_pw: secret
  map_server_db: ragzinho

  log_db_id: ragzinho
  log_db_pw: secret
  log_db_db: ragzinho

#### map_athena.conf
  char_ip: 127.0.0.1
  bind_ip: 0.0.0.0
  map_ip: 138.59.122.12

  userid: ragzinho
  passwd: secret

#### login_athena.conf
  use_MD5_passwords: yes
  bind_ip: 0.0.0.0
> The 0.0.0.0 may not be needed
> Change 127.0.0.1 for WAN ip if configuring VPS

## Opening ports
  $ sudo iptables -A INPUT -p udp --dport 6900 -m state --state NEW -j ACCEPT
  $ sudo iptables -A INPUT -p udp --dport 5121 -m state --state NEW -j ACCEPT
  $ sudo iptables -A INPUT -p udp --dport 6121 -m state --state NEW -j ACCEPT
  $ sudo iptables -A INPUT -p tcp --dport 6900 -m state --state NEW -j ACCEPT
  $ sudo iptables -A INPUT -p tcp --dport 5121 -m state --state NEW -j ACCEPT
  $ sudo iptables -A INPUT -p tcp --dport 6121 -m state --state NEW -j ACCEPT

## Bugs
This commit was causing problems in Izlude Criatura Academy
git revert ec2f02796fa3a7ece41f40bda936467a964fd7a1
> https://github.com/rathena/rathena/commit/ec2f02796fa3a7ece41f40bda936467a964fd7a1

# Client setup
Download kro


Download 2015 client

copy data/ System/ folder and guildtip and tipoftheday from translation to client folder

run NEMO select client and recommended config
  - Find "Skip licence screen" and make it green
  - Find "Use Ragnarok icon" and make it green
  - Find "Custom Window Title" here you can write name of your RO client (whatever you want) 

Open GRF editor
look for clientinfo.xml
<address>138.59.122.12</address>
<yellow>
  <admin>2000000</admin>
</yellow>

<?xml version="1.0" encoding="euc-kr" ?>
<clientinfo>
	<desc>Private Server Description</desc>
	<servicetype>korea</servicetype>
	<servertype>primary</servertype>
	<connection>
		<display>ServerName</display>
      	<address>127.0.0.1</address>
      	<port>6900</port>
      	<version>55</version>
      	<langtype>0</langtype>
		<registrationweb>www.ragnarok.com</registrationweb>
		<loading>
			<image>loading00.jpg</image>
			<image>loading01.jpg</image>
			<image>loading02.jpg</image>
			<image>loading03.jpg</image>
			<image>loading04.jpg</image>
		</loading>
		<yellow>
			<admin>2000000</admin>
		</yellow>
   	</connection>
</clientinfo>

creating gm account
insert into `login` (account_id, userid, user_pass, sex, group_id) values (2000000, "gm", md5("secret"), "M", 99);
