### 检查gitlab应用
sudo gitlab-rake gitlab:backup:create --trace

```bash
# Tasks: TOP => gitlab:backup:registry:create
sudo mkdir /var/opt/gitlab/gitlab-rails/shared/registry
sudo chown git /var/opt/gitlab/gitlab-rails/shared/registry
```

##### 停止相关数据连接服务
```bash
sudo gitlab-ctl stop unicorn
sudo gitlab-ctl stop sidekiq
```

##### 从备份中恢复
```bash
sudo gitlab-rake gitlab:backup:restore BACKUP=***
```

##### 启动Gitlab
```bash
sudo gitlab-ctl start unicorn
sudo gitlab-ctl start sidekiq
```

##### 访问项目时 500
覆盖 gitlab 的 db_key_base(/etc/gitlab/gitlab-secrets.json)

sudo gitlab-rails runner "Project.where.not(import_url: nil).each { |p| p.import_data.destroy if p.import_data }"
