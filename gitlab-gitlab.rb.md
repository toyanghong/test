
### 宿主机配置路径  /srv/gitlab/config# vi gitlab.rb

```
#prometheus_monitoring['enable'] = true
#letsencrypt['enable'] = true
external_url "http://gitlab.hyx123.cn"
#letsencrypt['contact_emails'] = ['344454075@qq.com']

gitlab_rails['smtp_enable'] = true
gitlab_rails['smtp_address'] = "smtp.qq.com"
gitlab_rails['smtp_port'] = 465
gitlab_rails['smtp_user_name'] = "344454075@qq.com"
gitlab_rails['smtp_password'] = ""
gitlab_rails['smtp_domain'] = "smtp.qq.com"
gitlab_rails['smtp_authentication'] = "login"
gitlab_rails['smtp_enable_starttls_auto'] = true
gitlab_rails['smtp_tls'] = true
gitlab_rails['smtp_openssl_verify_mode'] = 'peer'
gitlab_rails['gitlab_email_from'] = '344454075@qq.com'

### 备份gitlab路径

gitlab_rails['backup_path'] = '/var/opt/gitlab/backups'


```
