---
layout: post
title:  go commands
description: "go commands"
tags: [golang]
image:
  background: triangular.png
---

# go build #
- compile the package named by the import paths and thier dependencies
- go build package/*.go
   - if build *.go, a virtual package command-line-arguments is created internally
      + $WORK/command-line-arguments/_obj/: stores the obj files

      + $WORK/command-line-arguments/_obj/exe/: stores the executable after link
- `-v - work`: print the name of the temporary work directory and do not delete it when exiting
- `-n`: print the commands but do not run them
- `-x`: as the `-n` flag but run them

# go install #
- compile and install the package and its dependent package. The result is archive/executable and installed to $GOBIN dir

- If $GOBIN not set, error promption: `go install: no install location for directory`
- `-o` flag currently is not acceptable for go install
- go install only accepts \*.go of cmd/executable src code, not *.go of lib src code. To install libs, need run `go install package` 

# go get #
- download/update specific package or their dependencies
- `-d`: only download the source code, no package installation
- `-u`: update the named packages and their depenencies
- `-fix`: for backword compatible

# go list #
> list package information

- go list -json package: print the package strcuture in json format
- text/template syntax
   * go list -f \{\{.ImportPath\}\} package
   * go list -f \{\{.S.F\}\} package

{% highlight bash %}
$ go list -json net/http
$ go list -json github.com/kasperdeng/grpcPt/client
{% endhighlight %}

# go test #
> test *_test.go

- `go help testflag`
- `-i`: install the test dependencies, but not compile and run it
- `-c`: generate test executables, but not run it
- `-bench regexp` 执行相应的benchmarks，例如 -bench=.
- `-cover` 开启测试覆盖率
- `-run regexp` 只运行regexp匹配的函数，例如 -run=Array 执行包含有Array开头的函数

# godoc #

{% highlight bash %}
$ go get golang.org/x/tools/cmd/godoc
$ godoc -http=:8080 &
# godoc package
$ godoc net/http
# godoc func
$ godoc fmt Printf
{% endhighlight %}

# Reference #
* [Go Command Turorial](https://github.com/hyper-carrot/go_command_tutorial/blob/master/catalog.md)
* [极客学院 - GO 命令教程](http://wiki.jikexueyuan.com/project/go-command-tutorial/)
* [astaxie - GO命令](https://astaxie.gitbooks.io/build-web-application-with-golang/zh/01.3.html)
