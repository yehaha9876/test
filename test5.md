服务器端使用LESS
================

~~~~ {.plain code_snippet_id="93778" snippet_file_name="blog_20131203_1_7469649" name="code"}
安装python2.6或者更高版本
apt-get install g++ curl libssl-dev apache2-utils
apt-get install git
git clone git://github.com/joyent/node.git
cd node
git checkout v0.6.12
mkdir ~/local
./configure -prefix=$HOME/local/node
make
make install
echo 'export PATH=$HOME/local/node/bin/:$PATH' >> ~/.profile
echo 'export NODE_PATH=$HOME/local/node:$HOME/local/node/lib/node_modules' >>
~/.profile
source ~/.profile
~~~~
