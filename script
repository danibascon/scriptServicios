#!/usr/bin/python
# -+- coding: utf-8 .*.

import os
import commands
import sys
import getpass

uid=[]
uid=commands.getoutput("slapcat | grep '^uid:'")

CN = raw_input('Dime nombre y apellidos: ')
commands.getoutput("echo "+CN+" > a")
commands.getoutput("base64 a > cn")
cn = commands.getoutput("cat cn")

SN = raw_input('Dime el primer apellido: ')
commands.getoutput("echo "+SN+" > b")
commands.getoutput("base64 b > sn")
sn = commands.getoutput("cat sn")

UID = raw_input('Dime el uid: ')

while UID in uid:
	UID = raw_input( "Ese uid ya existe, di otro distinto: ")


contra = getpass.getpass("Dime el passwd: ")
commands.getoutput("slappasswd -s "+contra+" > ssha")
ssha = commands.getoutput("cat ssha")


num = int(commands.getoutput('slapcat |grep -c uidNumber')) + 2000

commands.getoutput("echo 'dn: uid='"+UID+"',ou=People,dc=bascon,dc=gonzalonazareno,dc=org' > genuser.ldif")
commands.getoutput("echo 'objectClass: top' >> genuser.ldif")
commands.getoutput("echo 'objectClass: posixAccount' >> genuser.ldif")
commands.getoutput("echo 'objectClass: inetOrgPerson' >> genuser.ldif")
commands.getoutput("echo 'objectClass: person' >> genuser.ldif")
commands.getoutput("echo 'cn:: '"+cn+" >> genuser.ldif")
commands.getoutput("echo 'uid: '"+UID+" >> genuser.ldif")
commands.getoutput("echo 'uidNumber: '"+str(num)+" >> genuser.ldif")
commands.getoutput("echo 'gidNumber: 2000' >> genuser.ldif")
commands.getoutput("echo 'homeDirectory: /home/nfs/'"+UID+" >> genuser.ldif")
commands.getoutput("echo 'loginShell: /bin/bash' >> genuser.ldif")
commands.getoutput("echo 'userPassword: '"+ssha+" >> genuser.ldif")
commands.getoutput("echo 'sn:: '"+sn+" >> genuser.ldif")
commands.getoutput("echo 'mail: '"+SN+"'@hotmail.es' >> genuser.ldif")
commands.getoutput("echo 'givenName: '"+SN+" >> genuser.ldif")


commands.getoutput("rm -r uid a b cn sn ssha")


print "Enter LDAP Password:"
commands.getoutput("ldapadd -x -D 'cn=admin,dc=bascon,dc=gonzalonazareno,dc=org' -W -f genuser.ldif ")


commands.getoutput("sed -i'.bak' '$d' /etc/apache2/sites-available/www.conf")

ultima=["Alias /"+UID+" /home/nfs/"+UID+"\n",
		"	<Directory /home/nfs/"+UID+"/> \n",
		"       	Options +Indexes +SymLinksIfOwnerMatch \n",
		"       	AllowOverride None\n",
		"       	Require all granted\n",
		"	</Directory>\n",
		"</VirtualHost>\n"]

f=open('/etc/apache2/sites-available/www.conf','a')
f.writelines(ultima)
f.close()


commands.getoutput("service apache2 restart")

