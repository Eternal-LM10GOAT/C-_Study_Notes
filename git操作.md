# Git操作

## git配置

**在windows或linux上下载git客户端；**

```
linux下使用如下命令:

sudo apt install git
sudo apt install openssh-client
```

**创建github账号；**

**在git bash中输入以下命令生成秘钥;**

```
ssh-keygen -t rsa -C "注册github的邮箱地址"
```

**添加公钥到 GitHub：**

- 打开生成的公钥文件（如 `id_rsa.pub`），复制其中的内容。
- 登录到 GitHub，进入 Settings -> SSH and GPG keys -> New SSH key。
- 将复制的公钥内容粘贴到 Key 字段中，并填写一个标题（如 `My Laptop`）。
- 点击 Add SSH key 完成添加。

**在git bash中输入以下命令验证是否通信成功;**

```
ssh -T git@github.com
```

**在git bash中配置用户名和邮箱，用于以后修改代码时显示在git log中;**

```
git config --global user.name "用户名"
git config --global user.email "邮箱"
```

**使用git clone将远程仓库克隆到本地仓库；**

## git命令

```
git status
```

- 用于显示工作目录和暂存区的状态。它可以帮助你了解当前仓库的状态，包括哪些文件被修改、哪些文件被暂存、哪些文件未被跟踪等。

```
git pull
```

- 拉取最新的代码，便于在此基础上进行下一步开发工作。

### 提交操作

```
git add <file-name>
```

- 使用【file-name】表示将指定改动过的file从**工作区添加到暂存区**
- 使用【        **.**        】表示将工作区所有改动过的文件添加到暂存区

```
git commit -m "提交注释"
```

- 将暂存区的文件添加到本地仓库

```
 git log
```

- 提交后，你可以使用 `git log` 查看提交历史，包括提交的哈希值、作者、日期和提交信息，只能查看当前本地仓库中提交的历史记录。它只显示本地分支上的提交，而不会自动获取或显示远程仓库的最新修改

```
git push origin
git push origin main //本地的 main 分支推送到远程的 main 分支。
git push origin feature:new-feature //本地的 feature 分支推送到远程的 new-feature 分支
```

- 将本地仓库【指定分支】推送到远程仓库【指定分支】

### 回退操作

```
git checkout -- <file_name>
```

- 使用【file-name】表示将指定代码从本地仓库进行回退
- 使用【        **.**        】表示将所有代码从本地仓库进行回退

```
git reset HEAD <file_name>
```

- 将git add之后暂存区的内容全部取消

```
git reset --hard [commit-id]
```

- 用于重置当前分支的 HEAD 指针、暂存区和工作区，使它们与指定的提交（通常是当前分支的最新提交或指定的某个提交）完全一致。
- **`commit`**：可选参数，取某次commit的哈希码，表示将 HEAD 重置到指定的提交。如果不提供，则默认重置到当前分支的最新提交（即 `HEAD` 指向的提交）。

- **`--hard`**：这个选项告诉 Git 同时重置 HEAD 指针、暂存区和工作区。
- **数据丢失风险**：`git reset --hard` 是一个危险的命令，因为它会丢弃所有未暂存的更改，并且无法恢复（除非你使用了其他备份手段，如 `git reflog` 或其他版本控制工具的备份）。
- **影响范围**：这个命令会影响当前分支的所有文件，不仅仅是那些你明确修改过的文件。

```
git reflog 
git reset --hard [commit-id]
```

- 当我们重置了HEAD指针后后悔了，想回到之前的操作，可以先使用reflog查看历史commit-id在进行reset操作。

```
git push -f origin master
```

- 只有本地仓库版本领先远程仓库时才能使用git push命令推送代码，而当版本落后时则需要git push -f强制将代码推送到远程仓库。

- 当我们发现我们推送到远程仓库的代码有问题时，此时有两个办法补救：

  - 第一种：将代码修改好后再推送一次。

  - 第二种：使用git reset --hard [commit-id] 将回退到指定版本进行修改，此时本地仓库版本落后远程仓库版本是无法推送的，只用强制推送。

  - **千万注意该命令很危险，使用前一定要注意，因为我们是回退到某个版本进行修改后再进行提交的，所以如果其他人在我们修改时也提交了代码，那么当我们强制推送时会将别人的代码弄丢！！！**

  - 强制推送前先进行检查，在确定没有他人提交时才能强制推送

    ```
    git fetch origin    //获取远程更新，这将更新本地对远程分支的引用，但不会合并到本地分支。
    git log origin/main --oneline //查看远程分支的提交记录
    git log origin/main ..main --oneline //查看本地分支与远程分支之间的差异
    ```

### git提交代码冲突问题

```
git pull  //开发前先拉取代码
```

- 发生冲突有两种情况：
  - 第一种：开发者A和B对**同一文件**的**不同位置**进行修改，A先提交，B后提交会发现【rejected】，此时我们可以使用git pull先拉取代码到最新的版本，此时git会**自动merge**我们的代码，之后再进行提交。
  - 第二种：开发者A和B对**同一文件**的**相同位置**进行修改，A先提交，B后提交会发现【rejected】，此时我们可以使用git pull先拉取代码到最新的版本，但是git并**不会自动merge**代码，我们需要手动解决冲突之后再进行提交。

### 分支

```
git branch
```

-   不带参数   ：查看本地分支
- 【-a】参数  ：查看本地分支和远程分支
- 【-r】参数   ：查看远程分支
-   带分支名    ：创建分支但不切换到该分支

```
git branch --vv
```

- 显示本地分支的详细信息，包括每个分支是否跟踪远程分支、远程分支的最新提交以及本地分支与远程分支之间的差异。

- ```
  * master  1a2b3c4 [origin/master] Commit message for master
    feature 5d6e7f8 [origin/feature: behind 2] Commit message for feature
    bugfix  9a8b9c0 [origin/bugfix: ahead 3, behind 1] Commit message for bugfix
  ```

  1. 分支名：
     - 如 `master`、`feature`、`bugfix`。
     - 当前所在的分支会用 `*` 标记，例如 `* master`。
  2. 最新提交的哈希值：
     - 如 `1a2b3c4`、`5d6e7f8`。
     - 表示该分支的最新提交的哈希值。
  3. 跟踪信息：
     - `[origin/master]`：表示该分支正在跟踪远程分支 `origin/master`。
     - 如果没有 `[ ]` 中的信息，说明该分支没有设置上游分支。
  4. 与远程分支的差异：
     - `[ahead 3]`：表示本地分支比远程分支多了 3 个提交。
     - `[behind 2]`：表示本地分支比远程分支少了 2 个提交。
     - `[ahead 3, behind 1]`：表示本地分支比远程分支多了 3 个提交，但少了 1 个提交。
  5. 提交信息：
     - 如 `Commit message for master`，表示该分支的最新提交信息。

```
git checkout -b 分支名
```

- 创建分支并切换到该分支

```
git checkout 分支名
```

- 切换到该分支

### 分支推送

当我们处于新建的分支LocalBranch上进行开发，在完成开发任务要推送时，若直接使用**git push origin main**是无法推送到远程仓库的，此时有两种方法：

```
git push origin LocalBranch:main
```

```
git checkout main  //切换到本地main分支
git pull //拉取最新代码
git merge LocalBranch //将本地分支LocalBranch代码合并到本地main分支
```

分支合并后可以使用如下命令删除分支

```
git branch -d LocalBranch  //合并分支后删除分支
```

```
git branch -D LocalBranch  //当没有合并分支时强制删除分支
```

### 分支冲突

两位开发者A和B对同一文件进行操作，开发者A执行如下操作

```
git pull  //拉取最新的代码
git checkout -b fixbug //创建并切换到fixbug分支
对文件a进行修改...
```

开发者B执行如下操作

```
git pull //拉取最新的代码
直接在本地main分支上对文件a进行修改
git add .
git commit -m "...."
git push origin main //推送到远端仓库
```

随后开发者A修改完文件a后执行如下操作

```
git checkout main //切换回main分支
git pull //拉取最新的代码
git merge fixbug //将fixbug分支合并到main分支
```

此时如果A和B改动的是同一块地方就会出现冲突，如果改的不是一块地方则会自动合并，此时就要手动解决冲突。

### 远程分支管理

```
git checkout -b <本地分支名> <远程仓库名>/<远程分支名> 	//创建本地分支并追踪指定的远程分支
git branch -u <远程仓库名>/<远程分支名> 		//将本地已存在的分支设置追踪某个远程分支
```

当我们在本地创建一个分支但是没有关联远程分支时，切换到该本地分支并使用git pull并不能成功拉取到代码，因为`git pull` 命令不知道应该从哪个远程分支拉取更新。

```
git checkout -b dev origin/dev  //创建本地分支dev并追踪远程分支dev
git pull //拉取远程dev分支的代码到本地dev分支
进行开发工作...
git add .
git commit -m "dev分支进行开发"
git push origin dev
```

在dev分支上的代码修改并不影响main分支

## 实际开发

项目代码远程仓库的分支命名规则：

```
master：主干分支		
dev:开发分支
release:发布分支
```

个人开发本地仓库分支命名规则：

```
feature:特性分支
bugfix:缺陷修复分支
hotfix:热更新分支
```

实际开发流程

```
git checkout -b feature/mydev origin/dev  //创建本地分支feature/mydev并关联远程分支dev
git pull  //拉取远程dev分支代码到本地（经常使用git pull保持本地仓库与远程仓库一致性是比较好的习惯）
在本地分支feature/mydev上开发代码后（git add xxx   git commit -m "xxx"）
```

4、将本地分支推送到远程分支，注意并不是直接推送到dev分支上（因为写的代码需要经过评审）

```
git pull //提送代码之前要拉一下代码保持两个仓库的一直性
git push origin feature/mydev:dev //代码还没有经过评审不能这样做
```

```
git push origin feature/mydev //在远程仓库中新建feature/mydev的代码分支并将本地feature/mydev分支的代码推送到远程分支

git push origin feature/mydev:feature/mydev_v1.0 //在远程仓库中新建feature/mydev_v1.0 的代码分支并将本地feature/mydev分支的代码推送到远程分支
```

该远程分支feature/mydev 或 feature/mydev_v1.0 上需要使用代码评审工具 gitlab 、gerrit进行评审才能合并到dev分支，我们需要申请MR，**在代码托管平台上创建 MR的方法：**

```
GitLab：
登录到你的 GitLab 项目。
点击左侧菜单中的 "Merge Requests"。
点击 "New merge request"。
选择 源分支（你的分支）和 目标分支（通常是 main 或 master）。
填写标题和描述，说明你的修改内容。
点击 "Create merge request"。
```

```
GitHub：
登录到你的 GitHub 项目。
点击左侧菜单中的 "Pull requests"。
点击 "New pull request"。
选择 基准分支（目标分支，例如 main）和 比较分支（你的分支）。
填写标题和描述，说明你的修改内容。
点击 "Create pull request"。
```

等待审核和合并

- 提交 MR 后，项目的维护者会审核你的代码。
- 如果需要修改，你可以根据反馈继续更新代码，并再次提交到你的分支。
- 审核通过后，维护者会将你的分支合并到目标分支。

删除个人推送的远程分支

```
git push origin [使用空格来覆盖远程分支]:feature/mydev_v1.0
git branch -d feature/mydev    删除本地分支
```

## 本地仓库关联远程仓库

### **1. 在 GitHub 创建远程仓库**

1. 登录 GitHub，点击右上角 **+** → **New repository**。
2. 输入仓库名称（与本地仓库名一致或自定义）。
3. **不要初始化 README**（避免冲突），直接点击 **Create repository**。

------

### **2. 本地仓库关联远程仓库**

在本地仓库的终端中执行以下命令：

#### **步骤 1：添加远程仓库地址**

```bash
bash复制代码

git remote add origin https://github.com/你的用户名/仓库名.git
```

- 替换 `你的用户名` 和 `仓库名` 为实际值。
- 仓库 URL 可在 GitHub 仓库页面 → **Code** → **HTTPS** 或 **SSH** 获取。

#### **步骤 2：验证远程仓库**

```bash
bash复制代码

git remote -v
```

应显示类似：

```
origin  https://github.com/你的用户名/仓库名.git (fetch)
origin  https://github.com/你的用户名/仓库名.git (push)
```

------

### **3. 推送本地代码到 GitHub**

#### **首次推送（需关联分支）**

```bash
git push -u origin main  # GitHub 默认分支为 main
# 或（如果本地分支是 master）
git push -u origin master
```

- `-u` 参数将本地分支与远程分支关联，后续可直接用 `git push`。

#### **后续推送**

```bash
bash复制代码

git push
```

------

### **常见问题**

1. 权限问题：

   - **HTTPS**：推送时需输入 GitHub 账号密码。
   - **SSH**：需提前[配置 SSH 密钥](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)。

2. 冲突处理：

   - 如果 GitHub 已存在文件（如初始化 README），需先执行：

     ```bash
     bash复制代码
     
     git pull origin main --allow-unrelated-histories
     ```

------

### **完整流程示例**

```bash
# 进入本地仓库目录
cd /path/to/your/local/repo
 
# 添加远程仓库
git remote add origin https://github.com/yourname/repo.git
 
# 推送代码
git push -u origin main
```

完成后，刷新 GitHub 页面即可看到同步的代码。
