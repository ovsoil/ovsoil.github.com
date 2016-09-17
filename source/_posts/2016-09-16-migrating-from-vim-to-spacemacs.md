layout: post
title: migrating from vim to spacemacs
date: 2016-09-16 14:52:38
categories:
tags:
    - emacs
    - spacemacs
---

# 从 Vim 迁移到 spacemacs
转：[https://www.oschina.net/translate/migrating-from-vim](https://www.oschina.net/translate/migrating-from-vim)

### 哲学

很多 vim 都有的误解是，Spacemacs 是 vim 的 Emacs 克隆。Spacemacs 没有完全模仿 vim 的行为，它只有在编辑的时候才这样。你不能指望每个 vim 指令都可用，尽管很多都是可用的。你不能用 Vimscript 配置 Spacemacs，反正没人喜欢 Vimscript。重要的是，Spacemacs 旨在使用 vim 高级编辑模式以及 Emacs 更好的配置语言来改善 vim 和 Emacs 两者。

<!--more-->
## 基本介绍

### 术语

Spacemacs 使用与 vim 不同的术语，可能会使新用户感到困惑。本节试图厘清这些困惑。

**模式和状态**

在 vim 中，你具有多种编辑模式如插入模式和可视模式来处理文本。在 Emacs 中，我们使用的是[状态](https://github.com/syl20bnr/spacemacs/blob/master/doc/DOCUMENTATION.org#states)。它们等同于 vim 的模式。例如 evil-insert-state 与 vim中的insert-mode相同。

Emacs 中的 minor-mode 就像是激活一项功能。例如：aggressive-indent-mode 就是一个 minor-mode，它可以在你输入的同时自动缩进代码。需要知道的是，在一个缓冲区中可以激活多个 minor-modes。许多 Emacs 包通过提供一个 minor-mode 进行工作。Major-mode 确定 Emacs 在当前缓冲区的行为。通常每个文件类型对应一个 major-mode。一个 major-mode 的例子如 python-mode，它对 python 文件提供针对 python 的设置。每个缓冲区只有一个 major-mode。

#### 层

Spacemacs 具有层的概念。层类似于 vim 中的插件。它们提供可以在 Spacemacs 中使用的新功能。但是层通常是由数个相互整合良好的包组成。例如，python 层包括自动补齐支持，文档查找，测试和其它由不同的包提供的功能。这使你不需要考虑安装什么包，只需要考虑你想要什么功能。关于层的更多内容可以参考[自定义](https://github.com/syl20bnr/spacemacs/blob/master/doc/VIMUSERS.org#customization)章节和[官方文档](https://github.com/syl20bnr/spacemacs/blob/master/doc/DOCUMENTATION.org#configuration-layers)。同时还有一个关于编写层的更深入的[指南](https://github.com/syl20bnr/spacemacs/blob/master/doc/LAYERS.org)。

#### 微状态  

Spacemacs 提供一个称为微状态的特殊功能。微状态允许相似的命令连续执行，不需要重复按键。微状态通常由使用如下模式的键位绑定触发：<Leader> <group>这里的 group 是微状态从属的目录。当处于一个微状态时，你将会在窗口的底部看到说明文档。要退出微状态，按 q 键。

![img/spacemacs-scale-micro-state.png](http://static.oschina.net/uploads/img/201511/09112332_PKwl.png)

### 键位绑定约定

Spacemacs 使用 SPC 作为它的 <Leader> 键。本文档也使用 SPC 作为 <Leader> 键。所有的键位绑定都采用助记方式，并由 <Leader> 键组织。例如，语言指定命令的键位绑定通常使用 SPC m前缀。Spacemacs 使用的约定如下 [表](https://github.com/syl20bnr/spacemacs/blob/master/doc/CONVENTIONS.org). 注意所有的键位绑定都可以更改。

Spacemacs 在一个延迟之后使用 [which-key](https://github.com/justbur/emacs-which-key) 来显示可用的键位绑定：

![img/which-key.png](http://static.oschina.net/uploads/img/201511/09112333_yhDV.png)

### 运行命令

可以使用 SPC：运行 Emacs 命令。这将会弹出一个使用 [Helm](https://github.com/emacs-helm/helm) 的缓冲区。这个缓冲区中可以运行任意的 Emacs 命令。你同样可以使用：运行许多外部命令，就跟 vim 中一样。

注意：你可以使用: 运行 Emacs 交互命令，但是不能使用 SPC : 运行外部命令。

### 缓冲区和窗口管理  

#### 缓冲区

在 Emacs 和 vim 中缓冲区本质上是相同的。缓冲区的快捷键都具有 SPC b 前缀。
<table border="1">
<tbody>
<tr>
<th> 快捷键</th>
<th> 功能</th>
</tr>
<tr>
<td>    SPC b b <buffer-name></td>
<td>   创建一个名为<buffer-name>的缓冲区。</td>
</tr>
<tr>
<td>    SPC b b</td>
<td>   通过打开缓冲区和最近的文件搜索。</td>
</tr>
<tr>
<td>    SPC b n 或 :bnext</td>
<td>   切换到下一个缓冲区。(参见[特殊缓冲区](https://github.com/syl20bnr/spacemacs/blob/master/doc/*Special%20buffers))</td>
</tr>
<tr>
<td>    SPC b p 或 :bprevious</td>
<td>   切换到前一个缓冲区。 (参见[特殊缓冲区](https://github.com/syl20bnr/spacemacs/blob/master/doc/*Special%20buffers))</td>
</tr>
<tr>
<td>    SPC b d 或 :bdelete</td>
<td>   结束当前缓冲区。</td>
</tr>
<tr>
<td>    SPC b k</td>
<td>   查找并结束一个缓冲区。</td>
</tr>
<tr>
<td>    SPC b K</td>
<td>   结束除当前缓冲区的所有其他缓冲区。</td>
</tr>
<tr>
<td>    SPC b .</td>
<td>   缓冲区微状态。</td>
</tr>
</tbody>
</table>

特殊缓冲区

Emacs 默认会创建大量缓冲区，这些缓冲区很多人从来都不会使用到，就像 *Messages*。Spacemacs 会在使用这些快捷键时自动忽略这些缓冲区。可以在[这里](https://github.com/syl20bnr/spacemacs/blob/master/doc/DOCUMENTATION.org#special-buffers)找到更多相关信息。

#### 窗口

窗口就像 vim 中的分割。它们在一次编辑多个文件时相当有用。所有的快捷键都有 SPC w 前缀。

<table border="1">

<tbody>

<tr>

<th> 快捷键</th>

<th> 功能</th>

</tr>

<tr>

<td>    SPC w v 或 :vsplit</td>

<td>    在右侧打开一个垂直分割。</td>

</tr>

<tr>

<td>    SPC w s 或 :split</td>

<td>    在下部打开一个水平分割。</td>

</tr>

<tr>

<td>    SPC w h/j/k/l</td>

<td>    在窗口间导航。</td>

</tr>

<tr>

<td>    SPC w H/J/K/L</td>

<td>    移动当前窗口。</td>

</tr>

<tr>

<td>    SPC w .</td>

<td>    窗口微状态。</td>

</tr>

</tbody>

</table>

### 文件

Spacemacs 中所有文件命令都有 SPC f 前缀。

<table border="1">

<tbody>

<tr>

<th> 快捷键</th>

<th> 功能</th>

</tr>

<tr>

<td>    SPC f f</td>

<td>    打开一个缓冲区搜索当前目录中的文件。</td>

</tr>

<tr>

<td>    SPC f r</td>

<td>    打开一个缓冲区在最近打开的文件中搜索。</td>

</tr>

<tr>

<td>    SPC f s 或 :w</td>

<td>    保存当前文件。</td>

</tr>

<tr>

<td>    :x</td>

<td>    保存当前文件并退出。</td>

</tr>

<tr>

<td>    :e <file></td>

<td>    打开<file></td>

</tr>

</tbody>

</table>

### 帮助系统

Emacs 具有一个可扩展的帮助系统。所有的快捷键都有SPC h d 前缀，以允许便捷地访问帮助系统。最重要的快捷键是 SPC h d f, SPC h d k, 和 SPC h d v。同样还有 SPC <f1> 允许用户搜索文档。

<table border="1">

<tbody>

<tr>

<th> 快捷键</th>

<th> 功能</th>

</tr>

<tr>

<td>    SPC h d f</td>

<td>    对一个功能提示并显示其文档。</td>

</tr>

<tr>

<td>    SPC h d k</td>

<td>    对一个快捷键提示并显示其绑定的内容。</td>

</tr>

<tr>

<td>    SPC h d v</td>

<td>    对一个变量提示并显示其文档和当前值。</td>

</tr>

<tr>

<td>    SPC <f1></td>

<td>    搜索一个命令，功能，变量或接口，并显示其文档</td>

</tr>

</tbody>

</table>

不论何时，你遇到怪异的行为或想知道是什么东西做的，这些功能是你应该首先查阅的。

### 探索

有几种方式可以探索 Spacemacs 的功能。一个是阅读 Github 上的[源代码](https://github.com/syl20bnr/spacemacs)。你可以开始了解 Emacs Lisp，并能知道 Spacemacs 是怎样工作的。你还能通过如下快捷键来探索：

<table border="1">

<tbody>

<tr>

<th> 快捷键</th>

<th> 功能</th>

</tr>

<tr>

<td>    SPC f e h</td>

<td>    列出所有层并允许你浏览层上的文件。</td>

</tr>

<tr>

<td>    SPC ?</td>

<td>    列出所有快捷键。</td>

</tr>

</tbody>

</table>

### 自定义

**.spacemacs  文件**

首次启动 spacemacs 时，会提示你选择编辑样式。如果你现在正读到这里，你可能会选择 vim 样式。这样将会使用选择的相应样式创建一个  .spacemacs   文件。大多数琐碎的配置都在这个文件中。

在这个文件中有四个顶级函数：dotspacemacs/layers,  dotspacemacs/init，dotspacemacs/user-init 和 dotspacemacs/user-config。

Dotspacemacs/layers 函数仅用于启用和禁用层和包。Dotspacemacs/init  函数是在启动过程中，在其他东西运行前运行，并且包含  Spacemacs  设置。 除非你需要更改默认 Spacemacs 设置，否则你不用动这个函数。Dotspacemacs/user-init 函数也是在其他程序运行前运行，并包含用户特定配置。Dotspacemacs/user-config 函数是你用到最多的函数。 在这里，你可以定义任何用户配置。

<table>

<tbody>

<tr>

<th>快捷键  
</th>

<th>                功能</th>

</tr>

<tr>

<td>                SPC f e d</td>

<td>                打开你的 .spacemacs</td>

</tr>

<tr>

<td>                SPC f e D</td>

<td>                使用<span style="font-size:12px;line-height:18px;background-color:#F6F6F6;">diff 通过</span>默认模版手动更新你的 .spacemacs   

</td>

</tr>

</tbody>

</table>

### Emacs Lisp

这个部分介绍几个 配置 Spacemacs 需要的 Emacs Lisp 函数。如需详细了解这个语言，请查看次链接。如果你很想了解 emacs lisp 的一切，请使用 SPC h i elisp RET 上的信息页面

### 变量

设置变量是定制 Spacemacs 行为最常见的方式。语法很简单：
```lisp
(setq variable value) ; Syntax
;; Setting variables example
(setq variable1 t ; True
      variable2 nil ; False
      variable3 '("A" "list" "of" "things"))
```

### 快捷键

定义快捷键是几乎每个人都想做的事情，最好的方式就是使用内置的 define-key 函数。

```lisp
(define-key map new-keybinding function) ; Syntax
;; Map H to go to the previous buffer in normal mode
(define-key evil-normal-state-map (kbd "H") 'spacemacs/previous-useful-buffer)
;; Mapping keybinding to another keybinding
(define-key evil-normal-state-map (kbd "H") (kbd "^")) ; H goes to beginning of the line
```

map 是你想要绑定键位到的 keymap。大多数情况下你会使用 evil-<state-name>-state-map。其对应不同的 evil-mode 状态。例如，使用 evil-insert-state-map 映射用于插入模式的快捷键。

使用 evil-leader/set-key 函数来映射 <Leader> 快捷键。

```lisp
(evil-leader/set-key key function) ; Syntax
;; Map killing a buffer to <Leader> b c
(evil-leader/set-key "bc" 'kill-this-buffer)
;; Map opening a link to <Leader> o l only in org-mode
(evil-leader/set-key-for-mode 'org-mode
  "ol" 'org-open-at-point)
```

### 函数

你可能偶尔想要定义一个函数做更复杂的定制，语法很简单：

```lisp
(defun func-name (arg1 arg2)
  "docstring"
  ;; Body
  )
;; Calling a function
(func-name arg1 arg1)
```

这里有个现实可用的示例函数：

```lisp
;; This snippet allows you to run clang-format before saving
;; given the current file as the correct filetype.
;; This relies on the c-c++ layer being enabled.
(defun clang-format-for-filetype ()
  "Run clang-format if the current file has a file extensions
in the filetypes list."
  (let ((filetypes '("c" "cpp")))
    (when (member (file-name-extension (buffer-file-name)) filetypes)
      (clang-format-buffer))))
;; See http://www.gnu.org/software/emacs/manual/html_node/emacs/Hooks.html for
;; what this line means
(add-hook 'before-save-hook 'clang-format-for-filetype)
```

## 激活一个层

正如上文术语那段所说，层提供一个简单的方式来添加特性。可在 .spacemacs 文件中激活一个层。在文件中找到 dotspacemacs-configuration-layers 变量，默认情况下，它看起来应该是这样的：

```lisp
(defun dotspacemacs/layers ()
  (setq-default
   ;; ...
   dotspacemacs-configuration-layers '(;; auto-completion
                                       ;; better-defaults
                                       emacs-lisp
                                       ;; (git :variables
                                       ;;      git-gutter-use-fringe t)
                                       ;; markdown
                                       ;; org
                                       ;; syntax-checking
                                       )))
```

你可以通过删除分号来取消注释这些建议的层，开箱即用。要添加一个层，就把它的名字添加到列表中并重启 Emacs 或按 SPC f e R。使用 SPC f e h 来显示所有的层和他们的文档。

### 创建一个层  

为了将配置分组或当配置与你的 .spacemacs 文件之间不匹配时，你可以创建一个配置层。Spacemacs 提供了一个内建命令用于生成层的样板文件：SPC :configuration-layer/create-layer。这条命令将会生成一个如下的文件夹：

```bash
[layer-name]
  |__ [local]*
  | |__ [example-mode-1]
  | |     ...
  | |__ [example-mode-n]
  |__ config.el*
  |__ funcs.el*
  |__ keybindings.el*
  |__ packages.el

[] = 文件夹
* = 不是命令生成的文件
```

Packages.el 文件包含你可以在 <layer-name>-packages 变量中安装的包的列表。所有 [MELPA](http:melpa.org) 仓库中的包都可以添加到这个列表中。还可以使用 :excludedt 特性将包包含在列表中。每个包都需要一个函数来初始化。这个函数必须以这种模式命名：<layer-name>/init-<package-name>。这个函数包含了包的配置。同时还有一个 pre/post-init 函数来在包加载之前或之后运行代码。它看起来想这个样子：

```lisp
(setq layer-name-packages '(example-package
                            ;;这个层通过设置:excluded 属性
                            ;;为真(t)来卸载example-package-2
                            (example-package-2 :excluded t)))
(defun layer-name/post-init-package ()
  ;;在这里添加另一个层的包的配置
  )
(defun layer-name/init-example-package ()
  ;;在这里配置example-package
  )
```

**注意**：只有一个层可以具有一个对于包的 init 函数。如果你想覆盖另一个层对一个包的配置，请使用 [use-package hooks](https://github.com/syl20bnr/spacemacs/blob/master/doc/LAYERS.org#use-package-hooks) 中的 <layer-name>/pre-init 函数。

如果 MELPA 中没有你想要的包，你必须是由一个本地包或一个包源。关于此的更多信息可以从[层的剖析](https://github.com/syl20bnr/spacemacs/blob/master/doc/LAYERS.org#anatomy-of-a-layer)处获得。

确保你[添加](https://github.com/syl20bnr/spacemacs/blob/master/doc/VIMUSERS.org#activating-a-layer)了你的层到你的 .spacemacs 文件中，并重启 spacemacs 以激活。

关于层的加载过程和层的工作原理的详细描述可以参考[LAYERS.org](https://github.com/syl20bnr/spacemacs/blob/master/doc/LAYERS.org)。

## 安装一个单独的包

有时创建一个层会有点大材小用了，也许你仅仅想要一个包而不想维持整个层。Spacemacs 在 .spacemacs 文件中的 dotspacemacs/layers 函数里提供了一个叫做 dotspacemacs-additional-packages 的变量，只要在列表中添加一个包名，它就会在你重启的时候被安装。下一段来说明如何加载这个包。

## 加载包

有没有想过 Spacemacs 如何可以在仅仅几秒钟之内加载超过 100 个包呢？如此低的加载时间必须需要某种难以理解的黑魔法吧。还好这不是真的，多亏有了 [use-package](https://github.com/jwiegley/use-package)。它是一个可以轻松实现对包进行延迟加载和配置的包。以下是它的基础用法：

```lisp
;; Basic form of use-package declaration. The :defer t tells use-package to
;; try to lazy load the package.
(use-package package-name
  :defer t)
;; The :init section is run before the package loads The :config section is
;; run after the package loads
(use-package package-name
  :defer t
  :init
  (progn
    ;; Change some variables
    (setq variable1 t variable2 nil)
    ;; Define a function
    (defun foo ()
      (message "%s" "Hello, World!")))
  :config
  (progn
    ;; Calling a function that is defined when the package loads
    (function-defined-when-package-loads)))
```

这只是 use-package 的一个非常基本的概述。它还有许多其他的方式来控制包的加载，就不在这里介绍了。

## 卸载一个包

Spacemacs 在 .spacemacs 文件中的 dotspacemacs/init 函数里提供了一个叫做 dotspacemacs-excluded-packages 的变量。只要在列表中添加一个包名，它就会在你重启的时候被卸载。

## 常见调整

本段是为了想要做更多调整的人所写的。除非另有说明，所有这些设置都去你的 .spacemacs 文件中的 dotspacemacs/user-config 函数里完成。

### 变更 escape 键

 Spacemacs 使用 [[https://github.com/syl20bnr/evil-escape][evil-escape]] 来允许从许多拥有一个快捷键的 major-modes 中跳出。你可以在你的 dotspacemacs/user-config 函数中像这样定制变量：

```lisp
(defun dotspacemacs/user-config ()
  ;; ...
  ;; Set escape keybinding to "jk"
  (setq-default evil-escape-key-sequence "jk"))
```

更多的文档可以在 evil-escape [README](https://github.com/syl20bnr/evil-escape/blob/master/README.md) 中找到。

### 变更配色方案

.spacemacs 文件的 dotspacemacs/init 函数中有一个 dotspacemacs-themes 变量。这是一个可以用 SPC T n 键循环的主题的列表。列表中的第一个主题是在启动时加载的主题。以下为示例：

```lisp
(defun dotspacemacs/init
    ;; Darktooth theme is the default theme
    ;; Each theme is automatically installed.
    ;; Note that we drop the -theme from the package name.
    ;; Ex. darktooth-theme -> darktooth
    (setq-default dotspacemacs-themes '(darktooth
                                        soothe
                                        gotham)))
```

可以使用 SPC T h 键列出和选择所有已安装的主题。

### 非高亮搜索

Spacemacs 模仿了默认的 vim 行为，会高亮显示搜索结果，尽管你不在它们之间进行导航。你可以使用 SPC s c 或 :nohlsearch 来关闭搜索结果高亮。

若再也不需要自动高亮结果，你可以[卸载](https://github.com/syl20bnr/spacemacs/blob/master/doc/VIMUSERS.org#uninstalling-a-package) evil-search-highlight-persist 包。

### 会话

当你打开 Spacemacs 时，它不会自动恢复窗口和缓冲区。如果你常使用 vim 会话，你可能要在你的 .spacemacs 文件中的 dotspacemacs/user-config 里添加 (desktop-save-mode t)，然后你就可以使用 SPC : desktop-read 加载已被保存的会话。桌面文件的位置可以使用 desktop-dirname 变量设置。要自动加载一个会话，就在你的 .spacemacs 文件中添加 (desktop-read)。

### 使用 visual lines 导航

Spacemacs 使用 vim 默认 <span style="font-size:13.3333px;">actual lines 导航，即使它们被包装了。如果你想要让 j 和 k 的行为如 g j 和 g k 一般，将一下代码添加到你的 .spacemacs 文件：</span>

```lisp
(define-key evil-normal-state-map (kbd "j") 'evil-next-visual-line)
(define-key evil-normal-state-map (kbd "k") 'evil-previous-visual-line)
```

### 其他有用的连接
*   [Emacs Manual](https://www.gnu.org/software/emacs/manual/emacs.html)
*   [Spacemacs Documentation](https://github.com/syl20bnr/spacemacs/blob/master/doc/DOCUMENTATION.org)
*   [Spacemacs: A Vimmer’s Emacs Prerequisites](http://ian.mccowan.space/2015/04/07/Spacemacs/)
*   [Configuring Spacemacs: A Tutorial](http://thume.ca/howto/2015/03/07/configuring-spacemacs-a-tutorial/)
*   [From Vim to Emacs+Evil chaotic migration guide](http://juanjoalvarez.net/es/detail/2014/sep/19/vim-emacsevil-chaotic-migration-guide/)