# everything
# GitLab配置

## 安装

1. 正常安装步骤
    - 安装软件包及解决依赖项
    - 系统用户
    - Ruby环境
    - Go
    - 数据库(Mysql/Postgresql)
    - Redis
    - Gitlab-CE
    - Nginx


2. 我们使用gitlab官方写的安装脚本进行安装

 
    [https://www.gitlab.cc/downloads/#ubuntu1604](https://www.gitlab.cc/downloads/#ubuntu1604)

    a. 安装配置依赖项

        ```
        sudo apt-get install curl openssh-server ca-certificates postfix
        ```

    b. 添加GitLab仓库,并安装到服务器上
        ```
        curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash
        sudo apt-get install gitlab-ce
        ```

    c. 启动GitLab
        ```
        sudo gitlab-ctl reconfigure
        ```


## 邮箱配置
配置SMTP发送邮件配置，使用163邮箱。
        ```
        $ sudo vi /etc/gitlab/gitlab.rb

        #Sending application email via SMTP
        gitlab_rails['smtp_enable'] = true
        gitlab_rails['smtp_address'] = "smtp.163.com"
        gitlab_rails['smtp_port'] = 25 
        gitlab_rails['smtp_user_name'] = "xxuser@163.com"
        gitlab_rails['smtp_password'] = "xxpassword"
        gitlab_rails['smtp_domain'] = "163.com"
        gitlab_rails['smtp_authentication'] = :login
        gitlab_rails['smtp_enable_starttls_auto'] = true
 
        ##修改gitlab配置的发信人
        gitlab_rails['gitlab_email_from'] = "xxuser@163.com"
        user["git_user_email"] = "xxuser@163.com"
        ```

## 域名配置
### 配置


PS：使用动态域名解析（目前服务商是花生壳）


1. 修改etc/gitlab/gitlab.rb里面两行

    ```
    external_url "域名:9000" #这里的域名是我们最终要使用的域名
    unicorn[‘port‘] = 9001
    ```

2. 在路由器中将服务器ip设为10.0.0.6

3. 在路由器上添加端口映射
    - 将 9000端口映射到10.0.0.6:9000（电信服务商将80端口封掉了，只能使用非80端口进行http访问）
    - 将 22 端口映射到10.0.0.6:22（ssh使用的端口）


然后将我名最终使用的域名CNAME到花生壳提供的壳域名

### 遇到的问题
1. h3c路由器端口映射只能针对外网网卡的ip来做，而我们路由器的ip地址是不固定的（是固定的我们就不需要DDns了）
![image](http://i2.piimg.com/567571/06dd2e0db4e54706.png)
![image](http://i2.piimg.com/567571/9d21b06c2750d678.png)
2. 
