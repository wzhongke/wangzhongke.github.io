---
title: sublime 配置
---

## 配置markdown
1. 安装package controll: 快捷键`ctrl+\``打开Sublime控制台，输入下面代码：
```python
import urllib.request,os,hashlib; h = 'eb2297e1a458f27d836c04bb0cbaf282' + 'd0e7a3098092775ccb37ca9d6b2e4b7d'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```
2. 安装markdownediting: `ctrl+shift+p` 进入sublime命令面板，打开`install package`，输入`markdownediting`，安装后重启 sublime
3. 安装`OmniMarkupPreviewer`，同方法2