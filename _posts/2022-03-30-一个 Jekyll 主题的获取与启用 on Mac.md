---
layout: post
title: 一个 Jekyll 主题的获取与启用 on Mac
---







## 一个 Jekyll 主题的获取与启用 on Mac

以一个免费主题为例，说明从网络选择主题模板到初次使用模板生成网页的全过程。

#### 选择主题

Jekyll 官网提供主题下载：http://jekyllthemes.org/，并可以根据关键词进行搜索。可以对所选主题进行演示。以下两个主题均来自该网站。本文以 Pinghsu Theme 模板为例。

Ed：http://jekyllthemes.org/themes/ed/

Pinghsu Theme：http://jekyllthemes.org/themes/Pinghsu-Theme/

下载主题的办法包括：1）由以上网页直接链接到 github.com 下载 master.zip 文件，解压后在本地生成网页主目录。2）如果已经浏览到 github.com 的某个主题页面，无需登录即可打开 Code 标签下的页面，找到绿色 Code 按钮，点开下拉菜单后点选 Download ZIP。3）如果已经登录自己的 github.com 账户，可以使用网站提供的 Fork 功能，类似于云盘的线上转存功能，转存到自己账户后，将其设置成 Master 项目。再通过 git 命令，或者 Github Desktop 桌面应用将项目完整的 Clone 到本地电脑。

Fork 后的主题已经可以在浏览器通过自己的 Github 地址浏览，但所见页面与主题演示完全一致，没有任何自己的更新。下载主题到本地电脑，就是要方便在本地更新，并利用 Jekyll 软件包将更新的网页先在本地模拟站点运行，确认无误后，再同步到 Github 空间。同步的过程可参考 git 命令或者 Github Desktop 桌面应用的相关资料。

#### 在 Mac 运行网站并浏览页面

最理想的情况下，执行命令：

jekyll serve

（参数见补充资料或执行 help 命令：jekyll help serve）

然后在浏览器地址栏输入 http://localhost:4000 或者 http://127.0.0.1:4000 即可见到网页，注意不是 https。

即使仅需要一句命令，也有注意事项：1）当前目录必须是下载模板解压后的主目录。可以执行命令：pwd 显示当前所在目录，ls 列表当前目录下的文件和子目录，cd 改变当前目录层级（无参数时默认返回用户主目录，cd .. 返回上一层）。2）某些情况下终端提示需要前缀 bundle exec，即要求执行命令：

bundle exec jekyll serve

（参见补充资料 bundle exec）

执行之前的终端提示例如：.../.gem/ruby/3.1.0/gems/bundler-2.3.9/lib/bundler/runtime.rb:309:in \`check_for_activated_spec!': You have already activated i18n 1.10.0, but your Gemfile requires i18n 0.9.5. Prepending \`bundle exec` to your command may solve this. (Gem::LoadError)

保险的做法是遵循如下几个步骤依此先后顺序执行。

#### 检查 Jekyll 相关安装和更新是否正确

顺序执行命令：

jekyll new test-site，相当于合并 jekyll build 和 bundle install（参考补充资料），默认使用名叫 Minima 的主题，可以带参数 --blank 使用空白模板。Minima 主题在执行下面的命令时可能会提示缺少 webrick，而带 -- blank 则会避免该提示（此时不生成 Gemfile 和 Gemfile.lock）。

cd test-site

bundle exec jekyll serve，如果你的站点没有 Gemfile 和 Gemfile.lock 文件，则可以省略 bundle exec 命令，直接运行 jekyll serve 命令。注意，可能提示缺少 webrick 这个 gem，因为由于 Ruby 的发展，默认在新的版本中需要手动添加 webrick，此时使用命令 bundle add webrick。类似的情况还有 rexml、rss 等 libraries。"The users need to add rexml to their Gemfile after Ruby 2.8(3.0)"。

浏览 http://localhost:4000 应显示正常空网页。需要帮助可以运行 jekyll help new，jekyll help build。

如有错误提示，按提示依次解决。极端情况下可能还要运行 brew doctor 检查其它软件的错误。或者重新执行命令，通过 RubyGems 安装 Jekyll 和 bundler 这两个 gems：

gem install jekyll bundler

关于 Jekyll 的安装和 bundler 的更多信息，请参见专门文章。

#### 启用 Pinghsu Theme

实际操作中，进入主题目录后，首先执行命令：

jekyll serve 

出现错误提示：Could not find public_suffix-3.0.3 in any of the sources (Bundler::GemNotFound)。

可能缺少必要的依赖关系，也可能主题要求的 gems 和 libraries 的版本太老，尝试命令 bundle install 和其它查看命令。经反复碰壁之后，采取了删除 Gemfile.lock 文件的办法，避免安装旧版的 Ruby、gems 和 libruaries。删除后，为安装必要的依赖关系重新执行命令：

bundle install

结尾部分返回：Bundle complete! 5 Gemfile dependencies, 29 gems now installed.
Use \`bundle info [gemname]` to see where a bundled gem is installed.

命令执行成功。或可尝试修改 Gemfile 和 Gemfile.lock，添加版本信息，而不是暴力删除。

执行命令：

bundle exec jekyll serve

提示缺少 Webrick 和 Rexml 之后进行了手动添加，执行两条命令：

bundle add webrick

bundle add rexml

再次执行命令：

bundle exec jekyll serve

最终成功在本地 Mac 上运行并浏览。



#### 补充资料

1. jekyll serve：开发服务将会运行在 http://localhost:4000，自动生成更新会被开启，如果不想开启请使用 --no-watch。

   jekyll serve --no-watch：等同于 jekyll serve，但是内容更改时不会自动生成新的。

   jekyll serve --livereload：将在更新后刷新浏览器页面。

   jekyll serve --incremental：将会匹配更改部分，执行部分构建以减少生成更新的时间。

   jekyll serve --detach：等同于 jekyll serve，但是不会在当前终端中显示运行状态，而是转为后台模式。如果需要关闭服务了，可以 kill -9 1234。这里的 1234 代表 PID 号。如果不知道 PID，那么就执行 ps aux | grep jekyll，并关闭这个实例。

2. bundle exec：Execute a command in the context of the bundle; executes a command, making all gems specified in the Gemfile(5) available...; modify the gem method to be a no-op if a gem matching the requirements is in the bundle, and to raise a Gem::LoadError if it's not; also implicitly modifies Gemfile.lock if the lockfile and the Gemfile do not match...The Gemfile and lockfile must be synced in order to bundle exec successfully, so bundle exec updates the lockfile beforehand. https://bundler.io/v2.3/man/bundle-exec.1.html

   a) If you use bundle exec before any command, It will search for the Gems which are mentioned in our Gemfile, in application home folder. Suppose, You have 2 applications, and using different ruby versions for each of them. Without bundle exec the command might be failed to run, as it may take different version Gem to run that task. But If you start using bundle exec it will take the exact gem version to run the task/application. It is recommendable to use bundle exec before any command.

   b) "BUNDLE_GEMFILE=/path/to/gemfile bundle exec" can be used to precede any command (if BUNDLE_GEMFILE is not specified it searches up the file system and uses the first one it finds), not just rake. Any command you run may invoke executable Ruby commands (such as rake) or require code from Ruby libraries (such as the Rake::Task class), and these things are typically provided by gems. gem env tells you where the gem-provided libraries and executables are. However if you use bundle exec it restricts the available gems to those specified in the Gemfile.lock file associated with the Gemfile your bundle exec context is using.

   Using all the available gems on your machine (which can happen if you don't do bundle exec) can be undesirable for a couple reasons: You may have incompatibilities in your full gem set. It's harder to tell exactly what gems you're using, adding some unpredictability to your working environment.

   c) Suppose you have version 2.0 of the fictitious whateva-whateva gem installed on your *system* for some valid reason. You decide you want to pull down an old Rails project from somewhere (maybe Github)  to check it out and run "bundle install" within the cloned project's root folder. That command will install all of the gems that the Rails app requires and one of them happens to be verison 1.0 of the fictitious whateva-whateva gem.

   So the current state is this: your old rails app has a gem bundle that includes an older version of the whateva-whateva and your *system* wide gems include a newer version of the whateva-whateva gem. When you run rake tasks associated with your Rails app, which version do you want loaded? The older one of course.

   In order to do this you can use "bundle exec rake the:task" and it runs the rake command within the context of your bundle -- the older version of the gem plus whatever other stuff was specified in the old rails app's Gemfile.

   d) bundle exec 可以在当前 bundle 的上下文中执行一段脚本。通过它可以调用本项目中指定的 rake 版本来执行命令。

   下面是官方文档的说明。In some cases, running executables without bundle exec may work, if the executable happens to be installed in your system and does not pull in any gems that conflict with your bundle. However, this is unreliable and is the source of considerable pain. Even if it looks like it works, it may not work in the future or on another machine.

   所以我们只要每次执行命令的时候加上 bundle exec 的前缀就可以。但是这样搞的很烦人，试想每天都要敲上百次这样的字符，而且还常常忘记。有一个方法可以避免每次都要键入bundle exec作为前缀。安装 rubygems-bundler。执行命令 gem install rubygems-bundler，然后运行 gem regenerate_binstubs，...，最后运行 cd your_project_path，bundle install --binstubs=./bundler_stubs。参见官网 https://github.com/mpapis/rubygems-bundler。

3. jekyll build：当前文件夹中的内容将被生成到 ./_site 目录中。

   jekyll build --destination \<destination>：当前文件夹中的内容将被生成到 \<destination> 目录中。

   jekyll build --source \<source> --destination \<destination>：\<source> 文件夹中的内容将被生成到 \<destination> 文件夹中。

   jekyll build --watch：当前文件夹中的内容将被生成到 ./_site 中，检查改动，并重新自动生成。

4. bundle install：安装所需的依赖关系，会随 jekyll new 命令自动启动。

5. 