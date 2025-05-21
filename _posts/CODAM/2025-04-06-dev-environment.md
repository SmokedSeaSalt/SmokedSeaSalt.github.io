---
title: Dev Environment
description: My CODAM dev environment settings.
author: Mathijs
date: 2025-04-06 21:52:00 +0100
categories: [CODAM]
tags: []
pin: false
math: false
mermaid: false
---

## test
input



## My CODAM banner
```
  let s:asciiart = [
\'        ,--------------',
\'       /               ',
\'      /                ',
\'     /   ,--,/      ,--',
\'     |  /  ,'    ,·´   ',
\'     |  ;·'   ,·´      ',
\'     |  ;  ,·´         ',
\'     |  :,'            ',
\'      `´.              ',
\'       .¨.             ',
\'       ¨· .            ',
\"      :. ¨.            "
\]

* **************************************************************************** *
*                                                                              *
*                                                        ,--------------       *
*                                                       /                      *
*                                                      /                       *
*                                                     /   ,--,/      ,--       *
*                                                     |  /  ,'    ,·´          *
*                                                     |  ;·'   ,·´             *
*   $FILENAME__________________________________       |  ;  ,·´                *
*                                                     |  :,'                   *
*   By: $AUTHOR________________________________        `´.                     *
*                                                       .¨.                    *
*   Created: $CREATEDAT_________ by $CREATEDBY_         ¨· .                   *
*   Updated: $UPDATEDAT_________ by $UPDATEDBY_        :. ¨.                   *
*                                                                              *
* **************************************************************************** *

in header.js -> var headerRegex = "^(.{80}\n){15}";

in extension.js -> Range(0, 0, 17, 0) //note 2 times

var genericTemplate = "\n********************************************************************************\n*                                                                              *\n*                                                        ,--------------       *\n*                                                       /                      *\n*                                                      /                       *\n*                                                     /   ,--,/      ,--       *\n*                                                     |  /  ,'    ,·´          *\n*                                                     |  ;·'   ,·´             *\n*    $FILENAME__________________________________      |  ;  ,·´                *\n*                                                     |  :,'                   *\n*    By: $AUTHOR________________________________       `´.                     *\n*                                                       .¨.                    *\n*    Created: $CREATEDAT_________ by $CREATEDBY_        ¨· .                   *\n*    Updated: $UPDATEDAT_________ by $UPDATEDBY_       :. ¨.                   *\n*                                                                              *\n********************************************************************************\n\n".substring(1);


```
