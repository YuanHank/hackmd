# Python
## matplotlib
&emsp;在使用fill_between的時候，如果兩條線段的x,y數量不一樣，可以使用內插的方式使得兩縣段一樣，配合np.interp內插，程式碼如下
>線段一:x1,y1
>線段二:x2,y2
>xfill = np.sort(np.concatenate([x1,x2])) ->將x1和x2的x點總和，並且由小排到大(後續內插用這個list內的每個點作為欲內插的x值)
>y1fill = np.interp(xfill,x1,y1) ->利用線段一的x1和y1，內插出符合xfill內每個x值相對應的y值並命名為y1fill
>y2fill = np.interp(xfill,x2,y1)>利用線段二的x2和y2，內插出符合xfill內每個x值相對應的y值並命名為y2fill
>plt.fill_between(xfill,y1fill,y2fill,interpolate=True) ->填充已經將數量化成一樣的xfill,y1fill,y2fill

**&emsp;在使用np.interp的時候必須要由小排到大(只有要內插的線段由小排到大，要插的值不用)，因此如果x值為由大到小，則需要使用[x值][::-1]使得排序顛倒，同理y值也要同樣顛倒才能得到相對應的值[y值][::-1]**

## python 
### copy
#### 賦值/淺複製/深複製

* 賦值：單純利用等號 a = b，將a定義為b的別名，導向相同地址
* 淺複製：利用a = b.copy()，開設新的地址，改變不可變型時，互不影響，改變可變型時仍可影響
* 深複製：利用a=b.deepcopy()，開設新的地址，改變可變型/不可變型時，皆互不影響

**可變型：列表，字典 /不可變型：數字，字串，元組**
範例：
```
a = [1, [2, 3]]
a_equal = a
a_shallowcopy = copy.copy(a)
a_deepcopy = copy.deepcopy(a)
a[0] = 4    # 改變a中的第一個元素，即改變a中的數字
print'a:               ', a,       'id(a):             ', id(a)
print'a_equal:         ', a_equal, 'id(a_equal):       ', id(a_equal)
print'a_shallowcopy:   ', a_shallowcopy, 'id(a_shallowcopy): ', id(a_shallowcopy)
print'a_deepcopy:      ', a_deepcopy, 'id(a_deepcopy):    ', id(a_deepcopy)
#輸出：
a: [4, [2, 3]] id(a): 4578314360
a_equal: [4, [2, 3]] id(a_equal): 4578314360
a_shallowcopy: [1, [2, 3]] id(a_shallowcopy): 4562436248
a_deepcopy: [1, [2, 3]] id(a_deepcopy): 4562362096
#（因為第一個位置為數字，因此copy()/deepcopy()並無差異

a = [1, [2, 3]]
a_equal = a
a_shallowcopy = copy.copy(a)
a_deepcopy = copy.deepcopy(a)
a[0] = 4    # 改變a中的第一個元素，即改變a中的數字
a[1][1] = 5     # 改變a當中第二個元素中的元素，即 改變a當中的list
print'a:               ', a,       'id(a):             ', id(a)
print'a_equal:         ', a_equal, 'id(a_equal):       ', id(a_equal)
print'a_shallowcopy:   ', a_shallowcopy, 'id(a_shallowcopy): ', id(a_shallowcopy)
print'a_deepcopy:      ', a_deepcopy, 'id(a_deepcopy):    ', id(a_deepcopy)
#輸出：
a: [4, [2, 5]] id(a): 4454672504
a_equal: [4, [2, 5]] id(a_equal): 4454672504
a_shallowcopy: [1, [2, 5]] id(a_shallowcopy): 4438794392
a_deepcopy: [1, [2, 3]] id(a_deepcopy): 4438720240
#(因為第二個改變變量為list，為可變型，因此copy()/deepcopy()產生差異)
```

### iter,next

```
In python3 shell:
>>> mylist = [2,4,6,8,10]
>>> mylist_iterator = iter(mylist) # 這個iter()函數會調用python預設的mylist.__iter__函數，並回傳一個iterator物件，然後我們用mylist_iterator這個變數來儲存這個iterator物件
>>> # 開始迭代
>>> i = next(mylist_iterator) # i = 2 , next()函數會調用python預設的mylist_iterator.__next__函數
>>> print(i)
2
>>> i = next(mylist_iterator) # i = 4
>>> print(i)
4
>>> i = next(mylist_iterator) # i = 6
>>> print(i)
6
>>> i = next(mylist_iterator) # i = 8
>>> print(i)
8
>>> i = next(mylist_iterator) # i = 10
>>> print(i)
10
>>> i = next(mylist_iterator) 
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
>>> # 當最後一個元素遍歷完，再呼叫mylist_iterator.__next__就會跑出一個 StopIteration 的例外，一般for in在呼叫__next__的過程中遇到了 StopIteration 就會自動停止 for loop的執行
```
## pyenv
pyenv 是用來執行不同python版本以及管理python版本所使用，安裝步驟如下
### windows OS
```
//以下則一安裝
//powershell執行:
pip install pyenv-win --target $HOME\\.pyenv
//cmd執行:
pip install pyenv-win --target %USERPROFILE%\.pyenv
```
```
//設定環境變數
//powershell 執行:
[System.Environment]::SetEnvironmentVariable('PYENV',$env:USERPROFILE + "\.pyenv\pyenv-win\","User")
[System.Environment]::SetEnvironmentVariable('PYENV_HOME',$env:USERPROFILE + "\.pyenv\pyenv-win\","User")
[System.Environment]::SetEnvironmentVariable('path', $env:USERPROFILE + "\.pyenv\pyenv-win\bin;" + $env:USERPROFILE + "\.pyenv\pyenv-win\shims;" + [System.Environment]::GetEnvironmentVariable('path', "User"),"User")
```
### Linux OS
```
$ git clone https://github.com/yyuu/pyenv.git ~/.pyenv
$ git clone https://github.com/yyuu/pyenv-virtualenv.git ~/.pyenv/plugins/pyenv-virtualenv
$ sudo pip install virtualenv
// In ubuntunn:
$ sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm
```
```
//加入~/.bashrc:
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
```
### pyenv 指令使用
```
//pyen指令使用
//查看所有指令:
pyenv
--------------------------------------------
//確認pyenv版本:
pyenv --version
--------------------------------------------
//確認python版本:
pyenv version
--------------------------------------------
//安裝特定版本python:
pyenv install 3.10.0b3
--------------------------------------------
//設定特定專案使用特定版本 python:
pyenv local 3.10.0b3
--------------------------------------------
//設定全域環境所使用特定版本 python:
pyenv global 3.9.6
--------------------------------------------
//更新python 環境資訊(如果有使用 pip 安裝或解安裝 library 以及異動特定 version Python 的資料夾內容，必須使用 rehash 通知 pyenv 重新對應相關的使用連結。):
pyenv rehash
```
<a href = 'https://sdwh.dev/posts/2021/08/Python-Pyenv/'> reference (for Window OS)</a><br>
<a href = 'https://blog.codylab.com/python-pyenv-management/'>reference(for Linux OS)</a>

## virtual environment(虛擬環境架設)
### venv
使用此方法，會在使用專案資料夾產生一個新的環境資料夾，而非架設於系統。
```
//建立虛擬環境:
python –m venv (name)
--------------------------------------------
//啟動虛擬環境:
Windows : name\Scripts\activate 
(到activate.bat的路徑下，使用系統管理員啟動activate)
macOS/linux:source name\bin\activates
--------------------------------------------
//退出虛擬環境:
deactivate
--------------------------------------------
*註解1.從C槽連到D槽的cmd方式: cd /d d:\位置
*註解2.從power shell進入前，要先以系統管理員啟動powershell，然後輸入:Set-ExecutionPolicy RemoteSigned ，後輸入Y作為確認，最後回到環境變數的Script資料夾內，啟動activate(不需要用bat檔案)
```

### virtualenvwrapper
此種方式會集中儲存於初始安裝時設置的位置，而非隨著專案資料夾。
#### Windows OS
```
//利用pip3 安裝virtualenvwrapper-win:
pip3 install virtualenvwrapper-win
--------------------------------------------
//創建虛擬環境:
mkvirtualenv name
--------------------------------------------
//使用虛擬環境(指令):
>退出當前的虛擬環境:deactivate
>列出可使用的虛擬環境:workon
>使用虛擬環境:workon name
>刪除指定的虛擬環境:rmvirtualenv
--------------------------------------------
*註解1.Don’t know why but have to use command in cmd not powershell,otherwise it wont works

```
#### Linux OS
```
//Step1 使用sudo(superuser+pip3)安裝:
sudo pip3 install virtualenvwrapper
--------------------------------------------
將下列程式碼添加到.bashrc內:
//Step2-1: 
vi ~/.baschrc (開啟vim編輯器編輯baschrc，利用esc +i進入插入模式
//Step2-2 插入下列程式碼:
export WORKON_HOME=$HOME/.virtualenvs 
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3 
export VIRTUALENVWRAPPER_VIRTUALENV_ARGS=' -p /usr/bin/python3 ‘ 
export PROJECT_HOME=$HOME/Devel source /usr/local/bin/virtualenvwrapper.sh 
//Step2-3:
完成插入後點集esc回到一般模式，並按shift + :輸入指令wq以儲存並退出
--------------------------------------------
//Step3重新加載啟動文件:
source ~/.bashrc                                   

*註解(後續創建同windows的創建部分(mkvirtualenv))
```

#### requirtment
```
//Freeze requirement:
pip freeze > requirtment.txt
--------------------------------------------
//安裝requirtment: 
pip install –r requirement.txt

```
