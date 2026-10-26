# Отчёт по мидтерму DevOps

задание: midterm devops

## 1.1 Настройка и первый коммит

Проверила Git и настроила имя, почту и основную ветку:

```bash
git --version
git config --global user.name "Gulzhan000"
git config --global user.email "ashuubayeva_g@iuca.kg"
git config --global init.defaultBranch main
```

Создала пустой репозиторий на GitHub. Сначала склонировала его по HTTPS, потому что SSH ещё не был настроен:

```bash
git clone https://github.com/Gulzhan000/infra-project.git infra-project
```

Создала .gitignore и добавила в него .env, *.log и *.retry. Первым коммитом сохранила только этот файл.

```bash
git add .gitignore
git commit -m "Добавила gitignore"
```

Позже настроила отдельный SSH-ключ с правом чтения только этого репозитория и проверила клонирование по SSH:

```bash
git clone git@github.com:Gulzhan000/infra-project.git infra-project-ssh
```

Клонирование прошло успешно. Сначала работала в PowerShell с Git for Windows. Затем повторила упражнения с Git и сборку и запуск Docker в Linux-контейнере Alpine. Результаты повторной практики приведены перед контрольными вопросами; опубликованную историю при этом не переписывала.

### Фрагменты выполнения

```text
$ git --version
git version 2.55.0.windows.5
```
```text
$ git commit -m Добавила gitignore
[main (root-commit) 239d434] Добавила gitignore
 1 file changed, 3 insertions(+)
 create mode 100644 .gitignore
```
```text
$ git clone git@github.com:Gulzhan000/infra-project.git /tmp/infra-project
Cloning into '/tmp/infra-project'...
```

## 1.2 Файлы проекта

Скопировала стартовые файлы в репозиторий. Создала .env с учебным паролем из задания и app.log с тестовым текстом.

```bash
git status --short
git check-ignore .env app.log
```

Убедилась, что .env и app.log игнорируются. Файлы проекта сохранила тремя отдельными коммитами: сначала README.md и .env.example, затем docker и ansible.

```bash
git add README.md .env.example
git commit -m "Добавила описание и пример env"
git add docker/
git commit -m "Добавила файлы Docker"
git add ansible/
git commit -m "Добавила конфигурацию Ansible"
git push -u origin main
```

Изменения отправлены на GitHub. Файлы .env и app.log в репозиторий не попали.

### Фрагменты выполнения

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
$ git commit -m Добавила описание и пример env
[main 22639ba] Добавила описание и пример env
 2 files changed, 7 insertions(+)
 create mode 100644 .env.example
 create mode 100644 README.md
```
```text
$ git commit -m Добавила файлы Docker
[main 1d92dab] Добавила файлы Docker
 2 files changed, 7 insertions(+)
 create mode 100644 docker/Dockerfile
 create mode 100644 docker/index.html
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
```text
$ git push -u origin main
To https://github.com/Gulzhan000/infra-project.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

## 1.3 Просмотр изменений

Добавила в README описание проекта и сравнила изменения до и после добавления файла в индекс:

```bash
git diff
git add README.md
git diff
git diff --staged
git commit -m "Дописала описание проекта"
```

До git add было видно добавленное описание. После git add обычный git diff ничего не показал, потому что файл уже совпадал с индексом. Подготовленное изменение было видно через git diff --staged.

### Фрагменты выполнения

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

## 2.1 История коммитов

Посмотрела историю и предыдущий коммит:

```bash
git log --oneline
git log --oneline --graph --all
git show HEAD~1
```

На момент проверки HEAD~1 соответствовал коммиту fff870e «Добавила конфигурацию Ansible». HEAD~1 — это родитель текущего коммита.

### Фрагменты выполнения

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
Начало вывода git show HEAD~1; ниже в исходном выводе шли изменения файлов Ansible.

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
```

## 2.2 Ветка и слияние

Создала ветку для проверки работы nginx. В конце Dockerfile добавила HEALTHCHECK CMD wget -qO- http://localhost/ || exit 1.

```bash
git switch -c feature/healthcheck
git add docker/Dockerfile
git commit -m "Добавила проверку работы nginx"
git switch main
git merge feature/healthcheck
git branch -d feature/healthcheck
```

Слияние прошло как Fast-forward: указатель main переместился на новый коммит, отдельный merge-коммит не понадобился. Проверка работы nginx осталась в main, а временную ветку удалила.

### Фрагменты выполнения

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

## 2.3 Конфликт слияния

В ветке fix/app-port изменила app_port на 9090, а в main ту же строку изменила на 8000. Оба изменения сохранила отдельно.

```bash
git switch -c fix/app-port
git add ansible/group_vars/all.yml
git commit -m "Изменила порт на 9090"
git switch main
git add ansible/group_vars/all.yml
git commit -m "Поставила порт 8000 в main"
git merge fix/app-port
```

Возник конфликт, потому что в двух ветках была изменена одна и та же строка. В файле появились маркеры конфликта с двумя вариантами порта. Я оставила 8000, удалила маркеры и завершила слияние:

```bash
git add ansible/group_vars/all.yml
git commit -m "Разрешила конфликт порта, оставила 8000"
git log --oneline --graph --all
```

В истории появился merge-коммит. В итоговом all.yml остался один вариант порта без маркеров конфликта.

### Фрагменты выполнения

```text
$ git merge fix/app-port
Auto-merging ansible/group_vars/all.yml
CONFLICT (content): Merge conflict in ansible/group_vars/all.yml
Automatic merge failed; fix conflicts and then commit the result.
```
Содержимое all.yml с маркерами конфликта:

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
$ git commit -m Разрешила конфликт порта, оставила 8000
[main b96d148] Разрешила конфликт порта, оставила 8000
```

## 3.1 Получение изменений с сервера

Открыла README.md на GitHub, добавила строку об авторе через редактор сайта и сохранила коммит «Дописала README на GitHub». Затем получила изменение локально:

```bash
git pull
git log --oneline -6
```

В локальном README появилась новая строка, а в истории — коммит, сделанный через сайт.

### Фрагменты выполнения

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

## 3.2 Pull Request

В отдельной ветке добавила в ansible/playbook.yml задачу установки curl через ansible.builtin.package со state: present.

```bash
git switch -c feature/ansible-curl
git add ansible/playbook.yml
git commit -m "feat(ansible): install curl"
git push -u origin feature/ansible-curl
```

Создала [Pull Request №1](https://github.com/Gulzhan000/infra-project/pull/1). В описании объяснила, что curl нужен для проверки ответа веб-сервера из терминала. Слила ветку в main через GitHub и получила результат локально:

```bash
git switch main
git pull
```

Задача установки curl теперь есть в main. Сообщение коммита оформлено в формате Conventional Commits.

### Фрагменты выполнения

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

## 3.3 Уборка веток

После слияния удалила ветку локально и на GitHub, затем обновила список удалённых веток:

```bash
git branch -d feature/ansible-curl
git push origin --delete feature/ansible-curl
git fetch --prune
git branch -a
```

Проверила, что feature/ansible-curl больше нет ни локально, ни на сервере.

### Фрагменты выполнения

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

## 4.1 Сборка и запуск Docker

Начала работу в отдельной ветке:

```bash
git switch main
git pull --ff-only
git switch -c feature/docker-page
```

В docker/index.html сделала страницу «midterm devops» и указала автора Gulzhan000. В Dockerfile после COPY добавила EXPOSE 80.

```bash
docker build -t infra-web:1.0 docker/
docker run -d --name infra-web -p 8080:80 infra-web:1.0
curl http://localhost:8080
```

Образ собрался успешно. Контейнер запустился, а curl вернул HTML моей страницы. Дополнительно проверила команду из HEALTHCHECK внутри контейнера — страница тоже открылась. После проверки удалила контейнер:

```bash
docker exec infra-web wget -qO- http://localhost/
docker rm -f infra-web
```

Добавила в README раздел «Запуск в Docker» с командами сборки, запуска, проверки и удаления контейнера.

```bash
git add docker/Dockerfile docker/index.html README.md
git commit -m "feat(docker): add midterm page"
git push -u origin feature/docker-page
```

Создала и слила через GitHub [Pull Request №2](https://github.com/Gulzhan000/infra-project/pull/2). После git pull страница, EXPOSE 80 и инструкция запуска появились в локальной main.

### Фрагменты выполнения

Последние строки повторной сборки в Linux:

```text
$ docker build -t infra-web:1.0 docker/
Step 2/4 : COPY index.html /usr/share/nginx/html/
 ---> 689267c56229
Step 3/4 : EXPOSE 80
 ---> Running in 883c842ce141
 ---> Removed intermediate container 883c842ce141
 ---> 61711cb13546
Step 4/4 : HEALTHCHECK CMD wget -qO- http://localhost/ || exit 1
 ---> Running in e7589ccd7cb8
 ---> Removed intermediate container e7589ccd7cb8
 ---> d25b9dabed5f
Successfully built d25b9dabed5f
Successfully tagged infra-web:1.0
```
Ответ curl из Linux; индикатор загрузки опущен:

```text
$ curl --fail --retry 10 --retry-connrefused --retry-delay 1 http://localhost:8080
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
$ docker rm -f infra-web
infra-web
```
```text
$ git commit -m feat(docker): add midterm page
[feature/docker-page 527adce] feat(docker): add midterm page
 3 files changed, 29 insertions(+), 2 deletions(-)
```

## 5.1 Отмена изменений и исправление коммита

Добавила лишнюю строку в Dockerfile и отменила её:

```bash
git diff -- docker/Dockerfile
git restore docker/Dockerfile
git diff -- docker/Dockerfile
```

Файл вернулся к сохранённому состоянию. Затем дописала README и специально сделала опечатку в сообщении коммита. До отправки на GitHub исправила её:

```bash
git add README.md
git commit -m "docs: updte readme"
git commit --amend -m "docs: update readme"
git push
```

На сервер попал только коммит с исправленным сообщением.

### Фрагменты выполнения

```text
$ git restore docker/Dockerfile
```
```text
$ git diff docker/Dockerfile
```
```text
$ git commit --amend -m docs: update readme
[main 461afee] docs: update readme
 Date: Mon Oct 26 15:12:57 2026 +0600
 1 file changed, 2 insertions(+)
```

## 5.2 Отмена опубликованного коммита

Удалила строку *.log из .gitignore, сохранила изменение и отправила его на GitHub. Затем отменила этот коммит:

```bash
git add .gitignore
git commit -m "Убрала логи из gitignore"
git push
git revert --no-edit 3e0ca53
git push
git check-ignore app.log
```

Строка *.log вернулась в .gitignore, и app.log снова игнорируется. В истории сохранились и исходное изменение, и его отмена. Для опубликованного коммита использовала revert, чтобы не переписывать общую историю и не делать force-push.

### Фрагменты выполнения

```text
$ git log -2 --oneline
9ee4270 Revert "Убрала логи из gitignore"
3e0ca53 Убрала логи из gitignore
```
```text
$ git check-ignore app.log
app.log
```

## Проверка перед сдачей

Проверила историю, список файлов и ветки. Первый коммит содержит только .gitignore, файлы .env и app.log не отслеживаются. В main есть HEALTHCHECK, EXPOSE 80, задача установки curl и инструкция запуска Docker. Оба Pull Request слиты. Временные ветки удалены, локальная main совпадает с GitHub.

Историю, статус, игнорирование файлов и первый коммит также проверила из Linux-контейнера Alpine.

## Повторная практика в Linux

Для работы использовала оболочку sh в Linux-контейнере Alpine. По SSH склонировала репозиторий во временную папку и от первого коммита повторила добавление файлов, diff до и после индекса, создание ветки, fast-forward, конфликт слияния, restore, amend и revert. Эти повторные коммиты остались во временной копии: основную историю на GitHub не заменяла. Сборку образа и получение страницы curl тоже повторила в Linux.

```text
$ uname -s
Linux
```
```text
$ git --version
git version 2.54.0
```
```text
$ docker --version
Docker version 29.9.2, build a82e528901260c8df6073311379eac65f92db204
```
```text
$ git merge feature/healthcheck
Updating 32133a8..a4a1811
Fast-forward
 docker/Dockerfile | 1 +
 1 file changed, 1 insertion(+)
```
```text
$ git merge fix/app-port
Auto-merging ansible/group_vars/all.yml
CONFLICT (content): Merge conflict in ansible/group_vars/all.yml
Automatic merge failed; fix conflicts and then commit the result.
```
```text
$ cat ansible/group_vars/all.yml
---
<<<<<<< HEAD
app_port: 8000
=======
app_port: 9090
>>>>>>> fix/app-port
app_user: deploy
```
```text
$ git restore docker/Dockerfile
```
```text
$ git commit --amend -m docs: update readme
[main 7a3dbea] docs: update readme
 Date: Mon Oct 26 09:39:55 2026 +0000
 1 file changed, 2 insertions(+)
```
```text
$ git -c safe.directory=/project branch -a
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
```

В повторной Linux-практике слияние прошло Fast-forward, конфликт был исправлен, лишняя строка отменена, а сообщение коммита исправлено до публикации. Итоговая проверка исходного репозитория показала только main; временные ветки отсутствуют.

## 5.3 Контрольные вопросы

1. **Что такое коммит и зачем нужен индекс?** Коммит сохраняет выбранные изменения в истории. Через индекс я выбираю, какие изменения войдут в следующий коммит, добавляя их командой git add.

2. **Что такое ветка в Git?** Это указатель на коммит. В отдельной ветке можно работать над своей задачей, а потом объединить результат с main.

3. **Что такое HEAD?** HEAD показывает текущее положение в репозитории. Обычно он указывает на ветку, в которой я сейчас работаю.

4. **Чем git fetch отличается от git pull?** fetch скачивает изменения с сервера, но не объединяет их с текущей веткой. pull получает изменения и сразу включает их в текущую ветку через слияние или rebase, в зависимости от настроек.

5. **Почему .env нельзя добавлять в репозиторий?** В нём могут быть пароли и токены. После отправки они останутся в истории, даже если потом удалить файл. Поэтому .env добавляют в .gitignore, а в .env.example оставляют только пример настроек без секретов.
