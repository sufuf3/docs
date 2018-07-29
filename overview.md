# CORD 指南

這是一套描述如何安裝、維運、測試與開發 [CORD](https://opencord.org) 的指南。

CORD 是一個基於社群的開源專案。除了本指南外，您也可以在 [CORD wiki](https://wiki.opencord.org) 找到社群的相關資訊。包含比較早期的白皮書與設計筆記，這些都構成了 [CORD 的架構](https://wiki.opencord.org/display/CORD/Documentation)。

## 指南導覽

這份指南主要圍繞在 CORD 的生命週期：

* [Installation](README.md): CORD 安裝 (以及最新的升級)。
* [Operations](operating_cord/operating_cord.md): 操作已經完成部署安裝的 CORD。
* [Development](developer/developer.md): 在 CORD 中開發新功能。
* [Testing](cord-tester/README.md): 在 CORD 中的測試功能。

這些都是顯而易見的，如果不清楚的話，通常是這些 stages 的關係，可以參考 [Navigating CORD](navigate.md)。

## Making Changes to the Guide

The [http://guide.opencord.org](http://guide.opencord.org) website is built
using the [GitBook Toolchain](https://toolchain.gitbook.com/), with the
documentation root in
[docs](https://github.com/opencord/docs/blob/{{ book.branch }}) in a
checked out source tree.  It is build with `make`, and requires that `gitbook`,
`python`, and a few other tools are installed.

Source for individual guides is available in the [CORD code
repository](https://gerrit.opencord.org); look in the `docs` directory of each
project, with the documentation rooted in the top-level `/docs`
directory. Updates and improvements to this documentation can be
submitted through Gerrit.
