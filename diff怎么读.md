For those who don't get git diff --cc.

Note that in the quotes of the diff below, the first three characters
" > " are email quoting, and the rest of the line would normally
appear flush left in a diff.  Column references start from 1, and do
*not* include the email quoting columns.

Teemu Likonen writes:

 > $ git show b8de0d6

 > diff --cc doc/emacs/ChangeLog

diff --cc is a git-specific mode of diff (git diff, not GNU diff)
which handles multiple parent commits specially.

 > index e9ab5ed,7d7002a..39ffa2c

The index header shows abbreviated SHA1 IDs for the two parents (on
the left-hand side of the ..), and the merge commit (on the right-hand
side.


 > --- a/doc/emacs/ChangeLog
 > +++ b/doc/emacs/ChangeLog
 > @@@ -1,8 -1,7 +1,12 @@@

The tripled @@@ marks this as a git diff --cc.  patch will choke on
that, so git diff --cc diffs cannot be applied.

 > - 2010-01-19  Mark A. Hershberger  <address@hidden>
 > ++2010-01-24  Mark A. Hershberger  <address@hidden>

As you can see above, instead of a single unified diff marker, there
are two.  The left hand side are Mark's changes (7d7002a..39ffa2c),
the right hand side are those of the trunk which was merged into
Mark's branch (e9ab5ed..39ffa2c).  The two lines above show that (1)
Mark changed the date of this log message in his branch when resolving
conflicts etc in the merge, and (2) this had the effect of adding the
line relative to the trunk.

In detail, in the first line, the "-" in column 1 deletes this line
from the version in Mark's branch, and the space in column 2 makes no
change to the version in the trunk.  That is correct because this line
wasn't in the trunk version, so it can't be deleted.  In the second
line, the "+" in column 1 indicates this line was added in Mark's
branch (ie, when combined with the deletion above, it replaces the
original line dated "2010-01-19" with a new line dated "2010-01-24").
The "+" in column 2 indicates that this line was added by comparison
to the trunk version.

 >  +
 >  +   * programs.texi (Other C Commands): Replace reference to obsolete
 >  +   c-subword-mode.
 >  +

Relative to Mark's branch, they were already there and are left
unchanged, as indicated by the " " in column 1.  Relative to the
trunk, the above lines were *added*, as indicated by the "+" in column
2.

 > + 2010-01-21  Glenn Morris  <address@hidden>
 > + 
 > +    * trouble.texi (Bugs): Fix PROBLEMS keybinding.
 > + 

Relative to Mark's branch, these were added, as indicated by the "+"
in column 1.  Relative to the trunk, the above lines were already
there, as indicated by the " " in column 2.

*All* of the changes above were made relative to the common ancestor
commit (ie, the last commit before Mark's branch diverged from the
trunk).  In the merge, Mark arranged that his log message appears at
the top of the ChangeLog, as is GNU Emacs policy for new logs IIUC.

 >   2010-01-12  Glenn Morris  <address@hidden>
 >   
 >      * trouble.texi (Checklist): Use bug-gnu-emacs rather than

The above lines were present in the common ancestor, are unchanged in
both branches as indicated by " " in both columns 1 and 2, and are
presented here for context.

Thus, by looking for "+" in column 2, you can see lines which were not
in the trunk.  Ie, those are the lines added by Marks branch.

HTH