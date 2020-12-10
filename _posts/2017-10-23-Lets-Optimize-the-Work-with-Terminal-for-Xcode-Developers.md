---
layout: post
title: "Let’s Optimize the Work with Terminal for Xcode Developers"
subtitle: "Lately I’ve often had to switch between different Xcode projects and every time I had to change a path to a specific project in terminal. I am so tired of it! I want to have the opportunity to open Terminal right from Xcode with an already specifed path."
date: 2017-10-23 16:47:12 +0300
image: /assets/img/articles/xcode-terminal/xcode-terminal.png
---

Every iOS Developer often has to deal with things that need to be done via Terminal:

- work with git;
- upload a build for customers via fastlane;
- update dependencies via Carthage, Cocoapods or SPM and so on.

Lately I’ve often had to switch between different Xcode projects and every time I had to change a path to a specific project in terminal. I am so tired of it! I want to have the opportunity to open Terminal right from Xcode with an already specifed path. 🤔 If you feel the same then continue reading!

<hr>

**First**, we need to create a script with the following body:

```bash
#!/bin/sh

if [ -n "$XcodeProjectPath" ]; then	
  open -a Terminal "$XcodeProjectPath"/..
else		
  open -a Terminal "$XcodeWorkspacePath"/..
fi
```

After that don’t forget to make this script as an executable by the special command: `chmod +x <your_script.sh>` (you can check more about this command [here](https://www.freebsd.org/cgi/man.cgi?query=chmod&sektion=1)) and save it to some directory wherever you want (in my case it’s a user directory).

If you prefer iTerm, just change the Terminal keyword to iTerm in this script.

The next one is creating a custom Xcode’s behavior. Open Xcode and follow the next steps: *Xcode menu > Behaviors > Edit Behaviors…* Then at the bottom of the popup appeared press the + button.

![](/assets/img/articles/xcode-terminal/1.png)

Choose a recognizable name for this behaviour, setup a shortcut, select the `Run` checkbox and select the previously configured script. You only need to configure it once and it’ll work for all your projects. 🎉

<hr>

Thanks for reading! I hope you’ve enjoyed my Terminal optimization tip. If you had to face the same issue but resolved it in a different way — [contact me](/tabs/contact.html). 🙏