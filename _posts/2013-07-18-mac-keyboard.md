---
layout: post
title:  "mac下使用机械键盘"
date:   2013-03-18 14:06:05
categories: Mac
---

* content
{:toc}

为了将Home,End等利用起来,在~/Library/下建立目录KeyBindings,在下面建立文件DefaultKeyBinding.dict,内容为

    {
    /* Remap Home / End keys to be correct */
    "\UF729" = "moveToBeginningOfLine:"; /* Home */
    "\UF72B" = "moveToEndOfLine:"; /* End */
    "$\UF729" = "moveToBeginningOfLineAndModifySelection:"; /* Shift + Home */
    "$\UF72B" = "moveToEndOfLineAndModifySelection:"; /* Shift + End */
    "^\UF729" = "moveToBeginningOfDocument:"; /* Ctrl + Home */
    "^\UF72B" = "moveToEndOfDocument:"; /* Ctrl + End */
    "$^\UF729" = "moveToBeginningOfDocumentAndModifySelection:"; /* Shift + Ctrl + Home */
    "$^\UF72B" = "moveToEndOfDocumentAndModifySelection:"; /* Shift + Ctrl + End */
    /* page up/down */
    "\UF72C"  = "pageUp:";
    "\UF72D"  = "pageDown:";
    }

再将Command和Ctrl交换一下,感觉就比较舒服了