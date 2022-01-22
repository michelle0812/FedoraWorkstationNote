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
<p>
cat  ~/.ssh/id_rsa.pub | ssh root@192.168.245.1 "cat - >> ~/.ssh/authorized_keys" ;\ <br>
mv /etc/ipsec.conf /etc/ipsec.conf.bsd ;\ <br>
mv /etc/ipsec.secrets /etc/ipsec.secrets.bsd ;\ <br>
mv /etc/sysctl.conf /etc/sysctl.conf.bsd ;\ <br>
mv /etc/xl2tpd/xl2tpd.conf /etc/xl2tpd/xl2tpd.conf.bsd ;\ <br>
mv /etc/ppp/chap-secrets /etc/ppp/chap-secrets.bsd ;\ <br>
scp root@192.168.245.1:/etc/ipsec.conf /etc/ ;\ <br>
scp root@192.168.245.1:/etc/ipsec.secrets /etc/ ;\ <br>
scp root@192.168.245.1:/etc/sysctl.conf /etc/ ;\ <br>
scp root@192.168.245.1:/etc/xl2tpd/xl2tpd.conf /etc/xl2tpd ;\ <br>
scp root@192.168.245.1:/etc/ppp/chap-secrets /etc/ppp ;\ <br>
sysctl -p ;\ <br>
systemctl enable --now ipsec ;\ <br>
ipsec verify ;\ <br>
echo "logfile /var/log/xl2tpd.log" >> /etc/ppp/options.xl2tpd ;\ <br>
systemctl enable --now xl2tpd ; systemctl status xl2tpd ;\ <br>
/usr/sbin/xl2tpd -D ;\ for test <br>

dnf -y install rp-pppoe ; pppoe-setup ;\
echo '#!/bin/bash' >> /etc/rc.d/rc.local ;\ <br>
echo "nohup /root/frp/frpc -c /root/frp/frpc.ini &" >> /etc/rc.d/rc.local ;\ <br>
echo "aria2c --conf-path=/root/Aria2/Aria2.conf -D" >> /etc/rc.d/rc.local ;\ <br>
echo "sleep 10 ; /usr/sbin/pppoe-start" >> /etc/rc.d/rc.local ;\ <br>
echo "sleep 10" >> /etc/rc.d/rc.local ;\ <br>
echo "/usr/sbin/route add -host 140.114.63.7 gw 192.168.245.254" >> /etc/rc.d/rc.local ;\ <br>
echo "/usr/sbin/route add -host 140.114.64.29 gw 192.168.245.254" >> /etc/rc.d/rc.local ;\ <br>
echo "[Install]" >> /lib/systemd/system/rc-local.service ;\ <br>
echo "WantedBy=multi-user.target" >> /lib/systemd/system/rc-local.service ;\ <br>
chmod +x /etc/rc.d/rc.local ;\ <br>
systemctl enable --now rc-local.service ; systemctl status rc-local.service ;\ <br>
