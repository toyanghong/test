## git上传流程

- 设置全局参考
```
Git global setup
git config --global user.name "Administrator"
git config --global user.email "@qq.com"

Create a new repository
git clone git@gitlab..cn:root/testgit.git
cd testgit
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master

Push an existing folder
cd existing_folder
git init
git remote add origin http://gitlab..cn/root/testgit.git
git add .
git commit -m "Initial commit"
git push -u origin master

Push an existing Git repository
cd existing_repo
git remote rename origin old-origin
git remote add origin git@gitlab..cn:root/testgit.git
git push -u origin --all
git push -u origin --tags

```

- http上传首次需要输入密码 用`git config --system --unset credential.helper`重置
密码。注意与SSH区别不需要gitlab上设置密钥

- 查看远程提交git url  `git remote -v`

- 更改git url  git `remote set-url origin http://gitlab..cn/root/testgit.git`



