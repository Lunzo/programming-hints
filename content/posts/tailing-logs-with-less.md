---
title: "Tailing Logs with less"
date: 2014-09-22T05:46:00
tags: [linux, shell, less]
---
`less` is already an incredibly useful command in linux (GNU/linux for the purists). It's a better version of `more`, for paging through files. One not so well known switch for `less` is the following:

> less +F [file]

The `+F` option tails the end of the log. Updating when the log changes is buffered, so depending on how noisy your log file is there may be some delay in seeing updates.

At any time when `less` is in tailing mode you can hit `CTRL+C` to return to regular less. The status bar reminds you of this with a message like `interrupt to abort`. This feature is incredibly powerful, for example if something at the bottom of the log catches your eye and you want to stop tailing for a moment so you can check the logs around it.

In regular-mode `less`, pressing `shift+F` will put you into tailing mode. This is true even if you started `less` without the `+F` option.