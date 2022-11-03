# 宝塔面板降级旧版本教程

### 1. 官方版本（7.7.0）
     http://download.bt.cn/install/update/LinuxPanel-7.7.0.zip

### 2. 安装方法

  1、先使用官方命令正常进行宝塔安装

```
Ubuntu/Deepin安装命令：
wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && sudo bash install.sh 

Debian安装命令： 
wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && bash install.sh
```
  2、根据官方手动升级/降级方法进行覆盖安装

```
cd /root  #切换到root目录 
wget -c https://github.com/ipking/BaoTa/archive/refs/tags/LinuxPanel-7.7.0.zip  #下载7.7.0版本升级包
unzip LinuxPanel-7.7.0.zip #解压文件 
cd LinuxPanel-7.7.0/panel  #切换到升级包目录 
bash update.sh  #执行升级脚本 
cd /root  #切换到root目录 
rm -f LinuxPanel-7.7.0.zip  #删除升级包
rm -rf /root/LinuxPanel-7.7.0  #删除升级包
```

3、关闭账号绑定
```
mv /www/server/panel/data/bind.pl /www/server/panel/data/bind.pl.backup
```

### 3. 修复bug说明 (在原版基础上)


```
#修复module 'public' has no attribute 'is_error_path' 错误
#吧下面的代码 插入到 文件 /www/server/panel/class/publice.py 底部 
def is_error_path():
    if os.path.exists("/www/server/panel/data/error_pl.pl"):
        stop_status_mvore()
        return True
    return False
```

```
#修复'config' object has no attribute 'get_not_auth_status'' 错误
#吧下面的代码 插入到 文件 /www/server/panel/class/config.py 底部 
    def get_not_auth_status(self):
        '''
            @name 获取未认证时的响应状态
            @author hwliang<2021-12-16>
            @return int
        '''
        try:
            status_code = int(public.read_config('abort'))
            return status_code
        except:
            return 0
```

