# Docker

Appsmith can be deployed locally or on your private instance using docker. 
To simplify installation, Appsmith comes with an installation script that will download all of the necessary dependencies and help you configure Appsmith.

Appsmith 可以使用 docker 部署在地端或是你的私有環境之中.  
為了簡化安裝程序, Appsmith 提供了安裝腳本幫助你安裝相關的依賴與設定 Appsmith. 

**支援以下作業系統**

* macOS
* Ubuntu
* OpenSuse
* Debian
* CentOS

{% hint style="warning" %}
對 Mac　來說, [Docker Desktop](https://docs.docker.com/docker-for-mac/install/) 是必要的. 對於其它的作業系統, Docker 將會透過腳本自動化安裝.
{% endhint %}

{% hint style="warning" %}
For Mac on M1, we've seen problems with Docker getting into a corrupt state and throwing errors like `error creating overlay mount`. Please follow [Docker's troubleshooting guide](https://docs.docker.com/docker-for-mac/troubleshoot/) to resolve and then proceed with the installation.
{% endhint %}

{% hint style="danger" %}
Appsmith does not officially support Windows \(or WSL\). Please try out one of our [pre-built images](../) if your production operating system is not on the list above.
{% endhint %}

在你想部署 appsmith 的系統下載安裝腳本 **install.sh** 

```bash
# Downloads install.sh
curl -O https://raw.githubusercontent.com/appsmithorg/appsmith/master/deploy/install.sh
```

設定腳本為可執行

```bash
chmod +x install.sh
```

執行腳本. **不允要執行 sudo & 並且確定 ports 80 & 443 並沒有被其它程序佔用**. 如果你需要在其它的 ports 執行 appsmith [請參考此指引](../../troubleshooting-guide/deployment-errors.md#ports-unavailable)

```bash
./install.sh
```

確認所有的 containers 是否運作順利.

```bash
docker ps

#Output should look like this
CONTAINER ID        IMAGE                             COMMAND                  CREATED             STATUS              PORTS                                      NAMES
3b8f2c9638d0        appsmith/appsmith-editor          "/bin/sh -c 'while :…"   17 minutes ago      Up 17 minutes       0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp   appsmith_nginx_1
f5a365aada1c        appsmith/appsmith-server:latest   "/bin/sh -c /entrypo…"   17 minutes ago      Up 17 minutes       0.0.0.0:8080->8080/tcp                     appsmith_appsmith-internal-server_1
84b15adf470a        mongo                             "docker-entrypoint.s…"   17 minutes ago      Up 17 minutes       0.0.0.0:27017->27017/tcp                   appsmith_mongo_1
09b42d5b0f1a        redis                             "docker-entrypoint.s…"   17 minutes ago      Up 17 minutes       0.0.0.0:6379->6379/tcp                     appsmith_redis_1
90833ba6d75a        certbot/certbot                   "/bin/sh -c 'trap ex…"   17 minutes ago      Up 17 minutes       80/tcp, 443/tcp                            appsmith_certbot_1
```

## Common Issues

You can debug common errors faced during deployment at the link below

{% page-ref page="../../troubleshooting-guide/deployment-errors.md" %}

## Updating to the latest release

Appsmith auto-updates using Watchtower so you do not need to manually update your appsmith installation. In the odd event that you do, you can run the following command in the installation directory.

```text
#!/bin/sh
sudo su
docker-compose pull && docker-compose rm -fsv appsmith-internal-server nginx && docker-compose up -d
```

## Disable Signup

To disable users from signing up on your self hosted instance, set the below env variables in your docker.env file

```text
APPSMITH_SIGNUP_DISABLED=true
APPSMITH_ADMIN_EMAILS=YOUR_ADMIN_EMAIL
```

Restart docker and Nginx using the following command

```text
sudo docker-compose rm -fsv appsmith-internal-server nginx && sudo docker-compose up -d
```

This will disable allowing users other than those specified in the `APPSMITH_ADMIN_EMAILS` field from signing up on your appsmith instance. Other users can only signup once they have been invited by an organization administrator.

{% hint style="success" %}
The signup page will continue to show up but will throw an error when a user tries to sign up.
{% endhint %}

{% hint style="info" %}
Learn about more options to restrict signup, including allowing emails with specific domains to signup at the page on [Restricting Sign-up](../../how-to-guides/restricting-signup.md).
{% endhint %}

## Enabling Services for Self Hosting

Appsmith ships with third-party services that improve the app building experience. All third-party services are entirely optional.

{% hint style="success" %}
All third party services are enabled by default in our [cloud-hosted](https://appsmith.com) version.
{% endhint %}

Our self-hosted version allows you to configure your own keys to enable third party services such as Google OAuth. To enable a service, simply open the folder of your Appsmith installation and edit the **docker.env** file.

Each service requires a specific key or configuration to be enabled. Configure the service of your choice using the following guides

* [Email](email/)
* [Google OAuth](google-login.md)
* [Github OAuth](github-login.md)
* [Google Maps](google-maps.md)

You may need root access to modify the docker.env file.

Restart docker and Nginx using the following command

```text
sudo docker-compose rm -fsv appsmith-internal-server nginx && sudo docker-compose up -d
```

{% page-ref page="../../tutorial-1/" %}
