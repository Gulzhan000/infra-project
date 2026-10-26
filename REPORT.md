# Отчёт по мидтерму DevOps

Проект: midterm devops

Репозиторий: https://github.com/Gulzhan000/infra-project

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

Клонирование прошло успешно. Основную работу выполняла в PowerShell с Git for Windows, а в конце дополнительно проверила репозиторий из Linux-контейнера Alpine.

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

## 2.1 История коммитов

Посмотрела историю и предыдущий коммит:

```bash
git log --oneline
git log --oneline --graph --all
git show HEAD~1
```

На момент проверки HEAD~1 соответствовал коммиту fff870e «Добавила конфигурацию Ansible». HEAD~1 — это родитель текущего коммита.

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

## 3.1 Получение изменений с сервера

Открыла README.md на GitHub, добавила строку об авторе через редактор сайта и сохранила коммит «Дописала README на GitHub». Затем получила изменение локально:

```bash
git pull
git log --oneline -6
```

В локальном README появилась новая строка, а в истории — коммит, сделанный через сайт.

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

## 3.3 Уборка веток

После слияния удалила ветку локально и на GitHub, затем обновила список удалённых веток:

```bash
git branch -d feature/ansible-curl
git push origin --delete feature/ansible-curl
git fetch --prune
git branch -a
```

Проверила, что feature/ansible-curl больше нет ни локально, ни на сервере.

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

## Проверка перед сдачей

Проверила историю, список файлов и ветки. Первый коммит содержит только .gitignore, файлы .env и app.log не отслеживаются. В main есть HEALTHCHECK, EXPOSE 80, задача установки curl и инструкция запуска Docker. Оба Pull Request слиты. Временные ветки удалены, локальная main совпадает с GitHub.

Историю, статус, игнорирование файлов и первый коммит дополнительно проверила из Linux-контейнера Alpine.

## 5.3 Контрольные вопросы

1. **Что такое коммит и зачем нужен индекс?** Коммит сохраняет выбранные изменения в истории. Через индекс я выбираю, какие изменения войдут в следующий коммит, добавляя их командой git add.

2. **Что такое ветка в Git?** Это указатель на коммит. В отдельной ветке можно работать над своей задачей, а потом объединить результат с main.

3. **Что такое HEAD?** HEAD показывает текущее положение в репозитории. Обычно он указывает на ветку, в которой я сейчас работаю.

4. **Чем git fetch отличается от git pull?** fetch скачивает изменения с сервера, но не объединяет их с текущей веткой. pull получает изменения и сразу включает их в текущую ветку через слияние или rebase, в зависимости от настроек.

5. **Почему .env нельзя добавлять в репозиторий?** В нём могут быть пароли и токены. После отправки они останутся в истории, даже если потом удалить файл. Поэтому .env добавляют в .gitignore, а в .env.example оставляют только пример настроек без секретов.
