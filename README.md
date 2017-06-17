# Перевод "Руководство сотрудничества"

------

Оригинал смотри: https://golang.org/doc/contribute.html


`go version go1.8.3 linux/amd64`

# Оглавление
------


------

Проект Go приветствует всех помощников. Процесс внесения своего вклада в проект Go может отличаться от того к чему Вы привыкли. Данный документ предназначен для того чтобы помочь Вам разобраться в  том - как внести свой вклад. Данное руковоство предполагает, что у Вас уже есть общее знания работы в Git и Go.

(Примечание, Руководство сотрудничества для `gccgo` расположено [здесь](https://golang.org/doc/gccgo_contribute.html) )

(По воспросам связанным с безопасностью, обращайтесь по адресу security@golang.org).


# Как стать помощником(соучастником)

Прежде чем стать помощником для проекта Go, Вам необходимо выполнить несколько условий.
Проект Go использует [Gerrit](https://www.gerritcodereview.com/), как онлайн-инструмент с открытым исходным кодом для всестороннего рассмотрения кода.
Gerrit использует Ваш адрес электронной почты как уникальный идентификатор.
В проекте Go пока только настроен только на Google Accounts.
В связи с этим, Вам необходимо *предварительно* иметь хотя бы один Google Account.

## Настройка Git для использования Gerrit

Для этого Вам необходим веб-браузер и консоль командной строки.
Вам необходимо иметь установленный Git.

Gerrit использует Google Accounts для аутентификации.
Если у Вас нет Google Account, то Вы можете создать его [включая новый аккаунт почты Gmail](https://www.google.com/accounts/NewAccount) или создать ассоциированный аккаунт [с имеющийся у Вас адресом электронной почты](https://accounts.google.com/SignUpWithoutGmail).


### Шаг 1: Регистрация в googlesource для генерации пароля

Visit [go.googlesource.com](https://go.googlesource.com) и кликните на  "Generate Password" в верхней правой части меню страницы.
Вас потом перенаправит на accounts.google.com для авторизации.

### Шаг 2: Запуск скрипта

После регистрации, Вы попадеье на страницу [go.googlesource.com](https://go.googlesource.com) озаглавленный "Configure Git"(Настройка Git).
Эта страница включает область с Вашим персональным скриптом, который при локальном запуске (на Вашем компьютере) настроет git для Вас уникальный аутентификационный ключ.
Это ключ является одним из пары, причем один генерируется на стороне сервера, похоже на работу с ключами для ssh.

Скопируйте и запустите этот скрипт в Вашем терминале командной строки.
(На компьютере с Windows используйте программу `cmd` и используйте инструкцию в желтой области веб-страницы для запуска команд. Если же Вы используете git-bash то используйте тот же скрипт как для `*nix`)

Ваш секретный ключ после запуска скрипта расположен в файле `.gitcookie` и теперь Git настроен для использования этого файла.

### Шаг 3: Регистрация в Gerrit

Сейчас для регистрации своего ключа, Вам необходимо зарегистрировать свой аккаунт в Gerrit.
Для этого посетите страницу [ go-review.googlesource.com/login/](https://go-review.googlesource.com/login/).
Зарегистрируйтесь используя тот же Google Account, который использовали ранее.

## Лицензионное соглашение сотрудничества(Contributor License Agreement, CLA)

### Какое CLA

Перед отправкой Ваших первых изменений в проект Go Вы должны подтвердить один из двух CLA.
Какой именно CLA Вы подпишите зависит от того кто владеет авторскими правами на Вашу работу.

* Если Вы являетесь владельцем авторских прав, то Вам необходимо согласиться с  [individual contributor license agreement](https://developers.google.com/open-source/cla/individual), который допускают подтверждение в онлайн.
* Если владельцем Ваших авторских прав является организация, то организации необходимо согласиться с [corporate contributor license agreement](https://developers.google.com/open-source/cla/corporate).

*Если владелей авторских прав уже подтвердил соглашение в другом открытом проекте Google, то дополнительно подтверждать нет необходимости.*

### Завершение CLA

Вы можете увидеть подписанное Вами соглашение и подписать ещё одно через интерфейс Gerrit.
Для этого, [войдите в свой аккаунт Gerrit](https://go-review.googlesource.com/login/), кликните на Вашем имени в вверхнем правом углу и выбирете "Settings"("Настройки"), потом выбирете "Agreements"(Соглашения) слево вверху. Но если у Вас нет подписанных соглашений описанных тут, то Вы можете создать новый кликнув на "New Contributor Agreement" и действовать по следующим шагам.

Если Вы владелец авторских прав на код, который Вы будуте отправлять изменения - к примеру, если Вы начинаете вводить код от имени новой компании - то пожайлуста отправьте электронное письмо golang-dev и дайте нам знать, чтобы мы могли убедиться, что соответствующее соглашение будет завершено и обновим файл `AUTHORS`(авторы).

# Подготовка среды разработки

## Настройка Git для отправки в Gerrit

Изменения в Go должны пройти рассмотрение(review) для их принятия, в не зависимости от того кто создал данное изменение.
Команда git называется `git-codereview`, обсуждаемая ниже, поможет управлять рассмотрением кода, размещенный на Google-хост [экземляре](https://go-review.googlesource.com/) Gerrit.

### Установка команды git-codereview

Установка команды `git-codereview` происходит запуском,

```
$ go get -u golang.org/x/review/git-codereview
```

Убедитесь что `git-codereview` установился в Вашу папку, как же команда `git` может найти его. Проверим это:

```
$ git codereview help
```

напечатает текст помощи, не ошибку.

Если Вы используете git-bash в Windows убедитесь что программа `git-codereview.exe` расположена вашей папке git. Запустите `git --exec-path` для того чтобы открыть нужное место, затем создайте символическую ссылку или просто скопируйте исполняемый файл из $GOPATH/bin в этот каталог.


**Примечание для поклонников Git:**
Команда `git-codereview` не требуется для загрузки и управления кода Gerrit рассмотрение.
Для тех, кто предпочитает простой Git, то текст ниже предоставит эквивалентную Git команды для каждой git-codereview рассмотрения кода.

Если Вы используете простой Git, обратите внимание, что вам все равно потребуется настройка команд `git-codereview`. Эта настройка добавляет `Change-Id` для Gerrit в сообщение фиксирования(commit) и проверки всех исходных файлов Go, который отформатирован `gofmt`.
Даже если Вы собираетесь использовать простого Git для ежедневной работы, то настройте Git checkout запустив `git-codereview` `hooks`.

Показанный ниже рабочий процесс предполагает одно изменение на ветку(branch).
Это также предполагает подготовку последовательности (обычно связанных) изменений в одну ветку.
Смотрите детальнее [git-codereview documentation](https://golang.org/x/review/git-codereview).

### Настройка git-псевдонимов(git aliases)

Команда `git-codereview` может быть запущено прямо из командной строки, например:

```
$ git codereview sync
```

Но будет удобнее если настроить псевдонимы для команд `git-codereview`, так что выше превращается в:

```
$ git sync
```


Подкоманда `git-codereview` были выбраны таким образом чтобы отличились от собственных команд Git, так что использование безопастно.

Псевдонимы не обязательны, но в дальнейшем в этом документе, мы будем предполагать что они установлены.
Для установки их, необходимо скопировать следующий текст в Ваш конфигурационный файл Git(обычно это файл в домашней папке `.gitconfig`):

```
[alias]
	change = codereview change
	gofmt = codereview gofmt
	mail = codereview mail
	pending = codereview pending
	submit = codereview submit
	sync = codereview sync
```

### Понимание команд git-codereview

После установки комманд `git-codereview`, вы можете запустить

```
$ git codereview help
```

для того чтобы изучить эти комманды.
Вы также можете почитать [command documentation](https://godoc.org/golang.org/x/review/git-codereview).

# Внесение вклада (Making a Contribution)

## Обсудите Ваш дизайн(вопрос)

The project welcomes submissions but please let everyone know what you're working on if you want to change or add to the Go repositories.



Before undertaking to write something new for the Go project, please [file an issue](https://golang.org/issue/new) (or claim an [existing issue](https://golang.org/issues)).
Significant changes must go through the [change proposal process](https://golang.org/s/proposal-process) before they can be accepted.



This process gives everyone a chance to validate the design, helps prevent duplication of effort, and ensures that the idea fits inside the goals for the language and tools.
It also checks that the design is sound before code is written; the code review tool is not the place for high-level discussions.



When planning work, please note that the Go project follows a [six-month development cycle](https://golang.org/wiki/Go-Release-Cycle).
The latter half of each cycle is a three-month feature freeze during which only bug fixes and doc updates are accepted. New contributions can be sent during a feature freeze but will not be accepted until the freeze thaws.


## Making a change

### Getting Go Source

First you need to have a local copy of the source checked out from the correct repository.
As Go builds Go you will also likely need to have a working version of Go installed (some documentation changes may not need this).
This should be a recent version of Go and can be obtained via any package or binary distribution or you can build it from source.



You should checkout the Go source repo anywhere you want as long as it's outside of your $GOPATH.
Go to a directory where you want the source to appear and run the following command in a terminal.


````
$ git clone https://go.googlesource.com/go
$ cd go
````

### Contributing to the main Go tree


Most Go installations use a release branch, but new changes should only be made based on the master branch.
(They may be applied later to a release branch as part of the release process, but most contributors won't do this themselves.)
Before making a change, make sure you start on the master branch:


```
$ git checkout master
$ git sync
```


(In Git terms, `git` `sync` runs `git` `pull` `-r`.)


### Contributing to subrepositories (golang.org/x/...)


If you are contributing a change to a subrepository, obtain the Go package using `go get`. For example, to contribute to `golang.org/x/oauth2`, check out the code by running:


```
$ go get -d golang.org/x/oauth2/...
```


Then, change your directory to the package's source directory (`$GOPATH/src/golang.org/x/oauth2`).


### Make your changes


The entire checked-out tree is editable.
Make your changes as you see fit ensuring that you create appropriate tests along with your changes. Test your changes as you go.


### Copyright


Files in the Go repository don't list author names, both to avoid clutter and to avoid having to keep the lists up to date.
Instead, your name will appear in the [change log](https://golang.org/change) and in the <a href="/CONTRIBUTORS">`CONTRIBUTORS`</a> file and perhaps the <a href="/AUTHORS">`AUTHORS`</a> file.
These files are automatically generated from the commit logs perodically.
The <a href="/AUTHORS">`AUTHORS`</a> file defines who "The Go Authors"-the copyright holders-are.


New files that you contribute should use the standard copyright header:

```
// Copyright 2017 The Go Authors. All rights reserved.
// Use of this source code is governed by a BSD-style
// license that can be found in the LICENSE file.
```


Files in the repository are copyright the year they are added.
Do not update the copyright year on files that you change.


### Commit your changes


Once you have edited files, you must tell Git that they have been modified.
You must also tell Git about any files that are added, removed, or renamed files.
These operations are done with the usual Git commands, `git` `add`, `git` `rm`, and `git` `mv`.



Once you have the changes queued up, you will want to commit them.
In the Go contribution workflow this is done with a `git change` command, which creates a local branch and commits the changes directly to that local branch.


```
$ git change *<branch>*
```


The name *<branch>* is an arbitrary one you choose to identify the local branch containing your changes and will not be used elsewhere.
This is an offline operation and nothing will be sent to the server yet.



(In Git terms, `git` `change` `<branch>` runs `git` `checkout` `-b` `branch`, then `git` `branch` `--set-upstream-to` `origin/master`, then `git` `commit`.)



As the `git commit` is the final step, Git will open an editor to ask for a commit message.
(It uses the editor named by the `$EDITOR` environment variable, `vi` by default.)

The file will look like:


```

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch foo
# Changes not staged for commit:
#	modified:   editedfile.go
#
```


At the beginning of this file is a blank line; replace it with a thorough description of your change.
The first line of the change description is conventionally a one-line summary of the change, prefixed by the primary affected package, and is used as the subject for code review email.
It should complete the sentence "This change modifies Go to _____."
The rest of the description elaborates and should provide context for the change and explain what it does.
Write in complete sentences with correct punctuation, just like for your comments in Go.
If there is a helpful reference, mention it here.
If you've fixed an issue, reference it by number with a # before it.



After editing, the template might now read:


```
math: improve Sin, Cos and Tan precision for very large arguments

The existing implementation has poor numerical properties for large arguments, so use the McGillicutty algorithm to improve accuracy above 1e10.

The algorithm is described at http://wikipedia.org/wiki/McGillicutty_Algorithm

Fixes #159

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch foo
# Changes not staged for commit:
#	modified:   editedfile.go
#
```


The commented section of the file lists all the modified files in your client.
It is best to keep unrelated changes in different change lists, so if you see a file listed that should not be included, abort the command and move that file to a different branch.



The special notation "Fixes #159" associates the change with issue 159 in the
[Go issue tracker](https://golang.org/issue/159).
When this change is eventually applied, the issue tracker will automatically mark the issue as fixed.
(There are several such conventions, described in detail in the [GitHub Issue Tracker documentation](https://help.github.com/articles/closing-issues-via-commit-messages/).)



Once you have finished writing the commit message, save the file and exit the editor.



You must have the $EDITOR environment variable set properly and working properly (exiting cleanly) for this operation to succeed.
If you run into any issues at this step, it's likely your editor isn't exiting cleanly.
Try setting a different editor in your $EDITOR environment variable.



If you wish to do more editing, re-stage your changes using `git` `add`, and then run


```
$ git change
```


to update the change description and incorporate the staged changes.
The change description contains a `Change-Id` line near the bottom, added by a Git commit hook during the initial `git` `change`.
That line is used by Gerrit to match successive uploads of the same change.
Do not edit or delete it.



(In Git terms, `git` `change` with no branch name runs `git` `commit` `--amend`.)


### Testing


You've [written and tested your code](code.html), but before sending code out for review, run all the tests for the whole tree to make sure the changes don't break other packages or programs:


```
$ cd go/src
$ ./all.bash
```


(To build under Windows use `all.bat`.)



After running for a while, the command should print


```
"ALL TESTS PASSED".
```

### Send the change for review


Once the change is ready, send it for review.
This is similar to a `git push` in a GitHub style workflow.
This is done via the mail alias setup earlier which despite its name, doesn't directly mail anything, it simply sends the change to Gerrit via git push.


```
$ git mail
```


(In Git terms, `git` `mail` pushes the local committed changes to Gerrit using `git` `push` `origin` `HEAD:refs/for/master`.)



If your change relates to an open issue, please add a comment to the issue announcing your proposed fix, including a link to your CL.



The code review server assigns your change an issue number and URL, which `git` `mail` will print, something like:


```
remote: New Changes:
remote:   https://go-review.googlesource.com/99999 math: improved Sin, Cos and Tan precision for very large arguments
```

### Troubleshooting


The most common way that the `git mail` command fails is because the email address used has not gone through the setup above.

If you see something like...


```
remote: Processing changes: refs: 1, done
remote:
remote: ERROR:  In commit ab13517fa29487dcf8b0d48916c51639426c5ee9
remote: ERROR:  author email address XXXXXXXXXXXXXXXXXXX
remote: ERROR:  does not match your user account.
```


You need to either add the email address listed to the CLA or set this repo to use another email address already approved.



First let's change the email address for this repo so this doesn't happen again. You can change your email address for this repo with the following command:


```
$ git config user.email email@address.com
```


Then change the previous commit to use this alternative email address.
You can do that with:


```
$ git commit --amend --author="Author Name <email@address.com>"
```


Finally try to resend with:


```
$ git mail
```

### Specifying a reviewer / CCing others


Unless explicitly told otherwise, such as in the discussion leading up to sending in the change list, it's better not to specify a reviewer.
All changes are automatically CC'ed to the [golang-codereviews@googlegroups.com](https://groups.google.com/group/golang-codereviews) mailing list. If this is your first ever change, there may be a moderation delay before it appears on the mailing list, to prevent spam.



You can specify a reviewer or CC interested parties using the `-r` or `-cc` options.
Both accept a comma-separated list of email addresses:


```
$ git mail -r joe@golang.org -cc mabel@example.com,math-nuts@swtch.com
```

## Going through the review process


Running `git` `mail` will send an email to you and the reviewers asking them to visit the issue's URL and make comments on the change.
When done, the reviewer adds comments through the Gerrit user interface and clicks "Reply" to send comments back.
You will receive a mail notification when this happens.
You must reply through the web interface.
(Unlike with the old Rietveld review system, replying by mail has no effect.)


### Revise and resend


The Go contribution workflow is optimized for iterative revisions based on
feedback.
It is rare that an initial contribution will be ready to be applied as is.
As you revise your contribution and resend Gerrit will retain a history of all the changes and comments made in the single URL.



You must respond to review comments through the web interface.
(Unlike with the old Rietveld review system, responding by mail has no effect.)



When you have revised the code and are ready for another round of review, stage those changes and use `git` `change` to update the
commit.
To send the update change list for another round of review, run `git` `mail` again.



The reviewer can comment on the new copy, and the process repeats.
The reviewer approves the change by giving it a positive score (+1 or +2) and replying `LGTM`: looks good to me.



You can see a list of your pending changes by running `git` `pending`, and switch between change branches with `git` `change` `*<branch>*`.


### Synchronize your client


While you were working, others might have submitted changes to the repository.
To update your local branch, run


```
$ git sync
```


(In git terms, `git` `sync` runs `git` `pull` `-r`.)


### Resolving Conflicts


If files you were editing have changed, Git does its best to merge the remote changes into your local changes.
It may leave some files to merge by hand.



For example, suppose you have edited `sin.go` but someone else has committed an independent change.
When you run `git` `sync`, you will get the (scary-looking) output:

```
$ git sync
Failed to merge in the changes.
Patch failed at 0023 math: improved Sin, Cos and Tan precision for very large arguments The copy of the patch that failed is found in:
   /home/you/repo/.git/rebase-apply/patch

When you have resolved this problem, run "git rebase --continue".
If you prefer to skip this patch, run "git rebase --skip" instead.
To check out the original branch and stop rebasing, run "git rebase --abort".
```


If this happens, run


```
$ git status
```


to see which files failed to merge.
The output will look something like this:


```
rebase in progress; onto a24c3eb
You are currently rebasing branch 'mcgillicutty' on 'a24c3eb'.
  (fix conflicts and then run "git rebase --continue")
  (use "git rebase --skip" to skip this patch)
  (use "git rebase --abort" to check out the original branch)

Unmerged paths:
  (use "git reset HEAD <file>..." to unstage)
  (use "git add <file>..." to mark resolution)

	*both modified: sin.go*
```


The only important part in that transcript is the italicized "both modified" line: Git failed to merge your changes with the conflicting change.
When this happens, Git leaves both sets of edits in the file,
with conflicts marked by `<<<<<<<` and `>>>>>>>`.
It is now your job to edit the file to combine them.
Continuing the example, searching for those strings in `sin.go` might turn up:


```
	arg = scale(arg)
<<<<<<< HEAD
	if arg < 1e9 {
=======
	if arg < 1e10 {
>>>>>>> mcgillicutty
		largeReduce(arg)
```


Git doesn't show it, but suppose the original text that both edits started with was 1e8; you changed it to 1e10 and the other change to 1e9, so the correct answer might now be 1e10.
First, edit the section to remove the markers and leave the correct code:


```
	arg = scale(arg)
	if arg < 1e10 {
		largeReduce(arg)
```


Then tell Git that the conflict is resolved by running


```
$ git add sin.go
```


If you had been editing the file, say for debugging, but do not care to preserve your changes, you can run `git` `reset` `HEAD` `sin.go` to abandon your changes.
Then run `git` `rebase` `--continue` to restore the change commit.


### Reviewing code by others


As part of the review process reviewers can propose changes directly (in the GitHub workflow this would be someone else attaching commits to a pull request).

You can import these changes proposed by someone else into your local Git repository.
On the Gerrit review page, click the "Download ▼" link in the upper right corner, copy the "Checkout" command and run it from your local Git repo. It should look something like this:


```
$ git fetch https://go.googlesource.com/review refs/changes/21/1221/1 &amp;&amp; git checkout FETCH_HEAD
```


To revert, change back to the branch you were working in.


## Apply the change to the master branch


After the code has been `LGTM`'ed, an approver may apply it to the master branch using the Gerrit UI.
There is a "Submit" button on the web page for the change that appears once the change is approved (marked +2).



This checks the change into the repository.
The change description will include a link to the code review, and the code review will be updated with a link to the change in the repository.
Since the method used to integrate the changes is "Cherry Pick", the commit hashes in the repository will be changed by the "Submit" operation.


## More information


In addition to the information here, the Go community maintains a [CodeReview](https://golang.org/wiki/CodeReview) wiki page. Feel free to contribute to this page as you learn the review process.
