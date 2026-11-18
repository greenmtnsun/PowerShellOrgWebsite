---
title: "What the ISE Did Well, and What PowerShell 7 Users Lost"
description: "The Windows PowerShell ISE stopped at 5.1 on purpose. Here is what it got right, why there is no ISE for PowerShell 7, and what an admin moving from 5.1 to 7 should expect from the editors that remain."
author: Keith Ramsey
authors:
  - Keith Ramsey
date: "2026-11-18T00:00:00+00:00"
categories:
  - PowerShell for Admins
tags:
  - ise
  - powershell-7
  - vs-code
  - editors
  - windows-powershell
draft: true
---

Before anything else: I build a commercial, ISE-style editor for PowerShell 7 called DetentShell. This article is not about it. It is about the ISE, why Microsoft stopped it, and what that means if you spent years in it. I mention my own product once, at the end, next to the free option Microsoft recommends.

## What the ISE was

The Windows PowerShell Integrated Scripting Environment shipped inside Windows from PowerShell 2.0 onward. You did not download it. You did not install it. On any supported Windows box, `powershell_ise` was already there, which is a bigger deal than it sounds when you work on servers you do not own.

It paired a script pane with a live console in one window. Write above, run below. The console was the real PowerShell host, so after a run your variables were still sitting there to poke at. The keyboard did the rest:

- F5 ran the script.
- F8 ran only the selected text, which is how most of us learned to debug by halves.
- F9 set a breakpoint, F10 stepped over, F11 stepped in.
- F1 opened help for the cmdlet under the cursor.
- Ctrl+T opened another PowerShell tab, which meant another runspace.
- Ctrl+Space gave IntelliSense from PowerShell 3.0 on.

None of that was remarkable on its own. What was remarkable was that it was all there by default, in the same place, on every machine, with no setup.

## Why it stopped at 5.1

Microsoft's documentation says it plainly. The ISE is "no longer in active feature development", it is supported "up to and including Windows PowerShell V5.1", and "there is no support for the ISE in PowerShell v6 and beyond". The same page points people at Visual Studio Code with the PowerShell extension.

The reason is the runtime underneath. Windows PowerShell 5.1 is the last version of Windows PowerShell. It runs on .NET Framework and only on Windows. PowerShell 7 is a different engine: cross-platform, open source, built on current .NET. The ISE is a WPF application wired tightly to the Windows PowerShell host, and nobody rebuilt it for the new engine. That was a decision, not an oversight. VS Code was already the cross-platform editor Microsoft was investing in, and a Windows-only, built-in editor did not fit where PowerShell was going.

So PowerShell 7 shipped without an ISE, and that is still true today.

## What a 5.1 admin actually loses

If you move from the ISE on 5.1 to PowerShell 7, two separate things change at once, and it helps to keep them apart.

The engine changes. Some Windows-native modules still expect 5.1. Intune, Configuration Manager and Group Policy scripting are the usual examples, and a handful of cmdlets have not carried over to 7. No editor fixes that. It is a fact about the platform, and the honest answer for a lot of shops is that both engines stay installed for years.

The editor changes. This is the part people feel in their hands:

- The console is no longer the bottom half of the window by default. In VS Code it is a terminal panel, one of several, and you choose which shell lives in it.
- F8 is not wired to "run selection" until the PowerShell extension is installed and active. Then it is.
- Variables surviving between runs depends on whether you are using the extension's integrated console or a plain terminal.
- There is no PowerShell-specific theme or layout on first launch. You set it up.
- Nothing is "already there" on a server. VS Code is an install, and the extension is another.

None of these are complaints about VS Code. It is a general-purpose editor that became a very good PowerShell editor through an extension, and it is free, maintained, and cross-platform. The point is that it starts from a different place than the ISE did, and an admin who never configured an editor before now has to.

## The free path, done properly

If you are moving to VS Code, three settings close most of the gap:

1. Install the PowerShell extension and let it start the PowerShell Integrated Console. That console keeps state between F8 runs the way the ISE console did.
2. Turn on `powershell.integratedConsole.focusConsoleOnExecute` so a run drops your cursor into the console, which is what your hands expect.
3. Set the default terminal profile to PowerShell 7 and keep a second profile for Windows PowerShell 5.1, so the scripts that still need 5.1 have somewhere to run.

With those in place, VS Code is a fine place to live. Many people never look back.

## The other option

Some of us wanted the ISE shape itself, on PowerShell 7, with nothing to configure. That is what I built. DetentShell opens straight into the two-pane layout, runs PowerShell 7 in-process, keeps F5, F8 and F9 where they were, and puts a Windows PowerShell 5.1 tab beside the 7 tabs for the scripts that still need it. It is an independent product, not a Microsoft one, and it costs money, with a trial. I keep a comparison of the four editors most PowerShell people use, including the rows where VS Code wins, in my author profile link.

Whichever way you go, the ISE itself is not coming back. Microsoft said so, and the runtime it was built on is in maintenance. What is worth keeping is the list above: a console that remembers, a key that runs the selection, help one keystroke away, and nothing to install before you can start. Judge any editor you move to against that list, because that is what the ISE got right.
