## Install vim8
    curl -L https://copr.fedorainfracloud.org/coprs/mcepl/vim8/repo/epel-7/mcepl-vim8-epel-7.repo -o /etc/yum.repos.d/mcepl-vim8-epel-7.repo
    yum remove vim-minimal //maybe needed
    yum update vim

# Lastest Install vim 8 on Devserver
git clone https://github.com/vim/vim.git vim

sudo yum install cscope ncurses ncurses-devel ncurses-libs ncurses-base lua lua-devel python-libs ruby ruby-devel

cd vim
make distclean  # if you build vim before
# the python3 vim installation is not working
./configure --enable-rubyinterp=yes --with-features=huge --enable-luainterp --enable-pythoninterp=yes
make
sudo yum remove 'vim-common.x86_64' # remove vim7 component
sudo make install
rm /usr/bin/vim # remove old vim


## cscope
CSCOPE_DB=any_cscope_db
export CSCOPE_DB
then in vim, you can do "cscope add $CSCOPE_DB", and then "cscope show" to
veiry.
