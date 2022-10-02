# Installing Ansible 

To be able to install ansible on a RHEL8+ machine you need to have the right repo in place.

<pre>
[root@host ~]# subscription-manager register

[root@host ~]# subscription-manager repos --enable ansible-2-for-rhel-8-x86_64-rpms
</pre>

And now to the installation part.

<pre>
[kristofer@fedora ~]$ sudo yum install ansible
</pre>

<pre>
[kristofer@fedora ~]$ ansible --version
ansible 2.9.15
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/student/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3.6/site-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.6.8 (default, Mar 18 2021, 08:58:41) [GCC 8.4.1 20200928 (Red Hat 8.4.1-1)]
</pre>

<pre>
[kristofer@fedora ~]$ ansible -m setup localhost | grep ansible_python_version
</pre>