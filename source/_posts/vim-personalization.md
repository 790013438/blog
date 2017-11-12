---
title: vim_personalization
date: 2017-11-06 11:03:38
tags:
---

## configuration files

### 三个配置文件
* vimrc
    * The global vimrc file is placed in the folder where all of your Vim system files are installed.
    > 全局vimrc
    ```bash
    :echo $VIM
    ```
    * You can find out what Vim considers as the home directory on your system by executing the following command in normal mode:
    > 个人vimrc
    ```bash
    :echo $HOME
    ```
    > Another way of finding out exactly which vimrc file you use as your personal file is by executing the following command in the normal mode:
    ```bash
    :echo $MYVIMRC
    ```
* gvimrc
    * The gvimrc file does not replace the vimrc file, but is simply used for configurations that only apply to the GUI version of Vim.
    > 用于gui的配置
* exrc
    * The exrc file is a configuration file that is only used for backwards compatibility with the old vi /ex editor.
    > 用的很少，要兼容时才用
Changing the fonts
------------------
> 字体
一般来说，由于Vim的字体是由终端决定的，因此不太可能更改。然而，在Gvim中，你可以随心所欲地改变字体。
* The main command for changing the font in Linux is:
    ```bash
    :set guifont=Courier\ 14
    ```
* For changing the font in Windows, use the following command:
    ```bash
    :set guifont=Courier:14
    ```
* If you are not sure about whether a particular font is available on the computer or not, you can add another font at the end of the command by add a comma between the two fonts.For example:
    ```bash
    :set guifont=Courier\ New\ 12, Arial\ 10
    ```
