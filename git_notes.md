### About git book 
...
[Link to think like a git](http://think-like-a-git.net)

[Link to a visual git reference](http://marklodato.github.io/visual-git-guide/index-en.html)


#### There are three stages that the files can reside in:
1. Commited: the data is safely store in your local database
2. Modified: it means that you have chenged the file but have not commited it to the databasse yet
3. Staged: It means that you have marked  a modified file in its current version to go into your next commit snapshot

#### Three main sections of a Git project:
1. The git directory: is where Git stores the metadata and object database for your project.
2. The working directory: is a checkout of one version of your project, the files are pulled out of the compressed database in the Gt directory and placed on disk for you to use or modifiy
3. The staging area: it is a file, generally contained in your Git directory, that
stores information about what will go into your next commit. It’s sometimes re-
ferred to as the “index”, but it’s also common to refer to it as the staging area.

#### The basic Git workflow goes something like this:
1. You modify files in your working directory.
2. You stage the files, adding snapshots of them to your staging area.
3. You do a commit, which takes the files as they are in the staging area and
stores that snapshot permanently to your Git directory.

If a particular version of a file is in the Git directory, it’s considered commit-
ted. If it has been modified and was added to the staging area, it is staged. And
if it was changed since it was checked out but has not been staged, it is modi-
fied.In Chapter 2, you’ll learn more about these states and how you can either
take advantage of them or skip the staged part entirely.

Each file in your working directory can be in one of two
states: tracked or untracked. 

Tracked files are files that were in the last snap-shot;
they can be unmodified, modified, or staged. Untracked files are every-
thing else – any files in your working directory that were not in your last snap-
shot and are not in your staging area. When you first clone a repository, all of
your files will be tracked and unmodified because Git just checked them out
and you haven’t edited anything.

#### .gitignore file

Here is another example .gitignore file:
```
# no .a files
*.a
# but do track lib.a, even though you're ignoring .a files above
!lib.a
# only ignore the TODO file in the current directory, not subdir/TODO
/TODO
# ignore all files in the build/ directory
build/
# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt
# ignore all .pdf files in the doc/ directory
doc/**/*.pdf
```
- GitHub maintains a fairly comprehensive list of good .gitignore file ex-
amples for dozens of projects and languages at https://github.com/github/
gitignore if you want a starting point for your project.

- Viewing Your Staged and Unstaged Changes

`git status
`
- To see what you’ve changed but not yet staged:

` git diff
`
- If you want to see what you’ve staged that will go into your next commit

`git diff --staged
`
- To remove a file from Git you have to remove it from your tracked files (more
accurately, remove it from your staging area) and then commit.

`git rm file`

and also removes the file from your working directory so
you don’t see it as an untracked file the next time around.

- Another useful thing you may want to do is to keep the file in your working
tree but remove it from your staging area. In other words, you may want to
keep the file on your hard drive but not have Git track it anymore. This is partic-
ularly useful if you forgot to add something to your .gitignore file and acci-
dentally staged it, like a large log file or a bunch of .a compiled files. To do this,
use the --cached option:

`git rm --cached file`

- You can pass files, directories, and file-glob patterns to the git rm com-
mand. That means you can do things such as:

`git rm log/\*.log`  

- If you want to rename a
file in Git, you can run something like:

`git mv file_from file_to`

and then do a commit. 

- To see the commit history
 
` git log`

git log --help

- The --stat option prints below each commit entry a list of
modified files, how many files were changed, and how many lines in those files
were added and removed. It also puts a summary of the information at the end. It is the more useful for me.

 `git log --stat`
 
 - Another really useful option is --pretty
 
 `git log --pretty=oneline --> print in one line every commit`  
 
 - To print in a custom format
 `git log --pretty=format:"%h - %an, %ar : %s"`
 
 - To se the difference in each commit
 
 `git log -p`
 
 - To see the commits in a especific time there are --since and --until
 
 ` git log --since=2.hours`
 
 #### Undoing Things
 
 - If you want
to try that commit again, you can run commit with the --amend option:

 `git commit --amend`
 
 
 - As an example, if you commit and then realize you forgot to stage the
changes in a file you wanted to add to this commit, you can do something like
this:

``` 
git commit -m 'initial commit'
git add forgotten_file
git commit --amend
```
You end up with a single commit – the second commit replaces the results of
the first

#### Unstaging a Staged File

- To undo a file that you staged for error:

`git reset HEAD file`
 
#### Unmodifying a Modified File

- To discard changes in a working directory.It means that if you do not want to keep changes in a file in working directory. 

`git checkout -- file`

#### Remotes

- To see which remote servers you have configured, you can run

`git remote`
 
 command.
 
 - Origin is the dedault name Git gives to the server you  cloned from, so when you run the above command, the most probably output would be `origin`
 
 - And with the -v flag, git shows you the URLs that Git has stored for the shortname to be used when reading and writing to that remote:
 
`git remote -v`

#### Adding remote repositories

- We know how to clone a remote repo:
  
  ` git clone <url>`

-But a more fancy way to do it. To add a new remote Git repository as a shortname you can reference easily:

`run git add <shortname> <url>`


#### Fetching and Pulling from Your Remotes

- To get the data that has been pushed to that sever since you cloned (or last fetched) of this repo:

`git fetch <shortname> `

That command only downloads data to local repository -

- To do a pull of this repo:
   
 ` git pull <shortname> <branch> `
 
 automaticaly fetch an d then merge a remote branch into your current branch. 
 

### checkout is to copy files and switch betweem branches:

#### Technical Notes from visual guide:

The contents of files are not actually stored in the index (.git/index) or in commit objects. Rather, each file is stored in the object database (.git/objects) as a blob, identified by its SHA-1 hash. The index file lists the filenames along with the identifier of the associated blob, as well as some other data. For commits, there is an additional data type, a tree, also identified by its hash. Trees correspond to directories in the working directory, and contain a list of trees and blobs corresponding to each filename within that directory. Each commit stores the identifier of its top-level tree, which in turn contains all of the blobs and other trees associated with that commit.
 
#### Walkthrough: Watching the effect of commands

Start by creating some repository:
```
$ git init foo
$ cd foo
$ echo 1 > myfile
$ git add myfile
$ git commit -m "version 1"
```
Now, define the following functions to help us show information in a file show_status.sh:
```
show_status() {
  echo "HEAD:     $(git cat-file -p HEAD:myfile)"
  echo "Stage:    $(git cat-file -p :myfile)"
  echo "Worktree: $(cat myfile)"
}

initial_setup() {
  echo 3 > myfile
  git add myfile
  echo 4 > myfile
  show_status
}
```
Initially, everything is at version 1.
```
$ show_status
HEAD:     1
Stage:    1
Worktree: 1
```

We can watch the state change as we add and commit.
```
$ echo 2 > myfile
$ show_status
HEAD:     1
Stage:    1
Worktree: 2
$ git add myfile
$ show_status
HEAD:     1
Stage:    2
Worktree: 2
$ git commit -m "version 2"
[master 4156116] version 2
 1 file changed, 1 insertion(+), 1 deletion(-)
$ show_status
HEAD:     2
Stage:    2
Worktree: 2
```

Now, let's create an initial state where the three are all different.
```
$ initial_setup
HEAD:     2
Stage:    3
Worktree: 4
```
Let's watch what each command does. You will see that they match the diagrams above.

git reset -- myfile copies from HEAD to stage:

```
$ initial_setup
HEAD:     2
Stage:    3
Worktree: 4
$ git reset -- myfile
Unstaged changes after reset:
M   myfile
$ show_status
HEAD:     2
Stage:    2
Worktree: 4
```
git checkout -- myfile copies from stage to worktree:
```
$ initial_setup
HEAD:     2
Stage:    3
Worktree: 4
$ git checkout -- myfile
$ show_status
HEAD:     2
Stage:    3
Worktree: 3
```


git checkout HEAD -- myfile copies from HEAD to both stage and worktree:
```
$ initial_setup
HEAD:     2
Stage:    3
Worktree: 4
$ git checkout HEAD -- myfile
$ show_status
HEAD:     2
Stage:    2
Worktree: 2
```
git commit myfile copies from worktree to both stage and HEAD:
```
$ initial_setup
HEAD:     2
Stage:    3
Worktree: 4
$ git commit myfile -m "version 4"
[master 679ff51] version 4
 1 file changed, 1 insertion(+), 1 deletion(-)
$ show_status
HEAD:     4
Stage:    4
Worktree: 4

```
The important part is:

git reset -- myfile COPIES from HEAD to stage:

git checkout -- myfile COPIES from stage to worktree:

git checkout HEAD -- myfile COPIES from HEAD to both stage and worktree:

git commit myfile COPIES from worktree to both stage and HEAD: 

### Tutorial to do a PR

https://github.com/AeroPython/PyFME/wiki/Tutorial-paso-a-paso-del-flujo-de-trabajo

### Don’t type your password every time


Don’t type your password every time


If you don’t want to type it every single time you push, you can set up a “credential cache”. The simplest is just to keep it in memory for a few minutes, which you can easily set up by running:

`git config --global credential.helper cache` 

