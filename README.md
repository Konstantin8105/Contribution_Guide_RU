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

## Configure Git to use Gerrit

You'll need a web browser and a command line terminal.
You should already have Git installed.



Gerrit uses Google Accounts for authentication.
If you don't have a Google Account, you can create an account which <a href="https://www.google.com/accounts/NewAccount">includes a new Gmail email account</a> or create an account associated <a href="https://accounts.google.com/SignUpWithoutGmail">with your existing email address</a>.


### Step 1: Sign in to googlesource and generate a password


Visit <a href="https://go.googlesource.com">go.googlesource.com</a> and click on "Generate Password" in the page's top right menu bar.
You will be redirected to accounts.google.com to sign in.

### Step 2: Run the provided script

After signing in, you are taken to a page on go.googlesource.com with the title "Configure Git".
This page contains a personalized script which when run locally will configure git to have your unique authentication key.
This key is paired with one generated server side similar to how ssh keys work.



Copy and run this script locally in your command line terminal.
(On a Windows computer using cmd you should instead follow the instructions in the yellow box to run the command. If you are using git-bash use the same script as `*nix`.)



Your secret authentication token is now in a `.gitcookie` file and Git is configured to use this file.


### Step 3: Register with Gerrit


Now that you have your authentication token, you need to register your account with Gerrit.
To do this, visit <a href="https://go-review.googlesource.com/login/"> go-review.googlesource.com/login/</a>.
Sign in using the same Google Account you used above.


## Contributor License Agreement

### Which CLA

Before sending your first change to the Go project you must have completed one of the following two CLAs.
Which CLA you should sign depends on who owns the copyright to your work.


<ul>
<li>
If you are the copyright holder, you will need to agree to the <a href="https://developers.google.com/open-source/cla/individual">individual contributor license agreement</a>, which can be completed online.
</li>
<li>
If your organization is the copyright holder, the organization will need to agree to the <a href="https://developers.google.com/open-source/cla/corporate">corporate contributor license agreement</a>.<br>
</li>
</ul>


<i>If the copyright holder for your contribution has already completed the agreement in connection with another Google open source project, it does not need to be completed again.</i>

### Completing the CLA


You can see your currently signed agreements and sign new ones through the Gerrit interface.
To do this, <a href="https://go-review.googlesource.com/login/">Log into Gerrit</a>, click your name in the upper-right, choose "Settings", then select "Agreements" from the topics on the left.
If you do not have a signed agreement listed here, you can create one by clicking "New Contributor Agreement" and following the steps.



If the copyright holder for the code you are submitting changes &mdash; for example, if you start contributing code on behalf of a new company &mdash; please send email to golang-dev and let us know, so that we can make sure an appropriate agreement is completed and update the `AUTHORS` file.

# Preparing a Development Environment for Contributing

## Setting up Git for submission to Gerrit

Changes to Go must be reviewed before they are accepted, no matter who makes the change.
A custom git command called `git-codereview`, discussed below, helps manage the code review process through a Google-hosted <a href="https://go-review.googlesource.com/">instance</a> Gerrit.

### Install the git-codereview command

Install the `git-codereview` command by running,


```
$ go get -u golang.org/x/review/git-codereview
```


Make sure `git-codereview` is installed in your shell path, so that the `git` command can find it. Check that


```
$ git codereview help
```


prints help text, not an error.



On Windows, when using git-bash you must make sure that `git-codereview.exe` is in your git exec-path. Run `git --exec-path` to discover the right location then create a symbolic link or simply copy the executible from $GOPATH/bin to this directory.



**Note to Git aficionados:**
The `git-codereview` command is not required to upload and manage Gerrit code reviews.
For those who prefer plain Git, the text below gives the Git equivalent of each git-codereview command.



If you do use plain Git, note that you still need the commit hooks that the git-codereview command configures; those hooks add a Gerrit `Change-Id` line to the commit message and check that all Go source files have been formatted with gofmt.
Even if you intend to use plain Git for daily work, install the hooks in a new Git checkout by running `git-codereview` `hooks`.



The workflow described below assumes a single change per branch.
It is also possible to prepare a sequence of (usually related) changes in a single branch.
See the <a href="https://golang.org/x/review/git-codereview">git-codereview documentation</a> for details.


### Set up git aliases


The `git-codereview` command can be run directly from the shell by typing, for instance,


```
$ git codereview sync
```


but it is more convenient to set up aliases for `git-codereview`'s own subcommands, so that the above becomes,


```
$ git sync
```


The `git-codereview` subcommands have been chosen to be distinct from Git's own, so it's safe to do so.



The aliases are optional, but in the rest of this document we will assume they are installed.
To install them, copy this text into your Git configuration file (usually `.gitconfig` in your home directory):


```
[alias]
	change = codereview change
	gofmt = codereview gofmt
	mail = codereview mail
	pending = codereview pending
	submit = codereview submit
	sync = codereview sync
```

### Understanding the git-codereview command

After installing the `git-codereview` command, you can run

```
$ git codereview help
```


to learn more about its commands.
You can also read the <a href="https://godoc.org/golang.org/x/review/git-codereview">command documentation</a>.



# Making a Contribution

## Discuss your design


The project welcomes submissions but please let everyone know what you're working on if you want to change or add to the Go repositories.



Before undertaking to write something new for the Go project, please <a href="https://golang.org/issue/new">file an issue</a> (or claim an <a href="https://golang.org/issues">existing issue</a>).
Significant changes must go through the <a href="https://golang.org/s/proposal-process">change proposal process</a> before they can be accepted.



This process gives everyone a chance to validate the design, helps prevent duplication of effort, and ensures that the idea fits inside the goals for the language and tools.
It also checks that the design is sound before code is written; the code review tool is not the place for high-level discussions.



When planning work, please note that the Go project follows a <a href="https://golang.org/wiki/Go-Release-Cycle">six-month development cycle</a>.
The latter half of each cycle is a three-month feature freeze during which only bug fixes and doc updates are accepted. New contributions can be sent during a feature freeze but will not be accepted until the freeze thaws.


## Making a change

### Getting Go Source

First you need to have a local copy of the source checked out from the correct
repository.
As Go builds Go you will also likely need to have a working version of Go installed (some documentation changes may not need this).
This should be a recent version of Go and can be obtained via any package or binary distribution or you can build it from source.



You should checkout the Go source repo anywhere you want as long as it's outside of your $GOPATH.
Go to a directory where you want the source to appear and run the following command in a terminal.


````
$ git clone https://go.googlesource.com/go
$ cd go
````

### Contributing to the main Go tree


Most Go installations use a release branch, but new changes should only be made based on the master branch. <br>
(They may be applied later to a release branch as part of the release process, but most contributors won't do this themselves.)
Before making a change, make sure you start on the master branch:


```
$ git checkout master
$ git sync
```


(In Git terms, `git` `sync` runs
`git` `pull` `-r`.)


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
Instead, your name will appear in the <a href="https://golang.org/change">change log</a> and in the <a href="/CONTRIBUTORS">`CONTRIBUTORS`</a> file and perhaps the <a href="/AUTHORS">`AUTHORS`</a> file.
These files are automatically generated from the commit logs perodically.
The <a href="/AUTHORS">`AUTHORS`</a> file defines who &ldquo;The Go Authors&rdquo;&mdash;the copyright holders&mdash;are.


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
$ git change <i>&lt;branch&gt;</i>
```


The name <i>&lt;branch&gt;</i> is an arbitrary one you choose to identify the local branch containing your changes and will not be used elsewhere.
This is an offline operation and nothing will be sent to the server yet.



(In Git terms, `git` `change` `&lt;branch&gt;` runs `git` `checkout` `-b` `branch`, then `git` `branch` `--set-upstream-to` `origin/master`, then `git` `commit`.)



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
<a href="https://golang.org/issue/159">Go issue tracker</a>.
When this change is eventually applied, the issue tracker will automatically mark the issue as fixed.
(There are several such conventions, described in detail in the <a href="https://help.github.com/articles/closing-issues-via-commit-messages/">GitHub Issue Tracker documentation</a>.)



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


You've <a href="code.html">written and tested your code</a>, but before sending code out for review, run all the tests for the whole tree to make sure the changes don't break other packages or programs:


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
<br>
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
$ git commit --amend --author="Author Name &lt;email@address.com&gt;"
```


Finally try to resend with:


```
$ git mail
```

### Specifying a reviewer / CCing others


Unless explicitly told otherwise, such as in the discussion leading up to sending in the change list, it's better not to specify a reviewer.
All changes are automatically CC'ed to the <a href="https://groups.google.com/group/golang-codereviews">golang-codereviews@googlegroups.com</a> mailing list. If this is your first ever change, there may be a moderation delay before it appears on the mailing list, to prevent spam.



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



You can see a list of your pending changes by running `git` `pending`, and switch between change branches with `git` `change` `<i>&lt;branch&gt;</i>`.


### Synchronize your client


While you were working, others might have submitted changes to the repository.
To update your local branch, run


```
$ git sync
```


(In git terms, `git` `sync` runs
`git` `pull` `-r`.)


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
  (use "git reset HEAD &lt;file&gt;..." to unstage)
  (use "git add &lt;file&gt;..." to mark resolution)

	<i>both modified: sin.go</i>
```


The only important part in that transcript is the italicized "both modified" line: Git failed to merge your changes with the conflicting change.
When this happens, Git leaves both sets of edits in the file,
with conflicts marked by `&lt;&lt;&lt;&lt;&lt;&lt;&lt;` and `&gt;&gt;&gt;&gt;&gt;&gt;&gt;`.
It is now your job to edit the file to combine them.
Continuing the example, searching for those strings in `sin.go` might turn up:


```
	arg = scale(arg)
&lt;&lt;&lt;&lt;&lt;&lt;&lt; HEAD
	if arg &lt; 1e9 {
=======
	if arg &lt; 1e10 {
&gt;&gt;&gt;&gt;&gt;&gt;&gt; mcgillicutty
		largeReduce(arg)
```


Git doesn't show it, but suppose the original text that both edits started with was 1e8; you changed it to 1e10 and the other change to 1e9, so the correct answer might now be 1e10.
First, edit the section to remove the markers and leave the correct code:


```
	arg = scale(arg)
	if arg &lt; 1e10 {
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


In addition to the information here, the Go community maintains a <a href="https://golang.org/wiki/CodeReview">CodeReview</a> wiki page. Feel free to contribute to this page as you learn the review process.
