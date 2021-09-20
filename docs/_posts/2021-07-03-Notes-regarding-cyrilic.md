---
layout: post
---

### Setting source encoding in gradle ###

```
compileJava.options.encoding = "UTF-8"
compileTestJava.options.encoding = "UTF-8"
```


### Cyrillic in Windows Terminal ###

On Windows to support output in cyrillic characters (if
default windows code page is "Western" or other non
cyrillic, then use `chcp 866` in the terminal.

When using gradle, to create directory with all jars and
startup script use `gradle installDist` (from distribution
plugin). It will create directory "build\installation"
with needed content.


### Cyrillic in IntelliJ ###

(This did not work for me, probably because default windows code page is not "cyrillic").

If using e.g. with Cyrillic, then IntelliJ by default showns
question marks. To solve it add "-Dfile.encoding=UTF-8" to the
`idea.exe.vmoptions` (in bin dir of IntelliJ, e.g.
`C:\Program Files\JetBrains\IntelliJ IDEA Community Edition 2021.1\bin`)
and to run configurations of the application.

See also [https://intellij-support.jetbrains.com/hc/en-us/community/posts/206971615-Cyrillic-symbols-in-console-on-mac](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206971615-Cyrillic-symbols-in-console-on-mac).
