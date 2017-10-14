## Git diff
git diff compares the difference between working dir and index.
There are three versions of files:
    1. working files, normal files to work with;
    2. index files for git, actually hash of files. The copy of files are stored as blobs.
    3. commit files for git, ditto.
git diff different_branch
In vim-fugitive, Git diff bring up the staged version of the file side by side with the working tree version.
:checkt[ime] to update all vim buffers.

### Solve conflicts
If merge conflicts, manually resolve and then `git add` and `git commit`.
If rebase conflicts, do the resolution and then `git rebase --continue`, or skip the commit which creates the conflict `git rebase --skip`. or abort `gir rebase --abort`.
List the files which have a conflict: `git diff --name-only --diff-filter=U`.
**Picking theirs or ours for conflicting files** : `git checkout --ours|theirs file_name`
**Merge Strategy**: ` git merge -s recusive -X ours|theirs` pick one of the versions in case of conflicts.
Enforing a merge commit instead of fast forwarding: `git merge --no-ff`

### Revert
1. revert changes in files in the working tree to its latest staged version.
    git checkout (--) file_name // acutally wheter the staged version is the same or not as commit states, it will check out the staged version
2. removes changes in working tree and index:
    git reset pointers -- dir/file_name
3. revert a change of file in index:
    git reset file_name
4. correct the last commit message:
    git commit --amend -m "Correct msg" 
5. revert the change of a commit:
    git revert commit_id

### Tags
1. list tags:
    git tag or git tag -l <pattern>
2. create tags:
    git tag 2.0.1 -m "comments" //tag current HEAD
    git tag 2.0.0 -m "version 2.0.0" [commit id]
3. push tags:
    git push origin [tagname]
    git push --tags // push all tags
4. delete tag:
    git tag -d tag_name // delete local tags
    git push origin :refs/tags/1.7.0
5. get tags:
    git fetch --tags

### Branches
1. delete a remote tracking branch locally actually:
    git branch -d origin/[remote_branch] // git fetch will create remote tracking branches again.
2. delete a branch in a remote repo:
    git push [remote] :branch
3. tracking branches allow yuo to use `git pull` or `git push` directly without specifying the branch and repository. If cloning, a local master branch is created as a tracking branch for origin/master.
e.g. master is a tracking branch and origin/master is a remote tracking branch.
    1. create a tracking branch:
        git checkout -b newbranch origin/newbranch
        git branch newbranch origin/newbranch // default tracking if created from a remote tracking branch
    2. create branches specifing tracking option:
        git branch --no-track new_no_trakcing origin/branch
        git branch -u origin/master new_branch_notrack //update to a tracking branch 
    3. update remote tracking branch:
        git fetch origin
