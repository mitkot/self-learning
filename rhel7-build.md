# How to build RHEL7

AMI: RHEL-7.4_HVM-20180103-x86_64-2-Hourly2-GP2 (ami-eb50cd8d)

````
$ sudo su -
# yum -y install rpm-build
# yum -y install kernel-devel
# yumdownloader --source kernel
# rpm -Uvh kernel-3.10.0-693.17.1.el7.src.rpm
# rpmbuild -bb /root/rpmbuild/SPECS/kernel.spec

````