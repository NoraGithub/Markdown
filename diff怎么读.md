
# git diff
[（谷歌）diff 原理](https://www.google.com.hk/search?newwindow=1&safe=strict&q=diff+%E5%8E%9F%E7%90%86+&oq=diff+%E5%8E%9F%E7%90%86+&gs_l=serp.3..0i30k1j0i5i30k1l2.68361.68361.0.68637.1.1.0.0.0.0.83.83.1.1.0....0...1c.1.64.serp..0.1.82.DtZbCTJhHNs)
[Diff 算法的原理是什么, 怎样学习和理解?](https://segmentfault.com/q/1010000000367833)
[diff程序的算法](http://www.voidcn.com/blog/dog250/article/p-5859714.html)
[读懂diff（阮一峰）](http://www.ruanyifeng.com/blog/2012/08/how_to_read_diff.html)
[Interpreting git diff -cc[was:merge conflict]](https://lists.gnu.org/archive/html/emacs-devel/2010-01/msg01182.html)
[git diff --cc 怎么读](https://www.google.com.hk/search?newwindow=1&safe=strict&q=git+diff+--cc+%E6%80%8E%E4%B9%88%E8%AF%BB&oq=git+diff+--cc+%E6%80%8E%E4%B9%88%E8%AF%BB&gs_l=serp.3...8438.11111.0.11377.11.9.2.0.0.0.227.772.3j2j1.6.0....0...1c.1.64.serp..3.0.0.bCGBg-Hw7IU)

#git push --force 的使用场景
git checkout -b dev
git add -all
git checkout master

**initialization后，从未`git commit` ，checkout后git branch --list 也看不到自己的分支。**[如何在git里撤销几乎任何操作](blog.jobbole.com/87700/)

#文件的命名不要带有`/`
[windows下不接受这种命名](https://www.google.com.hk/search?q=invalid+path+git+pull&ie=utf-8&oe=utf-8&gws_rd=cr,ssl)
导致git pull 后git rm，永远无法和remote repository保持一致。
这个时候可能一定会遇到push force问题。（由于是没用的文件，我直接舍弃了该文件。）


##git push --force 的使用场景
push错误后，
[git 怎样删除远程仓库的某次错误提交？](https://segmentfault.com/q/1010000002898735)
[push后撤销](https://www.google.com.hk/search?q=push%E5%90%8E%E6%92%A4%E9%94%80&ie=utf-8&oe=utf-8&gws_rd=cr,ssl)

## 
