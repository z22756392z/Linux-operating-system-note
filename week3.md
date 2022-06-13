enpos3 --interface-- talk to network ip address : 08:00:27

enp0s8 --interface-- talk to window ip address : 192.168...



Remote controll application:

Putty:[Download PuTTY: latest release (0.76) (greenend.org.uk)](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)



WINSCP :upload the file from window to linux OR download Linux to window

## Connect to linux

use enp0s8 's ip address (which is the interface talk to host)to connect to linux

#### Putty

![image-20220301094408312](./img/putty.png)

#### WinSCP

![image-20220301094443154](./img/winscp.png)

### Problem

There might be some reason which cause us unable to connect to vm.

Here are few list.

*  FireWall

  **command**:

  ``systemctl status firewalld``:check firewall

  ``systemctl stop firewalld``:close(for now) firewall: 

  ``systemctl disable firewall``: disable firewall (next time we login it will not active automatically)

  

  After closing firewall you might  not still unable to connect.

  Try close SELinux(security-enhanced linux)

* SELinux

  [getenforce命令_半吊子炼全栈的博客-CSDN博客_getenforce](https://blog.csdn.net/weixin_45861372/article/details/115362262)

  **command:**

  ``getenforce``:  get the SElinux state-> Enforcing or Permissive

  ``setenforce 0/1``: changes isn't permanent,we need to edit /etc/selinux/config

  ``/etc/selinux/config``:

  ![image-20220301100909236](./img/selinux_config.png)

  use ``gedit`` to edit the file, edit the SELINUX to enforcing、permissive or disabled  and then,reboot. The changes will stay permanently.

## Set up website

install software:

redhat, redora ,centos  ->yum

ubuntu ->apt

Linux strcture:

![](./img/strut.png)

All distribution kernel is same(version might differ), the difference between them are their shell or APP.



Since we are using centos , we use yum to install software.(before we install check for network whether is  connected)

![image-20220301102032094](./img/install_http.png)



After httpd installed, we use ```systemctl status httpd ```to check if we sucessfully install.

![image-20220301102717677](./img/http_status.png)

And use ``systemctl start httpd`` to setup server.

![image-20220301102747104](./img/image-20220301102747104.png)

The server is enabled.

We use ``ifconfig`` to look up IP address in en0ps8 which the interface used to talk to window(host).

Our IP address is 192.168.56.101, we search for 192.168.56.101 in host browser. 

![image-20220301103139702](./img/image-20220301103139702.png)



### Edit website

``vim /etc/httpd/conf/httpd.conf`` : edit httpd configuration

after we modify restart

``systemctl restart httpd``

We edit file in host  and then send it to vm using WinSCP(unable connect to linux see above,firewall,selinux).

![image-20220301110635862](./img/image-20220301110635862.png)

``mv web.htm /var/www/html``

we move the file in  Putty(unable to connect to linux also see above)

![image-20220301110836540](./img/image-20220301110836540.png)

Then we can connect to website in host using enp0s8(interface used to talk to host) ip address in linux

![image-20220301111009530](./img/image-20220301111009530.png)



We can also directly change the content of file in Putty.

Using ``echo "content" > web.htm``

``cat web.htm``: (concatenate )see the content of file 

![image-20220301112653532](./img/image-20220301112653532.png)

![image-20220301112705310](./img/image-20220301112705310.png)



Currently the website we setup others can't see because are in private network.



## Set up http server using python





### Connect to ipv6 website

example: http://**[**2001:b400:e757:3dc5:cdfd:d051:8826:4da2**]**:8888/hi.htm

但是這樣太難使用 所以把ipv6 address轉成domain name

[Free dynamic DNS for IPv6 (dynv6.com)](https://dynv6.com/)

example: username.dynv6.net:port/file

``curl http://127.0.0.1:6666``: to test whether the server is successfully setup.



