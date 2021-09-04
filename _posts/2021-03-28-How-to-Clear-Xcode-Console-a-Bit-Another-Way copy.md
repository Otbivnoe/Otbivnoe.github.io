---
layout: post
title: "How to clear Xcode console a bit another way"
subtitle: "I think everyone has had a whole debugging day trying to figure out how this bug was caught by a QA team. Embellishes the fact you spend a lot of time repeating these steps to reproduce the bug even once but the QA team does it regularly. Familiar feeling, isn't it? We have such days from time to time and I had a similar one recently. I turned out as a detective and started to investigate the problem."
date: 2021-03-28 19:00:00 +0300
image: /assets/img/articles/xcode-clear/logo.png
---

I think everyone has had a whole debugging day trying to figure out how this bug was caught by a QA team. Embellishes the fact you spend a lot of time repeating these steps to reproduce the bug even once but the QA team does it regularly. Familiar feeling, isn't it? We have such days from time to time and I had a similar one recently. I turned out as a detective and started to investigate the problem. 

The first step of my investigation was turning on verbose logging. Thereby Xcode's console started to spam plenty of messages. Yep, it became a mess by far. And since my goal was finding a "key" inside this galaxy of logs, I became clicking the tiny icon "trash" on every lap of my investigation steps to start looking again. On the 25th lap, I opened the StackOverflow trying to find a programmable way to clear the console - without success. On that note, I took off the detective hat to find a way to simplifying the debug workflow a bit.

<hr>

The first thought that visited me - my old friend Apple Script. So I wrote a script in minutes:

```applescript
activate application "Xcode"

tell application "System Events" to tell process "Xcode"
	click menu item "Clear Console" of menu 1 of menu item "Debug Workflow" of menu 1 of menu bar item "Debug" of menu bar 1
end tell
```

The longest line of this code just invokes the _"Clear Console"_ command from the Xcode's menu: _Debug > Debug Workflow > Clear Console._ 

Of course, it does look nicer using a shortcut invocation from the script, but I think it's less safe. 

```applescript
keystroke "K" using {command down}
```

Now that we have the worked script, need to find a way to invoke it right from Xcode. The handy thing you may not have heard, it's the ability to invoke an apple script right from a breakpoint: _(Right-click on any breakpoint) > Edit breakpoint > Add Action > Chouse 'Apple Script' from dropdown > (paste our script):_

<img class="centered post-img" srcset="/assets/img/articles/xcode-clear/breakpoint-aplescript.png" alt="">

At this step, I could get back to the initial problem, but the current solution doesn't look elegant as could be. What if I decide to repeat this code for another breakpoint? You know for sure that developers are lazy people, so we don't like to repeat code and maintain its consistency across many places in our codebase. Thus I went further!

Ideally is to have a simple [LLDB](https://lldb.llvm.org/use/python-reference.html#create-a-new-lldb-command-using-a-python-function) command - `clear` for instance. From my experience, a developer is able to write a LLDB command on his own. So if you haven't heard about this feature I recommend to read [this simple article](https://www.ryanipete.com/blog/lldb/python/how_to_create_a_custom_lldb_command_pt_1/) that covers the basics.

#### Steps:

- At first check the existence of the `~/.lldbinit` file (create if doesn't exist). It's the file that LLDB looks for and loads each time a new session is started. 
- Then we need to import our further Python script that clears the Xcode's console. Paste this code inside the `.lldbinit` file:

```python
command script import ~/.lldb/otbivnoe.py # you can choose any directory for your script
```

- Final part is finally to write the magic script:

```python
#!/usr/bin/env python

from subprocess import Popen, PIPE

def clearXcodeConsole(debugger, command, result, internal_dict):
  applescript='''
    activate application "Xcode"
    tell application "System Events" to tell process "Xcode"
      click menu item "Clear Console" of menu 1 of menu item "Debug Workflow" of menu 1 of menu bar item "Debug" of menu bar 1
    end tell
  '''

  p = Popen(['osascript', '-'], stdin=PIPE, stdout=PIPE, stderr=PIPE, universal_newlines=True)
  stdout, stderr = p.communicate(applescript)

def __lldb_init_module(debugger, internal_dict):	
  debugger.HandleCommand('command script add -f otbivnoe.clearXcodeConsole clear')
```

Be aware I have the following version of python. Maybe you need to tune this script a bit by yourself.

```shell
3.8.2 (default, Dec 21 2020, 15:06:04)
[Clang 12.0.0 (clang-1200.0.32.29)]
```

-  Restart your LLDB debugging session or just reload the current one:

```python
command source ~/.lldbinit
```

That's It! Now you're able to write a simple command `clear` to flush the console. And I go back to detective duties :)

<hr>

### Additional Recourses:

- [Chisel](https://github.com/facebook/chisel) - a ton of useful LLDB commands by Facebook
- [Lowmad](https://github.com/bangerang/lowmad) - a command line tool for managing scripts and configurations in LLDB.

<hr>

I would happy to hear a better way or even a native way of clearing a console if it exists. So [let me know](https://twitter.com/{{ site.twitter_username }}) if you know anything about it.