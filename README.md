# lab02
## Laboratory work II

Данная лабораторная работа посвещена изучению систем контроля версий на примере **Git**.

```bash
$ open https://git-scm.com
```

## Tasks

- [ ] 1. Создать публичный репозиторий с названием **lab02** и с лиценцией **MIT**
- [ ] 2. Сгенирировать токен для доступа к сервису **GitHub** с правами **repo**
- [ ] 3. Ознакомиться со ссылками учебного материала
- [ ] 4. Выполнить инструкцию учебного материала
- [ ] 5. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```sh
$ export GITHUB_USERNAME=<имя_пользователя>
$ export GITHUB_EMAIL=<адрес_почтового_ящика>
$ export GITHUB_TOKEN=<сгенирированный_токен>
$ alias edit=<nano|vi|vim|subl>
```

```sh
$ cd ${GITHUB_USERNAME}/workspace
$ source scripts/activate
```

```sh
$ mkdir ~/.config
$ cat > ~/.config/hub <<EOF
github.com:
- user: ${GITHUB_USERNAME}
  oauth_token: ${GITHUB_TOKEN}
  protocol: https
EOF
$ git config --global hub.protocol https
```

```sh
$ mkdir projects/lab02 && cd projects/lab02
$ git init
$ git config --global user.name ${GITHUB_USERNAME}
$ git config --global user.email ${GITHUB_EMAIL}
# check your git global settings
$ git config -e --global
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab02.git
$ git pull origin master
$ touch README.md
$ git status
$ git add README.md
$ git commit -m"added README.md"
$ git push origin master
```

Добавить на сервисе **GitHub** в репозитории **lab02** файл **.gitignore**
со следующем содержимом:

```sh
*build*/
*install*/
*.swp
.idea/
```

```sh
$ git pull origin master
$ git log
```

```sh
$ mkdir sources
$ mkdir include
$ mkdir examples
$ cat > sources/print.cpp <<EOF
#include <print.hpp>

void print(const std::string& text, std::ostream& out)
{
  out << text;
}

void print(const std::string& text, std::ofstream& out)
{
  out << text;
}
EOF
```

```sh
$ cat > include/print.hpp <<EOF
#include <fstream>
#include <iostream>
#include <string>

void print(const std::string& text, std::ofstream& out);
void print(const std::string& text, std::ostream& out = std::cout);
EOF
```

```sh
$ cat > examples/example1.cpp <<EOF
#include <print.hpp>

int main(int argc, char** argv)
{
  print("hello");
}
EOF
```

```sh
$ cat > examples/example2.cpp <<EOF
#include <print.hpp>

#include <fstream>

int main(int argc, char** argv)
{
  std::ofstream file("log.txt");
  print(std::string("hello"), file);
}
EOF
```

```sh
$ edit README.md
```

```sh
$ git status
$ git add .
$ git commit -m"added sources"
$ git push origin master
```

## Report

```sh
$ cd ~/workspace/
$ export LAB_NUMBER=02
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER}.git tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gist REPORT.md
```

## Homework

### Part I

1. Создайте пустой репозиторий на сервисе github.com (или gitlab.com, или bitbucket.com).
2. Выполните инструкцию по созданию первого коммита на странице репозитория, созданного на предыдещем шаге.
3. Создайте файл `hello_world.cpp` в локальной копии репозитория (который должен был появиться на шаге 2). Реализуйте программу **Hello world** на языке C++ используя плохой стиль кода. Например, после заголовочных файлов вставьте строку `using namespace std;`.
4. Добавьте этот файл в локальную копию репозитория.
5. Закоммитьте изменения с *осмысленным* сообщением.
6. Изменитьте исходный код так, чтобы программа через стандартный поток ввода запрашивалось имя пользователя. А в стандартный поток вывода печаталось сообщение `Hello world from @name`, где `@name` имя пользователя.
7. Закоммитьте новую версию программы. Почему не надо добавлять файл повторно `git add`?
8. Запуште изменения в удалёный репозиторий.
9. Проверьте, что история коммитов доступна в удалёный репозитории.

### Part II

**Note:** *Работать продолжайте с теми же репоззиториями, что и в первой части задания.*
1. В локальной копии репозитория создайте локальную ветку `patch1`.
2. Внесите изменения в ветке `patch1` по исправлению кода и избавления от `using namespace std;`.
3. **commit**, **push** локальную ветку в удалённый репозиторий.
4. Проверьте, что ветка `patch1` доступна в удалёный репозитории.
5. Создайте pull-request `patch1 -> master`.
6. В локальной копии в ветке `patch1` добавьте в исходный код комментарии.
7. **commit**, **push**.
8. Проверьте, что новые изменения есть в созданном на **шаге 5** pull-request
9. В удалённый репозитории выполните  слияние PR `patch1 -> master` и удалите ветку `patch1` в удаленном репозитории.
10. Локально выполните **pull**.
11. С помощью команды **git log** просмотрите историю в локальной версии ветки `master`.
12. Удалите локальную ветку `patch1`.

### Part III

**Note:** *Работать продолжайте с теми же репоззиториями, что и в первой части задания.*
1. Создайте новую локальную ветку `patch2`.
2. Измените *code style* с помощью утилиты [**clang-format**](http://clang.llvm.org/docs/ClangFormat.html). Например, используя опцию `-style=Mozilla`.
3. **commit**, **push**, создайте pull-request `patch2 -> master`.
4. В ветке **master** в удаленном репозитории измените комментарии, например, расставьте знаки препинания, переведите комментарии на другой язык.
5. Убедитесь, что в pull-request появились *конфликтны*.
6. Для этого локально выполните **pull** + **rebase** (точную последовательность команд, следует узнать самостоятельно). **Исправьте конфликты**.
7. Сделайте *force push* в ветку `patch2`
8. Убедитель, что в pull-request пропали конфликтны. 
9. Вмержите pull-request `patch2 -> master`.

## Links

- [hub](https://hub.github.com/)
- [GitHub](https://github.com)
- [Bitbucket](https://bitbucket.org)
- [Gitlab](https://about.gitlab.com)
- [LearnGitBranching](http://learngitbranching.js.org/)

```
Copyright (c) 2015-2021 The ISC Authors
```
## Homework

## Part 1

1. Создайте пустой репозиторий на сервисе github.com (или gitlab.com, или bitbucket.com).

2. Выполните инструкцию по созданию первого коммита на странице репозитория, созданного на предыдещем шаге.
```sh
$ echo "# lab02" >> README.md
$ git init
$ git add README.md
$ git commit -m "first commit"
$ git remote add origin https://github.com/DimaSokkk/lab02.git
$ git push -u origin master
```
```sh
подсказка: Using 'master' as the name for the initial branch. This default branch name
подсказка: is subject to change. To configure the initial branch name to use in all
подсказка: of your new repositories, which will suppress this warning, call:
подсказка: 
подсказка: 	git config --global init.defaultBranch <name>
подсказка: 
подсказка: Names commonly chosen instead of 'master' are 'main', 'trunk' and
подсказка: 'development'. The just-created branch can be renamed via this command:
подсказка: 
подсказка: 	git branch -m <name>
Инициализирован пустой репозиторий Git в /home/ledibonibell/Dima/Test/.git/
```
3. Создайте файл hello_world.cpp в локальной копии репозитория (который должен был появиться на шаге 2). Реализуйте программу Hello world на языке C++ используя плохой стиль кода. Например, после заголовочных файлов вставьте строку using namespace std;.
```sh
$ nano "Hello world.cpp"
```
```sh
#include <iostream>

using namespace std;

int main(int argc, char** argv) {
  cout << "Hello world" << endl;
}
```
4. Добавьте этот файл в локальную копию репозитория.
```sh
$ git add "Hello world.cpp"
```
5. Закоммитьте изменения с осмысленным сообщением.
```sh
$ git commit -m "This is first file"
```
```sh
[master (корневой коммит) ff9d1c4] first commit
 1 file changed, 1 insertion(+)
 create mode 100644 hello.cpp
```
6. Изменитьте исходный код так, чтобы программа через стандартный поток ввода запрашивалось имя пользователя. А в стандартный поток вывода печаталось сообщение Hello world from @name, где @name имя пользователя.
```sh
#include <iostream>
#include <string>

   string name;
using namespace std;
int main(int argc, char** argv){
   cin >> name;
   cout << "Hello world from " << name << endl;
}
```
7. Закоммитьте новую версию программы. Почему не надо добавлять файл повторно git add?
```sh
$ git commit -a -m "Changed cpp file"
```
```sh
[master ac12f88] changed
 1 file changed, 1 insertion(+)
```
8. Запуште изменения в удалёный репозиторий.
```sh
$ git push -u origin master
```
```sh
Username for 'https://github.com': dimasokkk
Password for 'https://dimasokkk@github.com': 
Перечисление объектов: 6, готово.
Подсчет объектов: 100% (6/6), готово.
Сжатие объектов: 100% (2/2), готово.
Запись объектов: 100% (6/6), 477 байтов | 477.00 КиБ/с, готово.
Всего 6 (изменений 0), повторно использовано 0 (изменений 0), повторно использовано пакетов 0
To https://github.com/DimaSokkk/test.git
 * [new branch]      master -> master
```
9. Проверьте, что история коммитов доступна в удалёный репозитории.

## Part 2

1. В локальной копии репозитория создайте локальную ветку patch1.
```sh
$ git checkout -b patch1
```
```sh
Переключено на новую ветку «patch1»
```
2. Внесите изменения в ветке patch1 по исправлению кода и избавления от using namespace std;.
```sh
#include <iostream>
#include <string>

int main(int argc, char** argv){
  string name;
  std::cin >> name;
  std::cout << "Hello world from " << name << std::endl;
}
```
3. commit, push локальную ветку в удалённый репозиторий.
```sh
$ git commit -a -m "Fixed cpp file"
$ git push -u origin patch1
```
```sh
[patch1 91d13fe] egor
 1 file changed, 2 insertions(+)
 create mode 100644 README.md
```
```sh
Username for 'https://github.com': dimasokkk
Password for 'https://dimasokkk@github.com': 
Перечисление объектов: 4, готово.
Подсчет объектов: 100% (4/4), готово.
Сжатие объектов: 100% (2/2), готово.
Запись объектов: 100% (3/3), 276 байтов | 276.00 КиБ/с, готово.
Всего 3 (изменений 0), повторно использовано 0 (изменений 0), повторно использовано пакетов 0
remote: 
remote: Create a pull request for 'patch1' on GitHub by visiting:
remote:      https://github.com/DimaSokkk/test/pull/new/patch1
remote: 
To https://github.com/DimaSokkk/test.git
 * [new branch]      patch1 -> patch1
```
4. Проверьте, что ветка patch1 доступна в удалёный репозитории.

5. Создайте pull-request patch1 -> master.

6. В локальной копии в ветке patch1 добавьте в исходный код комментарии.
```sh
#include <iostream>
#include <string>

int main(int argc, char** argv){
  string name;                                           // Name of @user
  std::cin >> name;                                      // Input name of @user
  std::cout << "Hello world from " << name << std::endl; // Output name of @user
} 
```
7. commit, push.
```sh
$ git commit -a -m "Code with comments"
$ git push -u origin patch1
```
```sh
[patch1 8df727c] jf
 1 file changed, 3 insertions(+)
```
```sh
Username for 'https://github.com': dimasokkk
Password for 'https://dimasokkk@github.com': 
Перечисление объектов: 5, готово.
Подсчет объектов: 100% (5/5), готово.
Сжатие объектов: 100% (3/3), готово.
Запись объектов: 100% (3/3), 325 байтов | 325.00 КиБ/с, готово.
Всего 3 (изменений 0), повторно использовано 0 (изменений 0), повторно использовано пакетов 0
To https://github.com/DimaSokkk/test.git
   91d13fe..8df727c  patch1 -> patch1
```
8. Проверьте, что новые изменения есть в созданном на шаге 5 pull-request

9. В удалённый репозитории выполните слияние PR patch1 -> master и удалите ветку patch1 в удаленном репозитории.

10. Локально выполните pull.
```sh
$ git pull origin
```
```sh
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0
Распаковка объектов: 100% (1/1), 619 байтов | 206.00 КиБ/с, готово.
Из https://github.com/DimaSokkk/test
   ac12f88..2763978  master     -> origin/master
Вы попросили получить изменения со внешнего репозитория «origin», но не указали ветку. Так как это не репозиторий по умолчанию для  вашей текущей ветки, вы должны указать ветку в командной строке.
```
11. С помощью команды git log просмотрите историю в локальной версии ветки master.
```sh
$ git log master
```
```sh
commit ac12f88222eecfb903813aaf2d25cffcad7aa583 (master)
Author: DimaSokkk <dimabomonka@mail.ru>
Date:   Wed Jun 7 10:43:09 2023 +0300

    Changed cpp file

commit ff9d1c421d52a5b61c412db16339c3d535605d8d
Author: DimaSokkk <dimabomonka@mail.ru>
Date:   Wed Jun 7 10:41:32 2023 +0300

    first commit
```
12. Удалите локальную ветку patch1.
```sh
$ git checkout -b origin
$ git branch -d patch1
```
```sh
Переключено на новую ветку «origin»
Ветка patch1 удалена (была 8df727c).
```

## Part 3

1. Создайте новую локальную ветку patch2.
```sh
$ git checkout -b patch2
$ git branch -d origin
```
```sh
Переключено на новую ветку «patch2»
Ветка origin удалена (была 8df727c).
```
2. Измените code style с помощью утилиты clang-format. Например, используя опцию -style=Mozilla.
```sh
$ sudo apt install clang-format
$ clang-format -i -style=Mozilla "hello world.cpp"
```
3. commit, push, создайте pull-request patch2 -> master.
```sh
$ git commit -a -m "Changed code style in cpp file"
$ git push -u origin patch2
```
```sh
[patch2 98701c1] Changed code style in cpp file
 1 file changed, 1 insertion(+)
```
```sh
Username for 'https://github.com': dimasokkk
Password for 'https://dimasokkk@github.com': 
Перечисление объектов: 5, готово.
Подсчет объектов: 100% (5/5), готово.
Сжатие объектов: 100% (3/3), готово.
Запись объектов: 100% (3/3), 357 байтов | 357.00 КиБ/с, готово.
Всего 3 (изменений 0), повторно использовано 0 (изменений 0), повторно использовано пакетов 0
remote: 
remote: Create a pull request for 'patch2' on GitHub by visiting:
remote:      https://github.com/DimaSokkk/test/pull/new/patch2
remote: 
To https://github.com/DimaSokkk/test.git
 * [new branch]      patch2 -> patch2
Ветка «patch2» отслеживает внешнюю ветку «patch2» из «origin».
```
4. В ветке master в удаленном репозитории измените комментарии, например, расставьте знаки препинания, переведите комментарии на другой язык.
```sh
$ git checkout master
```
```sh
#include <iostream>
#include <string>

int main(int argc, char** argv){
  string name;                                           // Имя пользователя
  std::cin >> name;                                      // Ввод имени пользователя
  std::cout << "Hello world from " << name << std::endl; // Вывод данных
} 
```
```sh
$ git commit -a -m "Fixed cpp file"
$ git push -u origin patch1
```
5. Убедитесь, что в pull-request появились конфликтны.

6. Для этого локально выполните pull + rebase (точную последовательность команд, следует узнать самостоятельно). Исправьте конфликты.
```sh
$ git pull origin master
$ git rebase master
$ edit "Hello world.cpp"
```
```sh
$ git commit -a -m "Update code"
```
```sh
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
Распаковка объектов: 100% (3/3), 677 байтов | 677.00 КиБ/с, готово.
Из https://github.com/DimaSokkk/test
 * branch            master     -> FETCH_HEAD
   2763978..5cff030  master     -> origin/master
подсказка: You have divergent branches and need to specify how to reconcile them.
подсказка: You can do so by running one of the following commands sometime before
подсказка: your next pull:
подсказка: 
подсказка:   git config pull.rebase false  # merge (the default strategy)
подсказка:   git config pull.rebase true   # rebase
подсказка:   git config pull.ff only       # fast-forward only
подсказка: 
подсказка: You can replace "git config" with "git config --global" to set a default
подсказка: preference for all repositories. You can also pass --rebase, --no-rebase,
подсказка: or --ff-only on the command line to override the configured default per
подсказка: invocation.
fatal: Need to specify how to reconcile divergent branches.
```
```sh
Current branch patch2 is up to date.
```
7. Сделайте force push в ветку patch2
```sh
$ git checkout patch2
$ git merge master
```
```sh
$ git add "Hello world.cpp"
$ git commit -a -m "Last fix"
$ git push -f origin patch2
```
```sh
Username for 'https://github.com': dimasokkk
Password for 'https://dimasokkk@github.com': 
Перечисление объектов: 5, готово.
Подсчет объектов: 100% (5/5), готово.
Сжатие объектов: 100% (3/3), готово.
Запись объектов: 100% (3/3), 309 байтов | 309.00 КиБ/с, готово.
Всего 3 (изменений 1), повторно использовано 0 (изменений 0), повторно использовано пакетов 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/DimaSokkk/test.git
   689ea29..f0c6c4a  patch2 -> patch2
```
8. Убедитель, что в pull-request пропали конфликтны.

9. Вмержите pull-request patch2 -> master.

