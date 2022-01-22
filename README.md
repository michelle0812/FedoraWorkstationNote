# FedoraWorkstationNote
about Installion of FedoraWorkstation

sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux <br>
ssh-keygen -t rsa<br>
cat  ~/.ssh/id_rsa.pub | ssh root@140.114.64.29 "cat - >> ~/.ssh/authorized_keys" ;\ <br>
scp root@140.114.64.29:/root/Fedora-Firewalld-Basic-Rules /root ;\ <br>
sh /root/Fedora-Firewalld-Basic-Rules ;\ <br>
echo 'alias reload="firewall-cmd --reload ; firewall-cmd --list-all"' >> /root/.bash_profile ;\ <br>
echo 'alias all="firewall-cmd --list-all"' >> /root/.bash_profile ;\ <br>
echo 'alias ens="ifconfig | grep inet"' >> /root/.bash_profile ;\ <br>
echo 'alias 29="ssh root@140.114.64.29"' >> /root/.bash_profile ;\ <br>
echo 'alias ap="ssh adminTW@192.168.245.253"' >> /root/.bash_profile ;\ <br>
echo '"\e[A": history-search-backward' >> /root/.inputrc ;\ <br>
echo '"\e[B": history-search-forward' >> /root/.inputrc <br>
<p>
dnf -y install tigervnc-server ;\ <br>
echo ":1=andy" >> /etc/tigervnc/vncserver.users ;\ <br>
echo "securitytypes=vncauth,tlsvnc" >> /home/andy/.vnc/config ;\ <br>
echo "session=gnome" >> /home/andy/.vnc/config ;\ <br>
echo "geometry=1680x1050" >> /home/andy/.vnc/config ;\ <br>
chown -R andy:andy /home/andy/.vnc ; sudo - andy ;\ <br>
vncpasswd ;\ <br>
<p>
yum install libreswan xl2tpd -y ;\ <br>
firewall-cmd --zone=public --add-port=500/udp --permanent ;\ <br>
firewall-cmd --zone=public --add-port=1701/udp --permanent ;\ <br>
firewall-cmd --zone=public --add-port=4500/udp --permanent ;\ <br>
firewall-cmd --zone=public --add-rich-rule 'rule family=ipv4 source address="192.168.253.0/24" accept' --permanent ;\ <br>
firewall-cmd --zone=public --add-masquerade --permanent ;\ <br>
reload ;\ <br>
