# Mercurial
In comparison to Git, Mercurial requires a shallower understanding to operate in a useful manner.

#Logical Archeticture
1. History model
Git basic object: blob, tree, and commit
Mercual: file, manifest, and changeset
In addition to global SHA1, mercurial also provides a loal revision, a simply incrementing integer, for each changeset, in addition to the reverse count notation provideded by Git(HEAD~4).
Mercurial's view of history is , just like Git, a DAG.

2. Branch Model
Git has its famous lightweight branches, which is just a pointer and you can do whatever with a pointer.
In Mercurial, branches are called heads and they can referred by their changeset identifier, liking using **Git detached head**s intead of branche names. If no branch name was set, the "default" is assigned.
Mercurial also has named branches, but it is disable in Fb.

3. Tag omdel
In Git, global tags hasve a dedicated repo object type, called annotated tags.
IN Mercurial, they are stored in .hgtags for versioning.

#Behaviorial differences
1. Communication between repositories
Git has the notion of trakcing branch and these allow to selectively push or pull branches from or to a remote repo.
Mercurial: when pull, bring all remote heads into your local repo.

2. Staging area
Git is the only one that exposes the concept of index or staging area.
Mercurial has DirState, but it is done automatically. You can do `hg commit --interative` or specify the files.

3. Bare repo
Mercural: hg update null
Git needs special configurations.
