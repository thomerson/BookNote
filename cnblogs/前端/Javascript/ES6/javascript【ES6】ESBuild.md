## ESBuild

ESbuild 是一个类似```webpack```构建工具。它的构建速度是 webpack 的几十倍

### 速度快的原因

* js是单线程串行，esbuild是**新开一个进程**，然后多线程并行，充分发挥多核优势

* **go**是纯机器码(Go 语言编写的程序比 JavaScript 少了一个动态解释的过程)，肯定要比JIT快

* 不使用 AST，优化了构建流程


### 其他工具

* webpack

* Vite 底层也是esbuild



[https://esbuild.docschina.org/](https://esbuild.docschina.org/)

<!-- TODO -->