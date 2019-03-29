## VIM 插件

### 1. Python自动补全

* 1、下载pydiction文件

	`wget https://github.com/rkulla/pydiction/archive/master.zip`

* 2、解压zip文件，解压后出现“pydiction-master”目录

	`unzip master`

* 3、创建目录

	`mkdir -p ~/.vim/tools/pydiction`

* 4、将“pydiction-master”目录中的文件复制到“~/.vim”目录下

	`cp -r pydiction-master/after ~/.vim`
	`cp pydiction-master/complete-dict ~/.vim/tools/pydiction`

* 5、配置`~/.vimrc`文件
	```
	"配置python语法补全
	filetype plugin on
	let g:pydiction_location = '~/.vim/tools/pydiction/complete-dict'
	```