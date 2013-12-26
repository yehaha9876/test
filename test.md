Capistrano集群部署CloudFoundry 
==============================
       开源的cloudfoundry部署时有两个必须解决的问题，一是服务器部署，如多少个cc节点，dea节点，官方推荐用BOSH，由于BOSH比较依赖底层IAAS，于是采用了手动部署的方案。




     
另一个问题是，这么多节点，代码修改了，如何更新至服务器呢？体力活显然是不能够的，或许BOSH也提供了类似方案，没用BOSH也就没深研，找到一个ruby界很有名气的开源工具capistrano。它既可以独立用，也可以集成chef，puppet等。功能还是很强大的。


 

一、什么是capistrano
====================






 A remote server automation and deployment tool written in Ruby {style="margin:0.2em 0px 0.5em; padding:0px; direction:ltr; font-family:Enriqueta,serif; font-weight:400; color:rgb(59,82,138); line-height:1.4"}
----------------------------------------------------------------




 
 
 
 

二、基本原理
============


 

       
原理很简单，就是利用ssh。ssh上服务器---\>执行脚本---\>返回。虽然简单，但是capistrano做了很多事情，让你比自己写ssh容易太多，而且内置了执行事务，回滚操作。通过capi部署，它会在服务器上维护每次release的版本列表，很方便实现rollback操作。



       说到ssh ，就有必要聊到shell的几种模式： non-login  login
non-interactive
interactive，区别可以google下。capi默认是non-login，non-interactive。原因也提到了，这样会少加载很多不必要的服务器的环境变量，而且也是减少会服务器端环境配置的影响。



服务端环境的变化对capi的来说，是不关心的，也不应该关心。但是这种模式也给脚本编写带来一些不便，后面会提到。


 

三、安装
========





~~~~ {.ruby name="code"}
gem install capistrano -v 2.15.5
gem install capistrano-ext
~~~~


 \


 

capistrano-3.0最近刚发布，网上资料比较少，目前用的是2.15版本。


 

四、目录结构
============


 

创建一个目录，cd至目录执行capify .  就会完成工具的初始化。


 



~~~~ {.ruby name="code"}
admin@ubuntu:/tmp$ mkdir test
admin@ubuntu:/tmp$ cd test/
admin@ubuntu:/tmp/test$ ls 
admin@ubuntu:/tmp/test$ capify .
[add] writing './Capfile'
[add] making directory './config'
[add] writing './config/deploy.rb'
[done] capified!
~~~~




 

五、部署流程
============


 
 
 
 
 



~~~~ {.java name="code"}
1. cap deploy:setup                        #在服务器创建必要的目录，用来存放每次的release记录等
2. cap deploy:check                        #检查一些目录是否存在
3. cap deploy                              #deploy入口
         deploy:update                     #
               deploy:update_code          #
                      strategy.deploy!     #根据参数更新代码，保存release
                      deploy:finallize_update  #更新文档的时间等
         deploy:create_synlink                 #将current目录ln至最新的release
         deploy:restart                        #重启服务
~~~~


 \

~~~~ {.java name="code"}
admin@ubuntu:~/capistrano/dea$ cap deploy
    triggering load callbacks
  * 2013-10-14 17:58:40 executing `development'
    triggering start callbacks for `deploy'
  * 2013-10-14 17:58:40 executing `multistage:ensure'
  * 2013-10-14 17:58:40 executing `deploy'
  * 2013-10-14 17:58:40 executing `deploy:update'
 ** transaction: start
  * 2013-10-14 17:58:40 executing `deploy:update_code'
    updating the cached checkout on all servers
    executing locally: "git ls-remote https://code.jd.com/dea_v2 master"
    command finished in 688ms
  * executing "if [ -d /tmp/jae/dea/shared/cached-copy ]; then cd /tmp/jae/dea/shared/cached-copy && git fetch -q origin && git fetch --tags -q origin && git reset -q --hard 08a647586cf4285019ced6ebde34d427ff65214a && git submodule -q init && git submodule -q sync && export GIT_RECURSIVE=$([ ! \"`git --version`\" \\< \"git version 1.6.5\" ] && echo --recursive) && git submodule -q update --init $GIT_RECURSIVE && git clean -q -d -x -f; else git clone -q -b master https://code.jd.com/dea_v2 /tmp/jae/dea/shared/cached-copy && cd /tmp/jae/dea/shared/cached-copy && git checkout -q -b deploy 08a647586cf4285019ced6ebde34d427ff65214a && git submodule -q init && git submodule -q sync && export GIT_RECURSIVE=$([ ! \"`git --version`\" \\< \"git version 1.6.5\" ] && echo --recursive) && git submodule -q update --init $GIT_RECURSIVE; fi"
    servers: ["10.168.1.12"]
Password:  
~~~~



~~~~ {.java name="code"}
command finished in 1778ms
    copying the cached version to /tmp/jae/dea/releases/20131014095846
  * executing "cp -RPp /tmp/jae/dea/shared/cached-copy /tmp/jae/dea/releases/20131014095846 && (echo 08a647586cf4285019ced6ebde34d427ff65214a > /tmp/jae/dea/releases/20131014095846/REVISION)"
    servers: ["10.168.1.12"]
    [10.168.1.12] executing command
    command finished in 88ms
  * 2013-10-14 17:58:46 executing `deploy:finalize_update'
  * executing "chmod -R -- g+w /tmp/jae/dea/releases/20131014095846 && rm -rf -- /tmp/jae/dea/releases/20131014095846/public/system && mkdir -p -- /tmp/jae/dea/releases/20131014095846/public/ && ln -s -- /tmp/jae/dea/shared/system /tmp/jae/dea/releases/20131014095846/public/system && rm -rf -- /tmp/jae/dea/releases/20131014095846/log && ln -s -- /tmp/jae/dea/shared/log /tmp/jae/dea/releases/20131014095846/log && rm -rf -- /tmp/jae/dea/releases/20131014095846/tmp/pids && mkdir -p -- /tmp/jae/dea/releases/20131014095846/tmp/ && ln -s -- /tmp/jae/dea/shared/pids /tmp/jae/dea/releases/20131014095846/tmp/pids"
    servers: ["10.168.1.12"]
    [10.168.1.12] executing command
    command finished in 21ms
  * 2013-10-14 17:58:46 executing `deploy:create_symlink'
  * executing "rm -f /tmp/jae/dea/current &&  ln -s /tmp/jae/dea/releases/20131014095846 /tmp/jae/dea/current"
    servers: ["10.168.1.12"]
    [10.168.1.12] executing command
    command finished in 8ms
    triggering after callbacks for `deploy:create_symlink'
  * 2013-10-14 17:58:46 executing `deploy:bundle_install'
Are you sure to bundle install (y): 
  * 2013-10-14 17:58:49 executing `deploy:link_config_file'
  * executing "cd /tmp/jae/dea/current && rm config/dea.yml && ln -s /tmp/jae/dea/config/dea.yml config/dea.yml"
    servers: ["10.168.1.12"]
    [10.168.1.12] executing command
    command finished in 9ms
 ** transaction: commit
  * 2013-10-14 17:58:49 executing `deploy:restart'
Are you sure to restart dea(y): y
  * 2013-10-14 17:58:51 executing `deploy:stop'
  * executing "sudo -p 'sudo password: ' kill -9 `cat /tmp/dea_ng.pid`"
    servers: ["10.168.1.12"]
    [10.168.1.12] executing command
    command finished in 14ms
  * 2013-10-14 17:58:53 executing `deploy:start'
  * executing "sudo -p 'sudo password: '             nohup /tmp/jae/dea/current/bin/dea /tmp/jae/dea/current/config/dea.yml >/dev/null < /dev/null 2>&1 &\\\n            sleep 3"
    servers: ["10.168.1.12"]
    [10.168.1.12] executing command
    command finished in 3009ms
~~~~





六、脚本示例
============


 
 



~~~~ {.java name="code"}
#require 'bundler/capistrano'           #添加之后部署时会调用bundle install， 如果不需要就可以注释掉  
require "capistrano/ext/multistage"     #多stage部署所需  
  
  
set :stages, ["development","part1", "part2","part3"]  #利用stage将dea分为三个部分来操作，避免全部dea同时重启造成的服务不可用
set :default_stage, "development"  #测试环境
  
  
set :application, "dea"   #应用名称 
set :scm,:git 
set :repository,  "https://code.jd.com/dea_v2"  
set :branch,"master"
set :git_enable_submodules,1   #有子模块，要加上这个配置
set :git_enable_recursion,1    #有多重子模块
set :keep_releases, 5          #只保留5个备份  
  
  
set :deploy_to, "/tmp/jae/#{application}"  #部署到远程机器的路径  
set :user, "admin"              #登录部署机器的用户名  
 
default_run_options[:pty] = true          #pty: 伪登录设备  
#default_run_options[:shell] = false      #Disable sh wrapping  
set :normalize_asset_timestamps, false
set :deploy_via, "remote_cache"           #本地cache，第一次全量，后面git pull就是增量了
  
set :default_environment,{                #环境变量，以key value的方式写上，因为non-login的关系，默认为会载入系统的环境变量
  "GIT_SSL_NO_VERIFY" => '1'
} 
set :use_sudo, false                          #执行的命令中含有sudo， 如果设为false， 用户所有操作都有权限  
set :runner, "admin"                          #以admin用户启动服务 
set :config_file, "dea.yml"        
  
namespace :deploy do  
    after "deploy:create_symlink" , "deploy:bundle_install"
    after "deploy:create_symlink","deploy:link_config_file"
    
    #由于每个服务器的配置文件不一样，dea没有跟rails一样，在一个config中写上不同环境的值。这里的解决办法是每个dea的配置文件只保存在本地， 
    #存放在#{deploy_to}/config/dea.yml，部署的时候用软链接
    desc "config file is unique on each server,we store it on deploy_to/config/xx.yml,use link to make it work"
    task :link_config_file,roles => :app do
          run "cd #{deploy_to}/current && rm config/#{config_file} && ln -s #{deploy_to}/config/#{config_file} config/#{config_file} "
    end  
    #不是每次部署都要bundle install，之所以不用capistrano/bundle，它会将gem安装在其它地方，破坏了原来工程的和谐
    desc "it's optional to bundle install everytime,unless modify the Gemfile etc."
    task :bundle_install, roles => :app do 
       answer = Capistrano::CLI.ui.ask("Are you sure to bundle install (y): ")
       if answer == "y"
          run "cd #{deploy_to}/current && bundle install" do |channel,stream,data|
            if data =~ /password/
              answer = Capistrano::CLI.ui.ask("Enter your password to install the bundled RubyGems to your system: ")
              channel.send_data(answer + "\n")
            else
               # allow the default callback to be processed
               Capistrano::Configuration.default_io_proc.call(channel, stream, data)
            end
          end 
       end
    end
    
    #
    desc "start the dea server,exclude dir server and warden"
    task :start,roles => :app do  
       sudo <<-EOC
            nohup #{deploy_to}/current/bin/dea #{deploy_to}/current/config/dea.yml >/dev/null < /dev/null 2>&1 &
            sleep 3
       EOC
    end  
  
    #
    desc "stop the dea server"
    task :stop,roles => :app do  
       sudo "kill -9 `cat /tmp/dea_ng.pid`"
    end  
  
    #
    desc "restart dea"
    task :restart ,roles => :app do  
       answer = Capistrano::CLI.ui.ask("Are you sure to restart dea(y): ")
       if answer=="y"
         deploy.stop
         sleep 2
         deploy.start
       end
    end  
      
   #up and donw dir server
   #up and donw warden
 
   desc "migrate do nothing here"
   task :migrate, roles => :app do 
   end 
end  
~~~~





如果想用
stage（即多个环境），可以在config文件夹下，再创建一个目录"deploy"，deploy下创建与stage同名的文件，如part1.rb，part2.rb。如dea，我的part1.rb其实只指定了一组服务器ip


 



~~~~ {.java name="code"}
role :app,"ip1","ip2","ip3",.......
~~~~


 \

 有两个地方要说明，也是我在编写脚本时碰到的问题：


 

​1. 服务器端的交互，capi提供的
sudo或其他方法，只是一次性交互，但如果在执行脚本过程中碰到需要交互的地方，如bundle
install
一半要你输入密码。capi客户端也会提示你输密码，但输入后按回车就“卡”住了，没反应，这就是因为non-interactive引起的，capi只管登录--\>执行脚本--\>等待回显，登出返回，而且每个脚本一个来回。如





~~~~ {.java name="code"}
run "cd /tmp"
run "bin/dea config/dea.yml"
~~~~


 \

这就两个来回了，第二句执行的时候就不是在/tmp目录下，而是在/home/admin目录下


 

这时可以用
channel搞定，我的理解是在同一个session当中，可以实现交互的效果。


 



~~~~ {.java name="code"}
  task :bundle_install, roles => :app do 
       answer = Capistrano::CLI.ui.ask("Are you sure to bundle install (y): ")
       if answer == "y"
          run "cd #{deploy_to}/current && bundle install" do |channel,stream,data|
            if data =~ /password/
              answer = Capistrano::CLI.ui.ask("Enter your password to install the bundled RubyGems to your system: ")
              channel.send_data(answer + "\n")
            else
               # allow the default callback to be processed
               Capistrano::Configuration.default_io_proc.call(channel, stream, data)
            end
          end 
       end
    end
~~~~


 \

 利用 run的 callback功能。


 

​2.
当服务端有程序需要后台运行，一般的做法是加&就可以了。但是capi不行。必须再加上nohup才行。但是nohup+&在事实证明中也是不靠谱的。再次因为“capi只管登录--\>执行脚本--\>等待回显，登出返回”
。它执行完 nohup bin/dea config/dea.yml &
后立刻返回，就是登出了，就断了，程序还没来得及提交给后台运行就挂了。所以发必须sleep
几秒，让程序提交后台运行后再登出。


 
 
 



~~~~ {.java name="code"}
run "(nohup #{deploy_to}/current/bin/dea #{deploy_to}/current/config/dea.yml  &) && sleep 3"
~~~~


 \

错误，因为capi会等待回显，傻傻地等，就是“卡”住了。所以一定要将nohup输出定向到某个地方，可以做到执行脚本的立刻返回。


 



~~~~ {.java name="code"}
run  "(nohup #{deploy_to}/current/bin/dea #{deploy_to}/current/config/dea.yml >/dev/null < /dev/null 2>&1 &) && sleep 3"
~~~~


 \

 这样才行。


 
 
 



~~~~ {.java name="code"}
sudo  "(nohup #{deploy_to}/current/bin/dea #{deploy_to}/current/config/dea.yml >/dev/null < /dev/null 2>&1 &) && sleep 3"
~~~~


 \

错误，语法上通不过。这个方法在服务器上是这么执行的，会提示语法错误，用分号之类的都不行。可能shell掌握不太好，没有搞定。


 



~~~~ {.java name="code"}
executing "sudo -p 'sudo password: ' (nohup /export/servers/jae/dea/current/bin/dea /export/servers/jae/dea/current/config/dea.yml >/dev/null < /dev/null 2>&1 &) && sleep 3"
~~~~


 \

 一个方法是写个shell文件，sudo
去执行shell文件，没有试过，但以下做法的效果是一样的：


 



~~~~ {.java name="code"}
#
    desc "start the dea server,exclude dir server and warden"
    task :start,roles => :app do  
       sudo <<-EOC
            nohup #{deploy_to}/current/bin/dea #{deploy_to}/current/config/dea.yml >/dev/null < /dev/null 2>&1 &
            sleep 3
       EOC
    end  
~~~~




 

七、结束语
==========


 
 
 

capistrano可以执行shell脚本，这就给了我们很多的自主空间，第三方扩展都是些包装好的东西，可以自己写shell，理论上什么都可以部署。


 
 
