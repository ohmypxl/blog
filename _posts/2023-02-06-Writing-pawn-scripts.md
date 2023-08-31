---
title: Writing Pawn Scripts
author: ohh
categories: [pawn, development, tutorial]
tags: [pawn tutorial, new stuff]
---

Today, i'm thinking about making a good tutorial that covers basic pawn usage for newly developer who want to learn pawn but don't want to read boring documentations that feels like it's making them want to ask more question about pawn itself. I mean i get it from the perspective of myself back then when i was really new into pawn stuff and reading pawn-lang.pdf is just like a hell to me since the documentation pdf is kinda diffrent than pawn that is samp used (it's taken from 3.4.xx source if i'm remember it correctly), hopefully this will help you learn some basic stuff about pawn.

Firstly, we must know that Pawn is a simple, easy to use, portable, and typeless language that is used in [San Andreas Multiplayer](https://sa-mp.com), [Open Multiplayer](https://open.mp), and [any other projects](https://www.compuphase.com/pawn/pawnprojects.htm) that is still using pawn to this very day. Pawn is created back in the 97' by [CompuPhase](https://github.com/compuphase), the language itself is not named PAWN as is today but rather it was SMALL until CompuPhase decided to change it to Pawn due to some reason. 

The language itself is simple to use and it kinda resembles C a lot, so if you trying pawn while have any experience with C/C++, learning this language is fairly easy compared to some people who's originally coming from Java, python, etc. With this aside we can start writing pawn code. The pawn version i will teaching you guys is [3.10.10 or higher](https://github.com/pawn-lang/compiler/releases), and that is meant to be used for SA:MP and open.mp server project since nowdays pawn itself is at version 4 and it also have types like `int, float, etc`, not really relying on the `tags:` anymore (i'll talk about this later).


# Chapter 1 - Setting up the environment

Before we begin, there is some steps that you must do before began writing pawn scripts which is setting up your project environment, but you need to think first about what `host` or `server` you're going to choose to, is it SA:MP or open.mp? For the sake of this tutorial i will be going to give a tutorial about open.mp since SA:MP is kinda ded.

Let's download [open.mp server](https://github.com/openmultiplayer/open.mp/releases) and then extract the contents of the zip/tar.gz into any directories you want to extract (mine will extract it under `Documents/pawn_tutorial/`). If you downloaded windows version, you're lucky because there is Qawno that you can used to write pawn scripts where linux users... well they need to compile their own compiler like i do.

Now, let's create some simple gamemode scripts using open.mp by creating `gamemodes` folder if it doesn't exists and then open your Qawno file, create new file under `gamemodes` folder that you just created in your projects folder and then we can finally start codin' stuff.

# Chapter 2 - Writing The Scripts

Like any programming tutorial, we'll starting with creating `hello world!` first in pawn scripting language, no need to rush things, all you need to do is patience while learning the programming language. 


The code itself looks like this:
```c
#include <open.mp>

main()
{
    print("Hello World");
}
```

It's really simple right? you might want to ask several questions about "Hey, what is that `main` stuff" or "How `includes` works, is it plugin or smth?"... Don't worry i'll try to explain the code line-by-line and hopefully it will answer your doubts and your questions.

### #include \<open.mp\>

This is include file, just like C/C++ where it contains stuff that you need for writing open.mp pawn scripts. It already includes some of useful callbacks, functions, and even `print` function itself instead of you including `console` and then do the rest.

You must be confused about `console` include huh?, well it's an include that is for importing `print` and `printf` from the open.mp server... Yes the contents of that include it's just that, nothing else. If you're using `#include` on Pawn Scripts, the contents of the file inside the `<>` will be copied into your script via internally before the compilation happens. Well i'll give you a brief what's going on with this nice example below:

Let's say i have `a.inc` that contains:
```c
native print(const text[]);

SaySomething()
{
    print("Hello from a.inc");
}
```

And then i have `b.pwn` that contains:
```c
#include "a.inc"

main()
{
    SaySomething();
}
```

When i compile my project, this is what it looks like just before the compilation happens:

```c
// a.inc
SaySomething()
{
    print("Hello from a.inc");
}

// b.pwn
main()
{
    SaySomething();
}
```

You can see the process if you add `#pragma option -l` in your script if you're curious and don't believe what i just said and then you compile the script, it will generate a file named `b.asm` that will tell you how your truest form of your gamemode before compilation.

So, i'm assuming you understand this part, let's move on to the another code lines shall we?

### main\(\)

This piece of code is your entry script, it only calls this when everyhing is loaded (like your plugins, filterscripts, and the gamemode), usually this function is really useless for some developers since there is some event/callback that will be called whenever your script is being loaded from the server and they're using this function to avoid runtime errors (usually a developer like me just using this function to display copyright footer stuff).

Oh i forgot to tell you `main()` is a function, a function for those who don't know is like a package that contains your code or statement for then to be used into other function. This is useful for developers instead of writing several code manually, you bundled them into single function and then can be used anywhere you want.

That's what i'm about to say about this line of code, moving on.

### print\(\"Hello World\"\);

This code is outputting message that you add in the `""` (double quote) to the console, terminal, or open.mp server.... Yeah that's what it does. There is two version of print which you can use, `print` and `printf`. `print` function is used when you just want to send messages without adding anything to format while `printf`... `f` as the name suggest, it will format your message and then outputing it in the console (i'll just call open.mp server as console now).

Okay finally we done with this stuff, let's compile and run the server now. If you still have `#pragma option -l` you better remove that first, otherwise it will not working as you expected (since the code will still generating `.asm` instead of `.amx`).

# Chapter 3 - Running the scripts

When you compile the code, it produces binary file called `AMX`, AMX itself stands for Abstract Machine eXecutor, it's a file that SA:MP and open.mp needed to be loaded into the server, not hte `pwn`, `pawn` or `inc` ones. Since `AMX` is a binary file, it can only be understand by computers, unlike `pwn`, `pawn`, and `inc` that can only be understand by humans... That is why SA:MP and open.mp needed it in order for your scripts to work as expected.

Before we run the script, you might want to generate the config from open.mp by running this command:

Windows:
```powershell
.\omp-server --default-config
```

Linux:
```bash
./omp-server --default-config
```

if you already do that then open up the `config.json` file and go to this section:
```json
"pawn": {
    "legacy_plugins": [],
    "main_scripts": [
        "test 1"
    ],
    "side_scripts": []
}
```

If your gamemode name is not `test.amx`, you can change `test` in `main_scripts` section into whatever you want, i created my gamemode and amx named `main` so i just replace the text from `test` to `main`, after that we can just simply run the server by running `omp-server` like above but without `--default-config`.

If you see this:
```bash
Hello World
```

Congratulations, you just write simple hello world pawn scripts, yaay! 👏👏👏
