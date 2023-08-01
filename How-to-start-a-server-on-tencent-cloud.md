启动一个服务器并不困难，不论是阿里，腾讯还是亚马逊，在降低门槛吸引客户上都下足了功夫。他们也做的确实不错。

这篇日志重点在于记录启动了服务器（使用腾讯云）之后遇到的一些问题。

启动服务器后，使用top命令可以看到占用服务器资源的进程。笔者使用的是`vscode`，经常看到一个名为`rg`的进程占用大量的资源。以至于服务器卡死。

最初以为是小马插入了什么病毒，后来发现是`vscode`设置所导致的。

在设置中搜索`search.followSymlinks`，将对应设置改为False即可。笔者找到的资料都是修改**远程**和**工作区**两项，但实际尝试时只找到了**用户**一项，所幸同样起效。 

另外有一个`node`进程同样占用了一些的资源，这是`Javascript/Typescript`的插件所引起的，禁用即可。不禁用也不会有太大影响。



国内服务器一个主要问题是无法跨越防火墙，如Github、Google这样的网站都访问不了。部分代码资源可以在国内找到镜像站，但终究不如直接下载方便。

为此，需要一个能直接访问外网的科学上网工具。

笔者所用的同样是在Github上找到的开源项目，和一些集成脚本。

```
https://github.com/the0demiurge/CharlesScripts/blob/master/charles/bin/ssr
```

由于此时尚不能合理访问Github，可以将这个脚本中的内容直接复制，保存在本地文件中（假定为ssr）

移动文件斌赋予执行权限

```
sudo mv ssr /usr/local/bin
sudo chmod +x /usr/local/bin/ssr
```

此时执行安装命令依然会报错，因为这个脚本安装的是另一个Github上的项目。建议手动将这个项目拷贝到服务器上。

该仓库地址

```
https://github.com/shadowsocksrr/shadowsocksr
```

复制到本地的

```
/root/.local/share/
```

随后使用

```
sudo config
```

即可配置远程端口及连接参数。请注意，这只是一个科学上网工具的客户端，你需要有一个实际的服务器才能达成科学上网的效果。

完成这一步后服务会自己启动并尝试连接。在实际使用时，你需要下载`python2`和`proxychains4`

比如

```
sudo apt-get install python2
sudo ln -s /usr/bin/python2 /usr/bin/python

sudo apt-get install proxychains4
```

编辑配置文件，并将端口配置为在 `ssr config` 这一步配置的端口

```
sudo vim /etc/proxychains4.conf
```

随后即可使用`proxychains`作为代理了。

```
curl ifconfig.me
proxychains curl ifconfig.me
```

这两条命令输出的应该是不一样的IP地址。