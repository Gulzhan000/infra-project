# Отчёт по мидтерму DevOps

Проект: midterm devops. Репозиторий: https://github.com/Gulzhan000/infra-project

В отчёте записаны фактические команды и их вывод. Невыполненные проверки отмечены отдельно.

## 1.1 Настройка и первый коммит

Репозиторий на GitHub был пустым. Клонирование по SSH не удалось: Host key verification failed. На момент первого клонирования SSH-ключ ещё не был настроен. Позже SSH настроен и проверен успешным клонированием; подробности ниже. Выполнено клонирование по HTTPS:

```text
$ git clone https://github.com/Gulzhan000/infra-project.git infra-project
Cloning into 'infra-project'...
warning: You appear to have cloned an empty repository.
```

Основные коммиты выполнены в PowerShell с Git for Windows. Для дополнительной проверки использован терминал Linux в контейнере Alpine: история, статус, игнорирование файлов и первый коммит проверены повторно; вывод приведён в конце отчёта. Отдельный пользовательский дистрибутив WSL не устанавливался.
```text
$ git --version
git version 2.55.0.windows.5
```

```text
$ git config user.name Gulzhan000

```

```text
$ git config user.email ashuubayeva_g@iuca.kg

```

```text
$ git config init.defaultBranch main

```

```text
$ git branch -m main

```

```text
$ git add .gitignore
warning: in the working copy of '.gitignore', LF will be replaced by CRLF the next time Git touches it
```

```text
$ git commit -m Добавила gitignore
[main (root-commit) 239d434] Добавила gitignore
 1 file changed, 3 insertions(+)
 create mode 100644 .gitignore
```

Первый коммит содержит только .gitignore. REPORT.md пока не включён в коммиты.


Настройки Git также установлены глобально:

```bash
git config --global user.name "Gulzhan000"
git config --global user.email "ashuubayeva_g@iuca.kg"
git config --global init.defaultBranch main
```


## 1.2 Файлы проекта

```text
$ git status --short
?? .env.example
?? README.md
?? REPORT.md
?? ansible/
?? docker/
```

```text
$ git check-ignore .env app.log
.env
app.log
```

```text
$ git add README.md .env.example
warning: in the working copy of '.env.example', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'README.md', LF will be replaced by CRLF the next time Git touches it
```

```text
$ git commit -m Добавила описание и пример env
[main 22639ba] Добавила описание и пример env
 2 files changed, 7 insertions(+)
 create mode 100644 .env.example
 create mode 100644 README.md
```

```text
$ git add docker/
warning: in the working copy of 'docker/Dockerfile', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'docker/index.html', LF will be replaced by CRLF the next time Git touches it
```

```text
$ git commit -m Добавила файлы Docker
[main 1d92dab] Добавила файлы Docker
 2 files changed, 7 insertions(+)
 create mode 100644 docker/Dockerfile
 create mode 100644 docker/index.html
```

```text
$ git add ansible/
warning: in the working copy of 'ansible/ansible.cfg', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'ansible/group_vars/all.yml', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'ansible/inventory.ini', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'ansible/playbook.yml', LF will be replaced by CRLF the next time Git touches it
```

```text
$ git commit -m Добавила конфигурацию Ansible
[main fff870e] Добавила конфигурацию Ansible
 4 files changed, 29 insertions(+)
 create mode 100644 ansible/ansible.cfg
 create mode 100644 ansible/group_vars/all.yml
 create mode 100644 ansible/inventory.ini
 create mode 100644 ansible/playbook.yml
```

Файлы проекта добавлены тремя отдельными коммитами. .env и app.log игнорируются.


## 1.3 Просмотр изменений

```text
$ git diff
warning: in the working copy of 'README.md', LF will be replaced by CRLF the next time Git touches it
diff --git a/README.md b/README.md
index 27ca4e3..27eb212 100644
--- a/README.md
+++ b/README.md
@@ -3,3 +3,5 @@
 Учебный инфраструктурный репозиторий курса

 <!-- Допишите сюда -->
+
+Проект midterm devops: учебная практика по Git, Docker и Ansible.
```

```text
$ git add README.md
warning: in the working copy of 'README.md', LF will be replaced by CRLF the next time Git touches it
```

```text
$ git diff

```

```text
$ git diff --staged
diff --git a/README.md b/README.md
index 27ca4e3..27eb212 100644
--- a/README.md
+++ b/README.md
@@ -3,3 +3,5 @@
 Учебный инфраструктурный репозиторий курса

 <!-- Допишите сюда -->
+
+Проект midterm devops: учебная практика по Git, Docker и Ansible.
```

```text
$ git commit -m Дописала описание проекта
[main 748276b] Дописала описание проекта
 1 file changed, 2 insertions(+)
```

После git add второй git diff пустой: рабочий файл совпадает с индексом. git diff --staged показывает разницу между индексом и последним коммитом.


## 2.1 История коммитов

```text
$ git log --oneline
748276b Дописала описание проекта
fff870e Добавила конфигурацию Ansible
1d92dab Добавила файлы Docker
22639ba Добавила описание и пример env
239d434 Добавила gitignore
```

```text
$ git log --oneline --graph --all
* 748276b Дописала описание проекта
* fff870e Добавила конфигурацию Ansible
* 1d92dab Добавила файлы Docker
* 22639ba Добавила описание и пример env
* 239d434 Добавила gitignore
```

```text
$ git show HEAD~1
commit fff870e43c122037e7e9773ddf7a383bb1b51ef0
Author: Gulzhan000 <ashuubayeva_g@iuca.kg>
Date:   Mon Oct 26 15:04:09 2026 +0600

    Добавила конфигурацию Ansible

diff --git a/ansible/ansible.cfg b/ansible/ansible.cfg
new file mode 100644
index 0000000..4dd424d
--- /dev/null
+++ b/ansible/ansible.cfg
@@ -0,0 +1,3 @@
+[defaults]
+inventory = inventory.ini
+host_key_checking = False
diff --git a/ansible/group_vars/all.yml b/ansible/group_vars/all.yml
new file mode 100644
index 0000000..d780f15
--- /dev/null
+++ b/ansible/group_vars/all.yml
@@ -0,0 +1,3 @@
+---
+app_port: 8080
+app_user: deploy
diff --git a/ansible/inventory.ini b/ansible/inventory.ini
new file mode 100644
index 0000000..c78d1be
--- /dev/null
+++ b/ansible/inventory.ini
@@ -0,0 +1,2 @@
+[web]
+web1 ansible_host=192.168.56.10 ansible_user=vagrant
diff --git a/ansible/playbook.yml b/ansible/playbook.yml
new file mode 100644
index 0000000..9814f7b
--- /dev/null
+++ b/ansible/playbook.yml
@@ -0,0 +1,21 @@
+---
+- name: Configure web servers
+  hosts: web
+  become: true
+
+  tasks:
+    - name: Install nginx
+      ansible.builtin.package:
+        name: nginx
+        state: present
+
+    - name: Create application user
+      ansible.builtin.user:
+        name: "{{ app_user }}"
+        shell: /bin/bash
+
+    - name: Ensure nginx is started and enabled
+      ansible.builtin.service:
+        name: nginx
+        state: started
+        enabled: true
```

HEAD~1 на момент проверки: fff870e Добавила конфигурацию Ansible. Это родитель текущего коммита.


## 2.2 Ветка и слияние

```text
$ git switch -c feature/healthcheck
Switched to a new branch 'feature/healthcheck'
```

```text
$ git add docker/Dockerfile
warning: in the working copy of 'docker/Dockerfile', LF will be replaced by CRLF the next time Git touches it
```

```text
$ git commit -m Добавила проверку работы nginx
[feature/healthcheck b897a4a] Добавила проверку работы nginx
 1 file changed, 1 insertion(+)
```

```text
$ git switch main
Switched to branch 'main'
Your branch is based on 'origin/main', but the upstream is gone.
  (use "git branch --unset-upstream" to fixup)
```

```text
$ git merge feature/healthcheck
Updating 748276b..b897a4a
Fast-forward
 docker/Dockerfile | 1 +
 1 file changed, 1 insertion(+)
```

```text
$ git branch -d feature/healthcheck
Deleted branch feature/healthcheck (was b897a4a).
```

Слияние прошло Fast-forward. Проверка здоровья теперь в main, временная ветка удалена.


## 2.3 Конфликт слияния

```text
$ git switch -c fix/app-port
Switched to a new branch 'fix/app-port'
```

```text
$ git add ansible/group_vars/all.yml
warning: in the working copy of 'ansible/group_vars/all.yml', LF will be replaced by CRLF the next time Git touches it
```

```text
$ git commit -m Изменила порт на 9090
[fix/app-port bbe29b9] Изменила порт на 9090
 1 file changed, 1 insertion(+), 1 deletion(-)
```

```text
$ git switch main
Switched to branch 'main'
Your branch is based on 'origin/main', but the upstream is gone.
  (use "git branch --unset-upstream" to fixup)
```

```text
$ git add ansible/group_vars/all.yml

```

```text
$ git commit -m Поставила порт 8000 в main
[main 811dbf5] Поставила порт 8000 в main
 1 file changed, 1 insertion(+), 1 deletion(-)
```

```text
$ git merge fix/app-port
Auto-merging ansible/group_vars/all.yml
CONFLICT (content): Merge conflict in ansible/group_vars/all.yml
Automatic merge failed; fix conflicts and then commit the result.
```

Содержимое all.yml во время конфликта:
```yaml
---
<<<<<<< HEAD
app_port: 8000
=======
app_port: 9090
>>>>>>> fix/app-port
app_user: deploy
```
```text
$ git add ansible/group_vars/all.yml
warning: in the working copy of 'ansible/group_vars/all.yml', LF will be replaced by CRLF the next time Git touches it
```

```text
$ git commit -m Разрешила конфликт порта, оставила 8000
[main b96d148] Разрешила конфликт порта, оставила 8000
```

```text
$ git log --oneline --graph --all
*   b96d148 Разрешила конфликт порта, оставила 8000
|\
| * bbe29b9 Изменила порт на 9090
* | 811dbf5 Поставила порт 8000 в main
|/
* b897a4a Добавила проверку работы nginx
* 748276b Дописала описание проекта
* fff870e Добавила конфигурацию Ansible
* 1d92dab Добавила файлы Docker
* 22639ba Добавила описание и пример env
* 239d434 Добавила gitignore
```

В итоговом файле оставлен порт 8000. Маркеры конфликта удалены, слияние записано отдельным merge-коммитом.
```text
$ git push -u origin main
To https://github.com/Gulzhan000/infra-project.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```


## 3.1 Получение веб-коммита

README.md изменён через кнопку Edit на GitHub. Коммит: «Дописала README на GitHub».

```text
$ git pull
From https://github.com/Gulzhan000/infra-project
   9ee4270..bcc6cd7  main       -> origin/main
Updating 9ee4270..bcc6cd7
Fast-forward
 README.md | 2 ++
 1 file changed, 2 insertions(+)
```
```text
$ git log --oneline -6
bcc6cd7 Дописала README на GitHub
9ee4270 Revert "Убрала логи из gitignore"
3e0ca53 Убрала логи из gitignore
461afee docs: update readme
b96d148 Разрешила конфликт порта, оставила 8000
811dbf5 Поставила порт 8000 в main
```




## 3.2 Pull Request с установкой curl

Pull Request создан и слит через GitHub: https://github.com/Gulzhan000/infra-project/pull/1 . В описании указаны изменение и его назначение. Merge-коммит: 48d8630.

```text
$ git switch main
Already on 'main'
Your branch is up to date with 'origin/main'.
```

```text
$ git pull --ff-only
Already up to date.
```

```text
$ git switch -c feature/ansible-curl
Switched to a new branch 'feature/ansible-curl'
```

```text
$ git add ansible/playbook.yml
warning: in the working copy of 'ansible/playbook.yml', LF will be replaced by CRLF the next time Git touches it
```

```text
$ git commit -m feat(ansible): install curl
[feature/ansible-curl 14cb146] feat(ansible): install curl
 1 file changed, 5 insertions(+)
```

```text
$ git push -u origin feature/ansible-curl
remote:
remote: Create a pull request for 'feature/ansible-curl' on GitHub by visiting:
remote:      https://github.com/Gulzhan000/infra-project/pull/new/feature/ansible-curl
remote:
To https://github.com/Gulzhan000/infra-project.git
 * [new branch]      feature/ansible-curl -> feature/ansible-curl
branch 'feature/ansible-curl' set up to track 'origin/feature/ansible-curl'.
```

```text
$ git switch main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
```



## 3.3 Удаление ветки

```text
$ git pull
From https://github.com/Gulzhan000/infra-project
   bcc6cd7..48d8630  main       -> origin/main
Updating bcc6cd7..48d8630
Fast-forward
 ansible/playbook.yml | 5 +++++
 1 file changed, 5 insertions(+)
```
```text
$ git branch -d feature/ansible-curl
Deleted branch feature/ansible-curl (was 14cb146).
```
```text
$ git push origin --delete feature/ansible-curl
To https://github.com/Gulzhan000/infra-project.git
 - [deleted]         feature/ansible-curl
```
```text
$ git fetch --prune

```
```text
$ git branch -a
  feature/docker-page
  fix/app-port
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/feature/docker-page
  remotes/origin/main
```


## 4.1 Страница и запуск Docker

```text
$ git switch main
Already on 'main'
Your branch is up to date with 'origin/main'.
```

```text
$ git pull --ff-only
Already up to date.
```

```text
$ git switch -c feature/docker-page
Switched to a new branch 'feature/docker-page'
```

```text
$ docker build -t infra-web:1.0 docker/
#0 building with "desktop-linux" instance using docker driver

#1 [internal] load build definition from Dockerfile
#1 transferring dockerfile: 164B 0.0s done
#1 DONE 0.1s

#2 [internal] load metadata for docker.io/library/nginx:1.27-alpine
#2 ...

#3 [auth] library/nginx:pull token for registry-1.docker.io
#3 DONE 0.0s

#2 [internal] load metadata for docker.io/library/nginx:1.27-alpine
#2 DONE 6.4s

#4 [internal] load .dockerignore
#4 transferring context: 2B done
#4 DONE 0.1s

#5 [internal] load build context
#5 transferring context: 363B 0.0s done
#5 DONE 0.1s

#6 [1/2] FROM docker.io/library/nginx:1.27-alpine@sha256:65645c7bb6a0661892a8b03b89d0743208a18dd2f3f17a54ef4b76fb8e2f2a10
#6 resolve docker.io/library/nginx:1.27-alpine@sha256:65645c7bb6a0661892a8b03b89d0743208a18dd2f3f17a54ef4b76fb8e2f2a10 0.1s done
#6 DONE 0.3s

#6 [1/2] FROM docker.io/library/nginx:1.27-alpine@sha256:65645c7bb6a0661892a8b03b89d0743208a18dd2f3f17a54ef4b76fb8e2f2a10
#6 sha256:39c2ddfd6010082a4a646e7ca44e95aca9bf3eaebc00f17f7ccc2954004f2a7d 0B / 15.52MB 0.2s
#6 sha256:34a64644b756511a2e217f0508e11d1a572085d66cd6dc9a555a082ad49a3102 0B / 1.40kB 0.2s
#6 sha256:197eb75867ef4fcecd4724f17b0972ab0489436860a594a9445f8eaff8155053 0B / 1.21kB 0.2s
#6 sha256:81bd8ed7ec6789b0cb7f1b47ee731c522f6dba83201ec73cd6bca1350f582948 0B / 402B 0.2s
#6 sha256:34a64644b756511a2e217f0508e11d1a572085d66cd6dc9a555a082ad49a3102 1.40kB / 1.40kB 1.8s done
#6 sha256:d7e5070240863957ebb0b5a44a5729963c3462666baa2947d00628cb5f2d5773 0B / 955B 0.2s
#6 sha256:81bd8ed7ec6789b0cb7f1b47ee731c522f6dba83201ec73cd6bca1350f582948 402B / 402B 2.0s done
#6 sha256:197eb75867ef4fcecd4724f17b0972ab0489436860a594a9445f8eaff8155053 1.21kB / 1.21kB 2.1s done
#6 sha256:b464cfdf2a6319875aeb27359ec549790ce14d8214fcb16ef915e4530e5ed235 0B / 629B 0.2s
#6 sha256:f18232174bc91741fdf3da96d85011092101a032a93a388b79e99e69c2d5c870 0B / 3.64MB 0.2s
#6 sha256:d7e5070240863957ebb0b5a44a5729963c3462666baa2947d00628cb5f2d5773 955B / 955B 1.5s done
#6 sha256:61ca4f733c802afd9e05a32f0de0361b6d713b8b53292dc15fb093229f648674 0B / 1.79MB 0.2s
#6 sha256:b464cfdf2a6319875aeb27359ec549790ce14d8214fcb16ef915e4530e5ed235 629B / 629B 1.7s done
#6 sha256:39c2ddfd6010082a4a646e7ca44e95aca9bf3eaebc00f17f7ccc2954004f2a7d 0B / 15.52MB 5.3s
#6 sha256:f18232174bc91741fdf3da96d85011092101a032a93a388b79e99e69c2d5c870 1.05MB / 3.64MB 3.5s
#6 sha256:61ca4f733c802afd9e05a32f0de0361b6d713b8b53292dc15fb093229f648674 1.05MB / 1.79MB 2.4s
#6 sha256:39c2ddfd6010082a4a646e7ca44e95aca9bf3eaebc00f17f7ccc2954004f2a7d 1.05MB / 15.52MB 8.9s
#6 sha256:f18232174bc91741fdf3da96d85011092101a032a93a388b79e99e69c2d5c870 2.10MB / 3.64MB 6.6s
#6 sha256:61ca4f733c802afd9e05a32f0de0361b6d713b8b53292dc15fb093229f648674 1.79MB / 1.79MB 5.4s done
#6 sha256:f18232174bc91741fdf3da96d85011092101a032a93a388b79e99e69c2d5c870 3.15MB / 3.64MB 8.3s
#6 sha256:f18232174bc91741fdf3da96d85011092101a032a93a388b79e99e69c2d5c870 3.64MB / 3.64MB 8.5s done
#6 extracting sha256:f18232174bc91741fdf3da96d85011092101a032a93a388b79e99e69c2d5c870
#6 extracting sha256:f18232174bc91741fdf3da96d85011092101a032a93a388b79e99e69c2d5c870 0.3s done
#6 extracting sha256:61ca4f733c802afd9e05a32f0de0361b6d713b8b53292dc15fb093229f648674
#6 extracting sha256:61ca4f733c802afd9e05a32f0de0361b6d713b8b53292dc15fb093229f648674 0.3s done
#6 extracting sha256:b464cfdf2a6319875aeb27359ec549790ce14d8214fcb16ef915e4530e5ed235 0.0s done
#6 extracting sha256:d7e5070240863957ebb0b5a44a5729963c3462666baa2947d00628cb5f2d5773 0.0s done
#6 extracting sha256:81bd8ed7ec6789b0cb7f1b47ee731c522f6dba83201ec73cd6bca1350f582948 0.0s done
#6 extracting sha256:197eb75867ef4fcecd4724f17b0972ab0489436860a594a9445f8eaff8155053 0.0s done
#6 extracting sha256:34a64644b756511a2e217f0508e11d1a572085d66cd6dc9a555a082ad49a3102 0.0s done
#6 sha256:39c2ddfd6010082a4a646e7ca44e95aca9bf3eaebc00f17f7ccc2954004f2a7d 2.10MB / 15.52MB 12.6s
#6 sha256:39c2ddfd6010082a4a646e7ca44e95aca9bf3eaebc00f17f7ccc2954004f2a7d 3.15MB / 15.52MB 14.9s
#6 sha256:39c2ddfd6010082a4a646e7ca44e95aca9bf3eaebc00f17f7ccc2954004f2a7d 4.19MB / 15.52MB 16.8s
#6 sha256:39c2ddfd6010082a4a646e7ca44e95aca9bf3eaebc00f17f7ccc2954004f2a7d 5.24MB / 15.52MB 19.2s
#6 sha256:39c2ddfd6010082a4a646e7ca44e95aca9bf3eaebc00f17f7ccc2954004f2a7d 6.29MB / 15.52MB 20.4s
#6 sha256:39c2ddfd6010082a4a646e7ca44e95aca9bf3eaebc00f17f7ccc2954004f2a7d 7.34MB / 15.52MB 21.0s
#6 sha256:39c2ddfd6010082a4a646e7ca44e95aca9bf3eaebc00f17f7ccc2954004f2a7d 8.39MB / 15.52MB 21.8s
#6 sha256:39c2ddfd6010082a4a646e7ca44e95aca9bf3eaebc00f17f7ccc2954004f2a7d 9.44MB / 15.52MB 23.0s
#6 sha256:39c2ddfd6010082a4a646e7ca44e95aca9bf3eaebc00f17f7ccc2954004f2a7d 10.49MB / 15.52MB 23.7s
#6 sha256:39c2ddfd6010082a4a646e7ca44e95aca9bf3eaebc00f17f7ccc2954004f2a7d 11.53MB / 15.52MB 25.2s
#6 sha256:39c2ddfd6010082a4a646e7ca44e95aca9bf3eaebc00f17f7ccc2954004f2a7d 12.58MB / 15.52MB 25.5s
#6 sha256:39c2ddfd6010082a4a646e7ca44e95aca9bf3eaebc00f17f7ccc2954004f2a7d 13.63MB / 15.52MB 25.8s
#6 sha256:39c2ddfd6010082a4a646e7ca44e95aca9bf3eaebc00f17f7ccc2954004f2a7d 14.68MB / 15.52MB 26.6s
#6 sha256:39c2ddfd6010082a4a646e7ca44e95aca9bf3eaebc00f17f7ccc2954004f2a7d 15.52MB / 15.52MB 26.8s done
#6 extracting sha256:39c2ddfd6010082a4a646e7ca44e95aca9bf3eaebc00f17f7ccc2954004f2a7d
#6 extracting sha256:39c2ddfd6010082a4a646e7ca44e95aca9bf3eaebc00f17f7ccc2954004f2a7d 0.7s done
#6 DONE 27.9s

#7 [2/2] COPY index.html /usr/share/nginx/html/
#7 DONE 0.1s

#8 exporting to image
#8 exporting layers 0.1s done
#8 exporting manifest sha256:306d8a9c2d6cda54d46936907e17a8660eb7b5782784eea1a99ec4f7c5d234a9 0.0s done
#8 exporting config sha256:85e4366aba04fc15cffd3da8ac284dd7cb8c3da820b2d0fc948eacf11a65b90a 0.0s done
#8 exporting attestation manifest sha256:045789debb851606f7fd6a2dfad878b3d26644184007baee29bbf981cc0f3298 0.0s done
#8 exporting manifest list sha256:46d6a3c278b1d2cfdc5fb9fc992db6f9242241d37b1e36d3b6dab1350d522135
#8 exporting manifest list sha256:46d6a3c278b1d2cfdc5fb9fc992db6f9242241d37b1e36d3b6dab1350d522135 0.0s done
#8 naming to docker.io/library/infra-web:1.0 done
#8 unpacking to docker.io/library/infra-web:1.0 0.0s done
#8 DONE 0.4s
```

```text
$ docker run -d --name infra-web -p 8080:80 infra-web:1.0
cf50615b462327aa3624e4e6d4c4e0e8a45fa721bfebe130eeef3df2a766f1ff
```

```text
$ curl http://localhost:8080
  % Total    % Received % Xferd  Average Speed  Time    Time    Time   Current
                                 Dload  Upload  Total   Spent   Left   Speed

  0      0   0      0   0      0      0      0                              0
100    324 100    324   0      0  67683      0                              0
100    324 100    324   0      0  65573      0                              0
100    324 100    324   0      0  63504      0                              0
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>midterm devops</title>
</head>
<body>
  <h1>midterm devops</h1>
  <p>Автор: Gulzhan000</p>
  <p>Учебный проект по Git, Docker и Ansible.</p>
</body>
</html>
```
```text
$ docker exec infra-web wget -qO- http://localhost/
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>midterm devops</title>
</head>
<body>
  <h1>midterm devops</h1>
  <p>Автор: Gulzhan000</p>
  <p>Учебный проект по Git, Docker и Ansible.</p>
</body>
</html>
```

Образ собран, контейнер запущен, curl получил страницу midterm devops. Внутри контейнера команда из HEALTHCHECK также вернула страницу.
```text
$ docker rm -f infra-web
infra-web
```

```text
$ git add docker/Dockerfile docker/index.html README.md
warning: in the working copy of 'README.md', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'docker/Dockerfile', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'docker/index.html', LF will be replaced by CRLF the next time Git touches it
```

```text
$ git commit -m feat(docker): add midterm page
[feature/docker-page 527adce] feat(docker): add midterm page
 3 files changed, 29 insertions(+), 2 deletions(-)
```

```text
$ git push -u origin feature/docker-page
remote:
remote: Create a pull request for 'feature/docker-page' on GitHub by visiting:
remote:      https://github.com/Gulzhan000/infra-project/pull/new/feature/docker-page
remote:
To https://github.com/Gulzhan000/infra-project.git
 * [new branch]      feature/docker-page -> feature/docker-page
branch 'feature/docker-page' set up to track 'origin/feature/docker-page'.
```

```text
$ git switch main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
```



## 5.1 Отмена изменений и исправление сообщения

```text
$ git diff docker/Dockerfile
diff --git a/docker/Dockerfile b/docker/Dockerfile
index e3a4bff..3d970f7 100644
--- a/docker/Dockerfile
+++ b/docker/Dockerfile
@@ -1,3 +1,4 @@
 FROM nginx:1.27-alpine
 COPY index.html /usr/share/nginx/html/
 HEALTHCHECK CMD wget -qO- http://localhost/ || exit 1
+лишняя строка
```

```text
$ git restore docker/Dockerfile

```

```text
$ git diff docker/Dockerfile

```

```text
$ git add README.md
warning: in the working copy of 'README.md', LF will be replaced by CRLF the next time Git touches it
```

```text
$ git commit -m docs: updte readme
[main 9101a5f] docs: updte readme
 1 file changed, 2 insertions(+)
```

```text
$ git commit --amend -m docs: update readme
[main 461afee] docs: update readme
 Date: Mon Oct 26 15:12:57 2026 +0600
 1 file changed, 2 insertions(+)
```

```text
$ git log -1 --oneline
461afee docs: update readme
```

git restore убрал незакоммиченную строку из Dockerfile. Сообщение последнего коммита исправлено до push; коммит с опечаткой на сервер не отправлялся.
```text
$ git push
To https://github.com/Gulzhan000/infra-project.git
   b96d148..461afee  main -> main
```



## 5.2 Отмена опубликованного коммита

```text
$ git add .gitignore
warning: in the working copy of '.gitignore', LF will be replaced by CRLF the next time Git touches it
```

```text
$ git commit -m Убрала логи из gitignore
[main 3e0ca53] Убрала логи из gitignore
 1 file changed, 1 deletion(-)
```

```text
$ git push
To https://github.com/Gulzhan000/infra-project.git
   461afee..3e0ca53  main -> main
```

```text
$ git revert --no-edit 3e0ca53c4f8b9647dd87929d3fff9a20a3e9a70b
[main 9ee4270] Revert "Убрала логи из gitignore"
 Date: Mon Oct 26 15:13:05 2026 +0600
 1 file changed, 1 insertion(+)
```

```text
$ git push
To https://github.com/Gulzhan000/infra-project.git
   3e0ca53..9ee4270  main -> main
```

```text
$ git log -2 --oneline
9ee4270 Revert "Убрала логи из gitignore"
3e0ca53 Убрала логи из gitignore
```

```text
$ git check-ignore app.log
app.log
```

revert создал новый коммит с обратным изменением: строка *.log снова на месте. История опубликованных коммитов сохранена, поэтому чужие копии репозитория не ломаются и force-push не нужен.


## Дополнение к 1.1 Проверка SSH

Создан отдельный ключ только для чтения infra-project. Ключ сервера проверяется по списку GitHub API, проверка ключа сервера не отключалась.

```text
$ git clone git@github.com:Gulzhan000/infra-project.git infra-project-ssh
Cloning into 'infra-project-ssh'...
```


## Итоговая проверка

Docker Pull Request создан и слит через веб-интерфейс: https://github.com/Gulzhan000/infra-project/pull/2 . Merge-коммит: c95510a.

```text
$ git pull
From https://github.com/Gulzhan000/infra-project
   48d8630..c95510a  main       -> origin/main
Updating 48d8630..c95510a
Fast-forward
 README.md         | 18 ++++++++++++++++++
 docker/Dockerfile |  1 +
 docker/index.html | 12 ++++++++++--
 3 files changed, 29 insertions(+), 2 deletions(-)
```

```text
$ git branch -d feature/docker-page
Deleted branch feature/docker-page (was 527adce).
```

```text
$ git push origin --delete feature/docker-page
To https://github.com/Gulzhan000/infra-project.git
 - [deleted]         feature/docker-page
```

```text
$ git branch -d fix/app-port
Deleted branch fix/app-port (was bbe29b9).
```

```text
$ git fetch --prune

```

```text
$ git branch -a
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
```

```text
$ git log --oneline --graph --all
*   c95510a Merge pull request #2 from Gulzhan000/feature/docker-page
|\
| * 527adce feat(docker): add midterm page
* |   48d8630 Merge pull request #1 from Gulzhan000/feature/ansible-curl
|\ \
| * | 14cb146 feat(ansible): install curl
| |/
* / bcc6cd7 Дописала README на GitHub
|/
* 9ee4270 Revert "Убрала логи из gitignore"
* 3e0ca53 Убрала логи из gitignore
* 461afee docs: update readme
*   b96d148 Разрешила конфликт порта, оставила 8000
|\
| * bbe29b9 Изменила порт на 9090
* | 811dbf5 Поставила порт 8000 в main
|/
* b897a4a Добавила проверку работы nginx
* 748276b Дописала описание проекта
* fff870e Добавила конфигурацию Ansible
* 1d92dab Добавила файлы Docker
* 22639ba Добавила описание и пример env
* 239d434 Добавила gitignore
```

```text
$ git check-ignore .env app.log
.env
app.log
```

```text
$ git show --stat 239d434f5f3197e69cbd3cd0894cf84222e3cc83
commit 239d434f5f3197e69cbd3cd0894cf84222e3cc83
Author: Gulzhan000 <ashuubayeva_g@iuca.kg>
Date:   Mon Oct 26 15:04:08 2026 +0600

    Добавила gitignore

 .gitignore | 3 +++
 1 file changed, 3 insertions(+)
```

```text
$ git diff --check

```

Проверено: первый коммит содержит только .gitignore; .env и app.log не отслеживаются; EXPOSE 80, HEALTHCHECK, задача curl и раздел запуска есть в main; маркеров конфликта нет; опубликованного коммита с updte нет. Временные ветки удалены.

Проверка в терминале Linux (Alpine в Docker):

```sh
set -eu
git config --global --add safe.directory /git
git config --global core.autocrlf true
git --version
git status --short --branch
git log --oneline --graph --all
git check-ignore .env app.log
git show --stat $(git rev-list --max-parents=0 HEAD)
```

```text
git version 2.54.0

## main...origin/main
?? REPORT.md
*   c95510a Merge pull request #2 from Gulzhan000/feature/docker-page
|\
| * 527adce feat(docker): add midterm page
* |   48d8630 Merge pull request #1 from Gulzhan000/feature/ansible-curl
|\ \
| * | 14cb146 feat(ansible): install curl
| |/
* / bcc6cd7 Дописала README на GitHub
|/
* 9ee4270 Revert "Убрала логи из gitignore"
* 3e0ca53 Убрала логи из gitignore
* 461afee docs: update readme
*   b96d148 Разрешила конфликт порта, оставила 8000
|\
| * bbe29b9 Изменила порт на 9090
* | 811dbf5 Поставила порт 8000 в main
|/
* b897a4a Добавила проверку работы nginx
* 748276b Дописала описание проекта
* fff870e Добавила конфигурацию Ansible
* 1d92dab Добавила файлы Docker
* 22639ba Добавила описание и пример env
* 239d434 Добавила gitignore
.env
app.log
commit 239d434f5f3197e69cbd3cd0894cf84222e3cc83
Author: Gulzhan000 <ashuubayeva_g@iuca.kg>
Date:   Mon Oct 26 15:04:08 2026 +0600

    Добавила gitignore

 .gitignore | 3 +++
 1 file changed, 3 insertions(+)
```


## 5.3 Контрольные вопросы

1. **Что такое коммит и зачем нужен индекс?** Коммит сохраняет выбранное состояние файлов вместе с автором и сообщением. Индекс — место подготовки: через git add выбираются изменения, которые войдут в следующий коммит.
2. **Что такое ветка?** Ветка — имя, указывающее на коммит. Она позволяет работать над изменениями отдельно, а потом объединить их с основной версией.
3. **Что такое HEAD?** HEAD указывает на текущее положение в репозитории, обычно на активную ветку. Через него Git понимает, с какого коммита продолжается работа.
4. **Чем fetch отличается от pull?** fetch получает данные с сервера и обновляет ссылки на удалённые ветки, не объединяя их с моей веткой. pull сначала получает изменения, а затем включает их в текущую ветку слиянием или rebase, в зависимости от настроек.
5. **Почему нельзя добавлять .env?** В .env могут храниться пароли и токены. Если отправить такой файл в репозиторий, секреты попадут в историю; одного удаления файла потом недостаточно. Поэтому .env игнорируется, а для примера используется .env.example без секретов.
