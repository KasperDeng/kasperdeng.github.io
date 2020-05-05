---
layout: post
title:  Golang Development Environment
description: "Golang Development Environment. First published on 2015-04-15."
tags: [Golang IDE]
image:
  background: triangular.png
---

# Installation
* Download go 
  - [official](https://golang.org/dl/) & [Getting Started](https://golang.org/doc/install)
  - [china official](https://golang.google.cn/dl)
* Go Complier: 
  - offcial: gc
  - GNU GC: gccgo
* Go supports instruction sets
  1. amd64 (a.k.a. `x86-64`); 6g,6l(Plan9 gc),6c,6a // support x84-64, naming as amd64
      is to amd's contribution of inventing 64bit instruction set
  2. 386 (a.k.a. `x86` or `x86-32`); 8g,8l,8c(Plan9 gc),8a
  3. arm (a.k.a. ARM); 5g,5l,5c(Plan9 gc),5a. Now supports 64-bit ARM architecture on FreeBSD 12.0 or later (the freebsd/arm64 port).
  4. Experimental support for 64-bit RISC-V on Linux (GOOS=linux, GOARCH=riscv64)
* Set Env Variables
  - GOBIN: C:\Go\bin (optional, if no GOBIN, GOPATH will be used)
  - GOARCH: x86-32bit: 386, x86-64bit: amd64, ARM: amr (android)
  - GOOS: windows
  - GOROOT: C:\Go
  - GOPATH: C:\GOPATH
    + To specify directories outside of $GOROOT that contain the source for Go projects and their binaries.
  - Add to PATH: %GOBIN%
* Multi-Version GO Env
  - [GVM](https://github.com/moovweb/gvm)
    + An interface to manage Go versions, like NVM(Nodejs Version Manager), RVM (Ruby Version Manager).
* Reference in astaxie/build-web-application-with-golang
  - [En](https://github.com/astaxie/build-web-application-with-golang/blob/master/en/01.1.md)
  - [Zh](https://github.com/astaxie/build-web-application-with-golang/blob/master/zh/01.1.md)

# Tools
* [Go Tools](http://dominik.honnef.co/posts/2014/12/an_incomplete_list_of_go_tools/)
* [dev-utilies](http://go-lang.cat-v.org/dev-utils)
* [gotags](https://github.com/jstemmer/gotags)
* [ctags-go](https://github.com/lyosha/ctags-go)
* [vim-godef](https://github.com/dgryski/vim-godef)

# IDE

# GOPROXY
* Set to [Goproxy China](https://github.com/goproxy/goproxy.cn)
* default value: `GOPROXY=https://proxy.golang.org,direct`

```bash
$ go env -w GO111MODULE=on
$ go env -w GOPROXY=https://goproxy.cn,direct
```

done.

### macOS or Linux

Open your terminal and execute

```bash
$ export GO111MODULE=on
$ export GOPROXY=https://goproxy.cn
```

or

```bash
$ echo "export GO111MODULE=on" >> ~/.profile
$ echo "export GOPROXY=https://goproxy.cn" >> ~/.profile
$ source ~/.profile
```

done.

### Windows

Open your PowerShell and execute

```poweshell
C:\> $env:GO111MODULE = "on"
C:\> $env:GOPROXY = "https://goproxy.cn"
```

# [gopls](https://github.com/golang/tools/tree/master/gopls)
* It is widely used in vscode, vim/neovim for golang IDE.
  - gopls (pronounced: "go please") is the official language server for the Go language.
  - It is currently in alpha, so it is not stable. Aiming stable in the first half of 2020.
* Installation
  - By vscode or vim-go
  - Manually
    + Set GOPROXY in above steps.
    + `go get golang.org/x/tools/gopls@latest`, note, don't use `-u` option to keep it compatible. 
* Problems of gopls
  - slow in searching implemenations of interface, callers and callees.
  - CPU and memory consuming (2G ~ 4G).

## Vscode/insiders
* Install Go plugin with tools (gopls, godef, gorename...)
  - command palette: "Go:install/update tools"
* Setup Configuration

```json
"go.autocompleteUnimportedPackages": true,
"go.inferGopath": true,
"go.gopath": "C:\\GOPATH",
"go.goroot": "C:\\Go",
"go.testFlags": ["-v"],
"go.testOnSave": true,
"go.toolsGopath": "C:\\Go",
"go.formatTool": "goimports",
"go.useLanguageServer": true
```

## Vim

* [vim-go](https://github.com/fatih/vim-go)
  - PluginInstall
    - Plug: Plug 'fatih/vim-go', { 'do': ':GoUpdateBinaries' }
    - Vundle: Plugin 'fatih/vim-go'
  - GoInstallBinaries/GoUpdateBinaries
  - ~~Install ctags: e.g. In Ubuntu, sudo apt-get install ctags and in /usr/bin/ctags~~
  - ~~Config the ctags path for tagbar: let g:tagbar_ctags_bin = '/usr/bin/ctags' (Ubuntu)~~
* [Tutorial](https://github.com/fatih/vim-go/wiki/Tutorial)
  - GoGuruScope
* Configurations

```
let g:go_auto_type_info = 1
let g:go_auto_sameids = 1
"let g:go_debug = ['lsp']
"let g:go_gopls_enabled = 0
"let g:go_def_mode = 'guru'
"let g:go_referrers_mode = 'guru'
"let g:go_info_mode = 'guru' "default is gopls
```

* Useful Commands
  - gd/CTRL-]: GoDef
  - CTRL-o
  - :GoReferrers
  - :GoDescribe
  - :GoImplementes
  - :GoChannelPeers
  - :GoCallees
  - :GoCallers
  - :GoGuruScope
  - :GoInfo
  - :GoSameIds/:GoSameIdsClear

* ~~Add key mapping to .vimrc, e.g. `au FileType go nmap gdf <plug>(go-def)`~~
* Vim build-in `<C-o>` and `<C-i>` help back/forward of code definition naviation.
* `<C-x><C-o>` auto go code complete by omnicomplete system, `<C-n>` to select.
  - Add config to vimrc to support invoke gocode for code completion
      ```
       " Golang
      au BufNewFile,BufRead *.go setf go
      autocmd FileType go setlocal omnifunc=go#complete#Complete
      ```
   - autocompletion is based on the `gocode`, when golang upgrade, please upgrade the gocode by `go get -u github.com/nsf/gocode`
* [YouCompleteMe](https://github.com/Valloric/YouCompleteMe) (Code-completion for Vim, but no official support on Windows)
* References
   - http://tonybai.com/2014/11/07/golang-development-environment-for-vim/
   - http://obahua.com/setup-vim-for-go-development/
   - [Golang - All Known Implementations of An Interface In VIM](https://coderwall.com/p/-t_5zw/golang-all-known-implementations-of-an-interface-in-vim)
   - [godef issue in go v1.4]( http://stackoverflow.com/questions/27860178/how-to-rebuild-godef-to-work-with-go-1-4)
   - Need to update the [godef](http://code.google.com/p/rog-go/source/browse/exp/cmd/godef/godef.go) and build it by self 

```shell
" Golang
au BufNewFile,BufRead *.go setf go
autocmd FileType go setlocal omnifunc=go#complete#Complete
```

  - If no config, use the gvim-ex which support it by default (unknown why)
  - autocompletion is dependent on the `gocode`, when golang upgrade, please upgrade the gocode by `go get -u github.com/nsf/gocode`

* [YouCompleteMe](https://github.com/Valloric/YouCompleteMe) (Code-completion for Vim, but no official support on Windows)
* Supports in cygwin vim, using imported env variable from Windows. Currently found godef has limitation in cygwin to browse the code definition in $GOPATH/src(config problem?)

* [godef issue in go v1.4](http://stackoverflow.com/questions/27860178/how-to-rebuild-godef-to-work-with-go-1-4)
  - Need to update the [godef](http://code.google.com/p/rog-go/source/browse/exp/cmd/godef/godef.go) and build it by self

## [Gogland](https://www.jetbrains.com/go/)
* JetBrains GO IDE
  - Disable parameter name hint (Editor -> General -> Appearance)

## sublime

* Install package control
  - "ctrl + \`" or Menu: View -> Show console: input

```bash
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
```

 - Restart sublime

* Install GoSublime
  - ctrl+shift+p -> input install<cr> -> input GoSublime <cr>
  - preferences —> package setting —> gosublime —> setting default, update **env**
  - Restart sublime

```json
{
    "env": {
            "path":"C:\\Go\\bin",
            "GOROOT":"C:\\Go",
            "GOPATH":"C:\\GOPATH",
            "GOARCH":"amd64",
            "GOOS":"windows",
            "PATH":"%GOBIN%;%PATH%"
    },
    "comp_lint_enabled": true,
    "comp_lint_commands": [
        {"cmd": ["go", "install"]}
    ],
    "on_save": [
        {"cmd":"gs_comp_lint"}
    ]
}
```

* `ctrl+p -> gosublime`: to find related gosublime functions and key binding
* keybinds: ctrl+.,ctrl+g: definition
* Build
  - ctrl+b (cli console in sublime), then run the go commands
* **gosublime drawback**: read code with goto definition but no easy way to go back.

* Ref: 
  - http://www.v-lover.com/2014/12/24/go-built-environment/
  - [Godef for Sublime](http://blog.buaa.us/godef-plugin-for-sublime-released/) (currently not support windows)

## Atom
* Lots of golang package support
  - go-plus (but currently failed to install in my workspace)

## Notepad++
* Gonpp

## intellij
* Ref: http://www.cnblogs.com/clivelee/p/3870186.html
* [golang plugin](http://github.com/go-lang-plugin-org/go-lang-idea-plugin)
* Setup Go project SDK

## Gogland
> [JetBrains](https://www.jetbrains.com/go/), commercial IDE, extends the IntelliJ platform

* Not formal release yet. Can download an early build for trial

## Eclipse
* Goclipse

## Wide
* [Coding with Go on the Wide way](https://wide.b3log.org)

## [Zeus IDE](http://www.zeusedit.com/go.html)
* TBD

## [LiteIDE](https://github.com/visualfc/liteide)
> LiteIDE is a simple, open source, cross-platform Go IDE. Based on Qt5/Qt4

* easy installation and usage
* based on Qt, gui is not fashion style
