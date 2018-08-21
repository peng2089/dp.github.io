
# CentOS之vsftpd安装配置

1. 更新源,关闭selinux

2. 安装vsftpd

    ```bash
    \$ yum install vsftpd
    ```

3. 配置

    ```bash
    anonymous_enable=NO # 是否允许匿名用户访问(YES-允许,NO-不允许)
    local_enable=YES # 设定本地用户可以访问。注：如使用虚拟宿主用户，在该项目设定为NO的情况下所有虚拟用户将无法访问。
    chroot_list_enable=YES # 限定用户不可以离开主目录

    local_enable=YES/NO 本地用户是否可以访问 注：如果为NO 则所有虚拟用户都将不能访问原因：虚拟用户访问在主机上其实是以本地用户访问的

    xferlog_file=/var/log/vsftpd.log # 设定vsftpd的服务日志保存路径。注意，该文件默认不存在。必须要手动touch出来
    ascii_upload_enable=YES # 允许使用ASCII模式上传
    ascii_download_enable=YES # 设定支持ASCII模式的上传和下载功能。
    pam_service_name=vsftpd # PAM认证文件名。PAM将根据/etc/pam.d/vsftpd进行认证

    #以下这些是关于Vsftpd虚拟用户支持的重要CentOS FTP服务配置项目。默认vsftpd.conf中不包含这些设定项目，需要自己手动添加RHEL/CentOS FTP服务配置。
    guest_enable=YES #设定启用虚拟用户功能。
    guest_username=ftp #指定虚拟用户的宿主用户。-RHEL/CentOS中已经有内置的ftp用户了（注：此用户在chroot_list_file=/etc/vsftpd/chroot_list文件里所指定的用户）
    user_config_dir=/etc/vsftpd/vuser_conf #设定虚拟用户个人vsftp的RHEL/CentOS FTP服务文件存放路径。存放虚拟用户个性的FTP服务文件(配置文件名=虚拟用户名)

    virtual_use_local_privs=YES # 让虚用户获得本地用户权限
    chmod_enable=YES # 开启chmod命令

    use_localtime=YES|NO # 默认为NO。YES，VSFTPD显示目录列表时使用你本地时区的时间。默认是显示GMT时间。同样，由ftp命令“MDTM”返回的时间值也受此选项影响。
    ```

4. 创建chroot list，将ftp用户加入其中

    ```bash
    touch /etc/vsftpd/chroot_list
    echo ftp >> /etc/vsftpd/chroot_list
    ```

5. 创建用户密码文本, 注意奇行是用户名，偶行是密码

    ```bash
    \$ vi /etc/vsftpd/vuser_passwd.txt
    test
    123456
    ```

6. 生成认证文件

    ```bash
    \$ db_load -T -t hash -f /etc/vsftpd/vuser_passwd.txt /etc/vsftpd/vuser_passwd.db
    ```

7. 编辑认证文件/etc/pam.d/vsftpd
    > 全部注释掉原来语句,再增加以下两句,注意要确认pam_userdb.so的位置.

    ```bash
    \$ vi /etc/pam.d/vsftpd
    auth required /usr/lib64/security/pam_userdb.so db=/etc/vsftpd/vuser_passwd
    account required /usr/lib64/security/pam_userdb.so db=/etc/vsftpd/vuser_passwd
    ```
8. 创建虚拟用户个性RHEL/CentOS FTP服务文件

    ```bash
    \$ mkdir /etc/vsftpd/vuser_conf/
    \$ vi /etc/vsftpd/vuser_conf/test
    local_root=/data/ftp/test
    write_enable=YES
    download_enable=YES
    anon_umask=022
    anon_world_readable_only=NO
    anon_upload_enable=YES
    anon_mkdir_write_enable=YES
    anon_other_write_enable=YES
    listen_port=21
    ```

9. 创建ftp文件夹并配置权限，注意:根目录不能有写权限.

    ```bash
    \$ mkdir -p /data/ftp/test
    \$ chown ftp.ftp -R /data/ftp/test
    \$ chmod 775 -R /data/ftp/test
    ```

10. 防火墙开启端口

    ```bash
    \$ firewall-cmd --add-service=ftp --permanent
    \$ firewall-cmd --reload
    \$ systemctl restart firewalld
    ```