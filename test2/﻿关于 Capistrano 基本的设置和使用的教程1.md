[关于 Capistrano 基本的设置和使用的教程](/michael_luthor/article/details/9615797)
=================================================================================

这是一个关于 `Capistrano` 基本的设置和使用的教程。他将不会为你介绍 绑定了 `Capistrano` 的deployment system, 但是作为替代本教程着重于常规领域的执行 `Capistrano` 和编写属于你自己的食谱（配置复合自己胃口的 `Capistrano`）。本教程主要吸引那些想使用 `Capistrano` 来达到 non-deployment domains（自动化部署）的人以及使得这些人对 `Capistrano` 更加了解。

### Installation

`Capistrano` 实际上是由 `Capistrano` gem 组成，以及

    # gem sources -a http://gems.github.com/
    # gem -v

你的 RubyGems 版本应该至少是 `1.3.x`, 如果不是请跟随他们的升级说明书（升级 RubyGems）然后回到这里... 当 RubyGems 更新过后，你可以安装 `Capistrano` 及其依赖通过如下（命令）：

    # gem install capistrano

### Assumptions 假设

`Capistrano` 对于你的服务器做了一些假设。为了使用 `Capistrano`,你将需要完成这些假设：

-   你正在使用 `SSH` 来访问你的远程机器。`Telnet` 和 `FTP` 不被支持。
-   你的远程服务器有一个 `POSIX-compatible`(POSIX兼容)的 shell 被安装。Shell必须被称为 "sh"以及它必须处于默认的系统路径。
-   如果你正在使用密码来访问你的服务器，他们全都都必须拥有相同的密码。由于这通常不是一个好主意，访问你的服务器的更好的方式是是通过一个 public key。确保你的 key 已经有了一个很强壮的密码。

`POSIX` 是IEEE为要在各种UNIX操作系统上运行的软件，而定义API的一系列互相关联的标准的总称，其正式称呼为IEEE 1003，而国际标准名称为ISO／IEC 9945。此标准源于一个大约开始于1985年的项目。POSIX这个名称是由理查德·斯托曼应IEEE的要求而提议的一个易于记忆的名称。它基本上是Portable Operating System Interface（可移植操作系统接口）的缩写，而X则表明其对Unix API的传承。

`Capistrano` 同样假设你你对计算机有一定熟悉：你应该对使用 command-line 方式来工作感到很舒适。你没有必要了解高级的 shell 脚本的技术或者其他比如（即使它可能会有帮助，如果你想着手编写复杂的`Capistrano` 配置文件），但是你应该能够导航到目录并且从命令行执行命令。`Capistrano` 没有GUI界面；它完全是命令行驱动的。

要想掌握 `Capistrano` 全部的高级技巧，你应该对使用 `Ruby` 编程语言感到舒适（或者至少，最少熟悉）。当你编写你自己的 `Capistrano` 任务时，你（需要）通过 Ruby 来做。

### Capfile

`Capistrano` 从一个名为 `capfile`的文件读取它的配置说明。（对于你熟悉的 `make` 或者 `rake` 使用工具，`capfile` 的概念就好像是一个 `makefile` 或者 ` rakefile`。）如果你创建一个名叫 `capfile` 的文件（或者 `Capfile`，在于你的喜好），`Capistrano` 将会读取那个文件并且处理里面的说明。

（通过）`Capfile` 你可以告诉 `Capistrano` 关于你希望连接的服务器并且在这些服务器上你希望执行的任务。它本质上仅仅是一个 Ruby 脚本，但是扩充了大量的 "helper" 语法，来使得定义服务器角色和任务更加简单。（使用行话来说， `Capfile` 是使用定制的基于Ruby的 DSL（专业领域语言）编写的。）

你可以使用你希望的任何编辑器来写你的 Capfiles；他们仅仅是简单的文本文件。我推荐一些为程序员设计的编辑器，如 vim, emacs, TextMate, Eclipse 等四个。选择你用得舒服的，**但是确保你所选择的编辑器可以保存这些文件为纯文本格式，并且不会为文件名自动添加类似 `.txt` 的扩展名**。`Capfile` 应该被称为 "capfile" 或者 "Capfile"，没有任何扩展名。

**Note:** 因为 `capistrano` 是十分有用的，他将会搜索你的 file tree 直到它找到了一个名叫 `capfile` 的文件，这里意图确保你在你的应用程序的任何位置，你尝试并运行 `cap`,它将会找到正确的 ` capfile`；this has been known to catch people out though；你的 home 目录也会被搜索。

### A Simple Example 一个简单的例子

闲聊了太多了，来看看一个很简单的 `capfile`，来明白它会是什么样的：

    task :search_libs, :hosts => "www.capify.org" do
      run "ls -x1 /usr/lib | grep -i xml"
    end

这里定义了一个简单的任务，叫做 “search\_libs”，并且指出了它将会只在 “www.capify.org” 主机上执行。当执行的时候，他将会显示所有的文件和 `/usr/lib` 中名字包含 "xml的子目录"。默认情况， "run" 将会显示所有的输出在控制台。

假设你的 `capfile` 在当前目录，你像如下下面这样执行任务(从命令行)：

    $ cap search_libs

你可以定义多个任务如果你喜欢：

    task :search_libs, :hosts => "www.capify.org" do
      run "ls -x1 /usr/lib | grep -i xml"
    end
    task :count_libs, :hosts => "www.capify.org" do
      run "ls -x1 /usr/lib | wc -l"
    end

Here we’ve added a second task, “count\_libs”, which will display the number of entries in /usr/lib. We could add more, but even with just two tasks, having to specify the host over and over is getting a little unwieldy. Here is where “roles” come into play:
 这里我们添加了第二个任务，“count\_libs”，他将会显示整个 `/usr/lib` 的数目。我们可以添加更多，但即使只有两个任务，一遍又一遍的指定主机，有一点点笨拙。这里是 "roles" 上台了：

        role :libs, "www.capify.org"
        task :search_libs do
          run "ls -x1 /usr/lib | grep -i xml"
        end
        task :count_libs do
          run "ls -x1 /usr/lib | wc -l"
        end

We’ve created one new role, called “libs”, and associated “www.capify.org” with that role. By default, a task will be executed on all servers in all roles, so we were able to drop the :hosts declaration from the tasks. Much simpler!
 我们创建一个新的 `role`，叫做 “libs”，并且分配 “www.capify.org” 给那个 `role`。默认情况，一个任务将会在所有服务器的所有角色中执行，因此我们能够从任务 drop `:hosts` 声明。非常简单！

（注意：无论你当前使用的哪个用户登录到你本地的机器，Capistrano的登录都是默认的。如果你需要用不同的用户登录，你可以编码用户名到服务定义中, “joe@www.capify.org”。另外，你可以设置 `:user` 变量给你想使用的用户名。我们将会一会儿介绍变量。）

### Gateway Servers 网关服务

在真实的世界中，我们不得不担心坏孩子尝试潜入我们的服务器，因此许多服务器集隐藏在 `NAT`s 和防火墙之后，来防止直接访问。作为替代，你不得不登录到一些 “网关” 服务器，并且从这里登录到你希望的服务器。

`Capistrano` 通过你定义一些网关服务来支持这个场景。所有的后续连接将会从隧道到gateway（使用 SSH 转发端口）。来告诉 `Capistrano` 关于你的网关服务，你可以简单地这样做：

        set :gateway, "www.capify.org"
        role :libs, "private.capify.org"

这里，假设 `private.capify.org` 在一个 NAT 后面，并且不能直接访问。通过设定到 “www.capify.org” 的 `:getway` 值，我们通知 `Capistrano` 为了访问任何其他的服务器，必须首先建立一个连接到 “www.capify.org”，并且通过其隧道化后续连接。

### Multiple Servers 多个服务器

在你的服务器上一切顺利，但是负荷的攀升，然后你决定你需要添加另外一个(服务器)。你仍然可以查询这些服务通过一个简单的任务，通过...

Easy enough:

        role :libs, "private.capify.org", "mail.capify.org"

现在，当你执行 `cap search_libs` 或者 `cap count_libs`，命令将会在两个服务器上并行执行，以及输出汇总到一个单个的流(single stream)或者显示到你的控制台中。

### Multiple Roles

在（编写教程的）路上，让我告诉你来添加一个文件服务器来混合（来承载所有这些盗版MP3，你服务的，你这淘气的人，你）。因为它并没有真正的 `_running_` 任何东西，你不必太过担心安装在这里的库，**因此你不希望 `search_libs` or `count_libs` 在这里运行**。同时，你有一个任务它会显示你仅仅运行文件服务的的剩余磁盘空间。这该怎么做？

        role :libs, "crimson.capify.org", "magenta.capify.org"
        role :files, "fuchsia.capify.org"
        task :search_libs, :roles => :libs do
          run "ls -x1 /usr/lib | grep -i xml"
        end
        task :count_libs, :roles => :libs do
          run "ls -x1 /usr/lib | wc -l"
        end
        task :show_free_space, :roles => :files do
          run "df -h /"
        end

这相当的简单。我们仅仅添加了另一个角色（“files”），并且添加 `:roles` 来限制每一个任务，指定哪个角色与之对应。当我们运行 `search_libs`，它将会仅仅在定义为`libs` 角色的中的服务器运行。相似的，`show_free_space` 将会仅仅运行在定义为 `files` 角色的服务器中。

(Note: 你可以任务在指定服务器以多个角色运行,通过一个以角色名组成的数组并对应到 `:roles`。)

### Documenting Tasks

Capistrano tasks can be considered to be self-documenting, there are a couple of Rake-inspired ways to get a list of all the tasks Capistrano can run for a given installation:
`Capistrano tasks` 可以被认为是自解释的，这里有一系列的 `Rake-inspired` 方式。要得到一个所有 `Capistrano` 任务可以运行一个提供的 installation：

    $ cap -T

Will list all tasks with a definition, the output may look something like this:

    cap deploy               # Deploys your project.
    cap deploy:check         # Test deployment dependencies.
    cap deploy:cleanup       # Clean up old releases.
    cap deploy:cold          # Deploys and starts a `cold' application.
    cap deploy:migrate       # Run the migrate rake task.
    cap deploy:migrations    # Deploy and run pending migrations.
    cap deploy:pending       # Displays the commits since your last deploy.

如果你添加了一个没有描述的任务，他将不会显示在这个列表中，除非详细版：

    $ cap -vT

默认任务（安装）的输出是相同的，但是，除非你的任务只有一个名字，他们应该只出现在详细的名单。

To add a description to a task, see the following short example:
 给一个任务添加一个描述，看如下简短的例子：

        desc "Echo the server's hostname"
        task :echo_hostname do 
          run "echo `hostname`"
        end

### cap invoke

Cap invoke is a simple command, when called on the command line, you can simultaneously send one command to all servers:
 Cap invoke是一个简单的命令，当其在命令行被调用的时候，你可以同时发送一个命令到所有的服务器：

    cap invoke COMMAND="echo 'Hello World'"

If for some reason you want to run a command using sudo, you may find the following syntax useful:
 如果因为一些原因你希望使用`sudo` 运行一个命令，你可能发现随后的语法很有用：

    cap invoke COMMAND="echo 'Hello World'" SUDO=1

### cap shell

As nifty as “invoke” is, it has at least one significant drawback: every time you invoke a command, it has to reestablish connections to the servers. This isn’t a big deal at all if you’re only executing a single command, but if you have two or three that you want to run, it can quickly get annoying, having to wait for the connections to be made.
 如同 “invoke” 一样别致，它至少有一个显着的缺点：每次你 `invoke` 一个命令，它不得不重建连接到服务器。这不是一个大麻烦，如果你仅仅执行一个单个的命令，但是如果你有两到三个需要执行，它会变的相当烦人，不得不为产生的连接等待。

Enter “cap shell”. This gives you an interactive prompt from which you can enter adhoc commands and even execute tasks, and all connections that are established during the duration of the shell session are cached and reused. That means that if you execute a command and it has to connect to three different servers, the next time you execute a command that needs to connect to those same servers, the connections are reused. Really slick! It’s a really handy tool for all kinds of system administration tasks.
 输入 “cap shell”，这会提供你一个互动提示符（`$`）通过它你可以输入 adhoc（特定的）命令或者甚至执行任务，并且在shell会话被缓存或者拒绝期间所有的连接都是成立的。意思是如果你执行了一个命令并且它连接到了三个不同的服务器，下次你需要执行一个命令也需要连接到相同的服务器，连接是被拒绝的。真圆滑！这是一个管理系统各种任务的非常方便工具。

就像 “invoke”， “shell” 任务通过 `host` 或者 `role` 来让你包装命令和任务。你可以在 shell中的任意时间输入 `help`，来获得更多信息。

### Namespaces

Since Capistrano 2.x all tasks are namespaced, this simply means that tasks are separated out into logical modules... most of the tasks that ship with the default recipes are broken into either a **deploy** or a **web** namespace, the deploy namespace contains all the tasks used in the [Default Deploy Path](http://ruby-china.org/wiki/Default_Deploy_Path), whilst **web** contains a couple of useful helpers for showing and hiding a "Upgrade in Progress" page to your users during a deploy.
 自从 `Capistrano` 2.x 所有的任务都被名称空间包含了，其简单的意思是任务被分割到逻辑模块中...
