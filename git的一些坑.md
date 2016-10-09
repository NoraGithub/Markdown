
weibo的git使用笔记。
[Git远程操作详解](http://www.ruanyifeng.com/blog/2014/06/git_remote.html?bsh_bid=1314465090)
[Git 使用规范流程](http://www.ruanyifeng.com/blog/2015/08/git-use-process.html)
[git rebase 阮一峰 谷歌搜索](https://www.google.com.hk/search?q=git+rebase++%E9%98%AE%E4%B8%80%E5%B3%B0&ie=utf-8&oe=utf-8&gws_rd=cr,ssl)
[Error:cannot pull with rebase:you have unstated changes](http://stackoverflow.com/questions/23517464/error-cannot-pull-with-rebase-you-have-unstaged-changes)
[you have unstated changes](https://www.google.com.hk/search?newwindow=1&safe=strict&q=you+have+unstaged+changes++&oq=you+have+unstaged+changes++&gs_l=serp.3..0j0i30k1l6j0i5i30k1l3.68687.78840.0.79129.12.12.0.0.0.0.188.1132.6j4.10.0....0...1c.1.64.serp..2.1.82.6SnD5AXtjG0)
[git workflow 常用命令](http://www.cnblogs.com/kidsitcn/p/4450466.html)
[多人协作](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013760174128707b935b0be6fc4fc6ace66c4f15618f8d000)
[分支管理策略](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013758410364457b9e3d821f4244beb0fd69c61a185ae0000)
[管理修改](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001374829472990293f16b45df14f35b94b3e8a026220c5000)

[探索.git目录](http://www.cnblogs.com/zhongxinWang/p/4235448.html)

# git diff
[（谷歌）diff 原理](https://www.google.com.hk/search?newwindow=1&safe=strict&q=diff+%E5%8E%9F%E7%90%86+&oq=diff+%E5%8E%9F%E7%90%86+&gs_l=serp.3..0i30k1j0i5i30k1l2.68361.68361.0.68637.1.1.0.0.0.0.83.83.1.1.0....0...1c.1.64.serp..0.1.82.DtZbCTJhHNs)
[Diff 算法的原理是什么, 怎样学习和理解?](https://segmentfault.com/q/1010000000367833)
[diff程序的算法](http://www.voidcn.com/blog/dog250/article/p-5859714.html)
[读懂diff（阮一峰）](http://www.ruanyifeng.com/blog/2012/08/how_to_read_diff.html)
[Interpreting git diff -cc[was:merge conflict]](https://lists.gnu.org/archive/html/emacs-devel/2010-01/msg01182.html)
[git diff --cc 怎么读](https://www.google.com.hk/search?newwindow=1&safe=strict&q=git+diff+--cc+%E6%80%8E%E4%B9%88%E8%AF%BB&oq=git+diff+--cc+%E6%80%8E%E4%B9%88%E8%AF%BB&gs_l=serp.3...8438.11111.0.11377.11.9.2.0.0.0.227.772.3j2j1.6.0....0...1c.1.64.serp..3.0.0.bCGBg-Hw7IU)
[sublime  diff](https://github.com/colinta/SublimeFileDiffs)
[20 个强大的 Sublime Text 插件 ](http://www.oschina.net/translate/20-powerful-sublimetext-plugins)

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
