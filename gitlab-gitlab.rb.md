```
prometheus_monitoring['enable'] = false
external_url "http://gitlab.hyx123.cn"

# 邮件配置

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



```
