# WhatsApp + Claude Setup Guide for Windows

### A complete, step-by-step guide for non-technical users

This guide will help you connect your **personal WhatsApp account** to **Claude** (the AI assistant), so you can ask Claude to read, search, and send WhatsApp messages for you.

**You do not need any technical experience.** Every step is explained in plain language, including exactly what to type, where to type it, and what you should see happen. Just follow the steps in order and do not skip any.

> **Roughly how long will this take?** About 30-45 minutes the first time. Most of that is installing software and waiting for downloads.

---

## Table of Contents

1. [What you are building (in plain English)](#1-what-you-are-building-in-plain-english)
2. [Important things to know before you start](#2-important-things-to-know-before-you-start)
3. [The tools you will install](#3-the-tools-you-will-install)
4. [How to open the command line (you will need this a lot)](#4-how-to-open-the-command-line-you-will-need-this-a-lot)
5. [Step 1: Install Git](#step-1-install-git)
6. [Step 2: Install Go](#step-2-install-go)
7. [Step 3: Install Python](#step-3-install-python)
8. [Step 4: Install UV (Python helper)](#step-4-install-uv-python-helper)
9. [Step 5: Install a C compiler (MSYS2)](#step-5-install-a-c-compiler-msys2)
10. [Step 6: Install Claude Desktop](#step-6-install-claude-desktop)
11. [Step 7: Download the WhatsApp project](#step-7-download-the-whatsapp-project)
12. [Step 8: Start the WhatsApp bridge and scan the QR code](#step-8-start-the-whatsapp-bridge-and-scan-the-qr-code)
13. [Step 9: Connect the project to Claude](#step-9-connect-the-project-to-claude)
14. [Step 10: Restart Claude and test it](#step-10-restart-claude-and-test-it)
15. [Everyday use: how to start it again next time](#everyday-use-how-to-start-it-again-next-time)
16. [Troubleshooting](#troubleshooting)
17. [Optional: FFmpeg for voice messages](#optional-ffmpeg-for-voice-messages)

---

## 1. What you are building (in plain English)

You will set up **two small programs** on your Windows computer that work together:

1. **The WhatsApp Bridge** - connects to your WhatsApp account (the same way WhatsApp Web does in your browser) and keeps a private copy of your messages on your own computer.
2. **The MCP Server** - lets Claude talk to that bridge, so Claude can read and send messages when you ask it to.

Your messages are stored **only on your computer**. They are sent to Claude only when you specifically ask Claude to do something with them.

> **A quick safety note:** Because Claude can read your messages and act on them, be careful. A malicious message someone sends you could, in theory, try to trick Claude into doing something you did not intend. Only ask Claude to work with WhatsApp when you are paying attention to what it is doing.

---

## 2. Important things to know before you start

- **You will be typing commands into a "command line" window.** This is a black or blue text window where you type instructions. Do not be intimidated - you will only copy and paste what this guide gives you.
- **You will need to keep one window open at all times** while you use this. Closing it turns WhatsApp access off. (This guide explains this at the right step.)
- **You will scan a QR code with your phone**, exactly like setting up WhatsApp Web.
- **After you install a program, close and reopen your command line windows.** New programs are only recognized by command line windows opened *after* the install. This guide reminds you when it matters.

---

## 3. The tools you will install

Here is everything you will install, and why. You will install each one in the steps below - you do not need to do anything right now.

| Tool                        | What it is for                          | Download link                             |
| --------------------------- | --------------------------------------- | ----------------------------------------- |
| **Git**               | Downloads the project files             | https://git-scm.com/download/win          |
| **Go**                | Runs the WhatsApp bridge program        | https://go.dev/dl/                        |
| **Python**            | Runs the Claude connector program       | https://www.python.org/downloads/windows/ |
| **UV**                | A helper that runs the Python program   | Installed with a command (Step 4)         |
| **MSYS2**             | A "C compiler" that Go needs on Windows | https://www.msys2.org/                    |
| **Claude Desktop**    | The Claude app itself                   | https://claude.ai/download                |
| **FFmpeg** (optional) | Only for sending voice messages         | https://www.gyan.dev/ffmpeg/builds/       |

---

## 4. How to open the command line (you will need this a lot)

Throughout this guide you will "open a command line" or "open a terminal." On Windows this is a program called **PowerShell**. Here is how to open it:

1. Click the **Start** button (Windows logo, bottom-left of your screen).
2. Type the word: **PowerShell**
3. In the search results, click **Windows PowerShell**.
4. A window opens with white or light-blue text. **This is your command line.** You type commands here and press **Enter** to run them.

> **How to paste into PowerShell:** Copy the command normally (Ctrl+C). Then in the PowerShell window, click once and press **Ctrl+V**, or **right-click** to paste. Then press **Enter** to run it.

> **Tip:** You can have more than one PowerShell window open at once. You will need this later.

---

## Step 1: Install Git

**What this does:** Git is the tool that downloads the WhatsApp project onto your computer.

1. Go to **https://git-scm.com/download/win** in your web browser.
2. Click **"64-bit Git for Windows Setup"** (this starts the download).
3. Open the downloaded file (it will be in your **Downloads** folder, named something like `Git-2.xx.x-64-bit.exe`).
4. The installer will ask **a lot** of questions. **You can safely click "Next" on every screen** and then "Install" at the end. The default choices are fine.
5. When it finishes, click **Finish**.

**Check it worked:**

1. Open a **new** PowerShell window (see [Section 4](#4-how-to-open-the-command-line-you-will-need-this-a-lot)).
2. Type this and press Enter:

   ```powershell
   git --version
   ```
3. You should see something like `git version 2.43.0`. If you do, Git is installed correctly.

---

## Step 2: Install Go

**What this does:** Go is the programming language that the WhatsApp bridge is written in. You need it to run the bridge.

1. Go to **https://go.dev/dl/** in your web browser.
2. Click the file that ends in **`.windows-amd64.msi`** (for example, `go1.22.5.windows-amd64.msi`). This is the Windows installer.
3. Open the downloaded file from your **Downloads** folder.
4. Click **Next** through the installer and accept the defaults. Click **Install**, then **Finish**.

**Check it worked:**

1. Open a **new** PowerShell window (important: a new one, so it recognizes Go).
2. Type this and press Enter:

   ```powershell
   go version
   ```
3. You should see something like `go version go1.22.5 windows/amd64`.

---

## Step 3: Install Python

**What this does:** Python is the programming language the Claude connector is written in.

1. Go to **https://www.python.org/downloads/windows/** in your web browser.
2. Click the big yellow **"Download Python 3.x.x"** button near the top.
3. Open the downloaded file from your **Downloads** folder.
4. **VERY IMPORTANT:** On the first screen of the installer, look at the bottom. **Check the box that says "Add python.exe to PATH."** This is easy to miss and will cause problems later if you skip it.

   ![Reminder: tick "Add python.exe to PATH" before clicking Install]
5. Then click **"Install Now"**.
6. When it finishes, click **Close**.

**Check it worked:**

1. Open a **new** PowerShell window.
2. Type this and press Enter:

   ```powershell
   python --version
   ```
3. You should see something like `Python 3.12.4`.

> **If you see an error or the Microsoft Store opens instead:** it usually means the "Add to PATH" box was not checked. Re-run the installer, choose "Modify," and make sure that box is ticked.

---

## Step 4: Install UV (Python helper)

**What this does:** UV is a small helper tool that starts the Python connector for Claude. Claude needs it.

1. Open a **new** PowerShell window.
2. Copy and paste this command, then press Enter:

   ```powershell
   powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
   ```
3. Wait for it to finish. You will see some text scroll by.
4. **Close this PowerShell window and open a new one** (so it recognizes the new tool).

**Check it worked:**

1. In the new PowerShell window, type this and press Enter:

   ```powershell
   uv --version
   ```
2. You should see something like `uv 0.4.0`.

> **Write this down - you will need it in Step 9:** Type the command below and copy the full line it prints out. This is the location of `uv` on your computer.
>
> ```powershell
> (Get-Command uv).Source
> ```
>
> It will print something like `C:\Users\YourName\.local\bin\uv.exe`. Keep this text somewhere handy (paste it into Notepad).

---

## Step 5: Install a C compiler (MSYS2)

**What this does:** On Windows, the WhatsApp bridge needs a "C compiler" to work with its message database. This is the part most people get stuck on, so follow carefully.

### 5a. Install MSYS2

1. Go to **https://www.msys2.org/** in your web browser.
2. Click the download link near the top (a file named like `msys2-x86_64-xxxxxxxx.exe`).
3. Open the downloaded file and click **Next** through the installer, accepting the defaults. It installs to `C:\msys64` by default - leave that as is.
4. When it finishes, let it open the **MSYS2 terminal** (a new dark window). If it does not open automatically, open the Start menu, type **MSYS2**, and click **MSYS2 UCRT64**.

### 5b. Install the compiler inside MSYS2

1. In the **MSYS2 terminal window** (not PowerShell - the MSYS2 one), copy and paste this command and press Enter:

   ```bash
   pacman -S mingw-w64-ucrt-x86_64-gcc
   ```
2. When it asks `Proceed with installation? [Y/n]`, type **Y** and press Enter.
3. Wait for it to finish downloading and installing. Then you can close the MSYS2 window.

### 5c. Tell Windows where to find the compiler (add it to PATH)

This tells Windows to look in the MSYS2 folder for the compiler. Follow exactly:

1. Click the **Start** button and type: **environment variables**
2. Click **"Edit the system environment variables."**
3. In the window that opens, click the **"Environment Variables..."** button near the bottom.
4. In the **top box** (labeled "User variables"), find and click the row named **Path**, then click **Edit...**
5. Click **New**, and type this exactly:

   ```
   C:\msys64\ucrt64\bin
   ```
6. Click **OK** on each window to close them all (three OKs).

**Check it worked:**

1. **Close all PowerShell windows and open a brand-new one** (this is required for the change to take effect).
2. Type this and press Enter:

   ```powershell
   gcc --version
   ```
3. You should see something like `gcc (Rev...) 13.2.0`. If you do, the compiler is ready.

> **If you see "gcc is not recognized":** the PATH step did not take. Double-check you typed `C:\msys64\ucrt64\bin` exactly, and that you opened a **new** PowerShell window afterward.

---

## Step 6: Install Claude Desktop

**What this does:** This is the Claude app where you will actually chat and ask about WhatsApp.

1. Go to **https://claude.ai/download** in your web browser.
2. Download the **Windows** version and install it (open the downloaded file, click through the installer).
3. Open Claude Desktop and sign in with your Claude account (create a free account if you do not have one).
4. Once you have signed in successfully, **close Claude Desktop completely** for now (we will come back to it in Step 9). To close it completely: right-click the Claude icon near the clock (bottom-right of screen), and choose **Quit** if available.

---

## Step 7: Download the WhatsApp project

**What this does:** This downloads the actual WhatsApp-to-Claude project files onto your computer.

1. Open a **new** PowerShell window.
2. First, move into your user folder by typing this and pressing Enter:

   ```powershell
   cd $HOME
   ```
3. Now download the project by typing this and pressing Enter:

   ```powershell
   git clone https://github.com/lharries/whatsapp-mcp.git
   ```
4. Wait for it to finish (you will see it download some files).

The project is now in a folder called `whatsapp-mcp` inside your user folder (for example `C:\Users\YourName\whatsapp-mcp`).

> **Write this down - you will need it in Step 9:** Type these two commands, one at a time, and copy the full line the second one prints:
>
> ```powershell
> cd $HOME\whatsapp-mcp\whatsapp-mcp-server
> (Get-Location).Path
> ```
>
> It will print something like `C:\Users\YourName\whatsapp-mcp\whatsapp-mcp-server`. Save this text in Notepad next to the `uv` location from Step 4.

---

## Step 8: Start the WhatsApp bridge and scan the QR code

**What this does:** This starts the program that connects to your WhatsApp account. You will scan a QR code with your phone, just like WhatsApp Web.

1. Open a **new** PowerShell window.
2. Move into the bridge folder by typing this and pressing Enter:

   ```powershell
   cd $HOME\whatsapp-mcp\whatsapp-bridge
   ```
3. Turn on the setting Go needs on Windows by typing this and pressing Enter:

   ```powershell
   go env -w CGO_ENABLED=1
   ```
4. Now start the bridge by typing this and pressing Enter:

   ```powershell
   go run main.go
   ```
5. **The first time, this takes a few minutes** while Go downloads and builds everything. Be patient. Eventually a **QR code** made of black-and-white squares will appear in the window.
6. **Scan the QR code with your phone:**

   - Open **WhatsApp** on your phone.
   - Tap the **three dots** (Android) or **Settings** (iPhone).
   - Tap **Linked Devices**.
   - Tap **Link a Device**.
   - Point your phone's camera at the QR code in the PowerShell window.
7. Once it connects, the window will show messages about syncing your chats. **This can take several minutes** if you have a lot of messages. That is normal.

> **VERY IMPORTANT: Leave this PowerShell window open.** As long as it is open and running, your WhatsApp is connected. If you close it, the connection stops. You will start it again the same way next time (see [Everyday use](#everyday-use-how-to-start-it-again-next-time)).

> **Note:** After about 20 days, WhatsApp may ask you to scan the QR code again. This is normal - just repeat this step.

---

## Step 9: Connect the project to Claude

**What this does:** This tells Claude how to find and use the WhatsApp connector. You will edit one settings file.

### 9a. Find the two pieces of text you saved

From the earlier steps, you should have saved in Notepad:

- **The `uv` location** (from Step 4), e.g. `C:\Users\YourName\.local\bin\uv.exe`
- **The server folder location** (from Step 7), e.g. `C:\Users\YourName\whatsapp-mcp\whatsapp-mcp-server`

If you did not save them, go back and run those commands again to get them.

### 9b. Open the Claude settings file

The easiest way to open the correct settings file:

1. Open a **new** PowerShell window.
2. Copy and paste this command and press Enter. It creates the settings file (if it does not exist) and opens it in Notepad:

   ```powershell
   $dir = "$env:APPDATA\Claude"; if (!(Test-Path $dir)) { New-Item -ItemType Directory -Path $dir | Out-Null }; $file = "$dir\claude_desktop_config.json"; if (!(Test-Path $file)) { "{}" | Out-File -Encoding utf8 $file }; notepad $file
   ```
3. Notepad will open the file `claude_desktop_config.json`. It might be empty or just contain `{}`.

### 9c. Paste in the settings

1. **Delete everything** currently in the Notepad file (select all with Ctrl+A, then Delete).
2. Copy the text below and paste it in:

   ```json
   {
     "mcpServers": {
       "whatsapp": {
         "command": "PASTE_UV_LOCATION_HERE",
         "args": [
           "--directory",
           "PASTE_SERVER_FOLDER_HERE",
           "run",
           "main.py"
         ]
       }
     }
   }
   ```
3. Now replace the two placeholders with your saved text:

   - Replace `PASTE_UV_LOCATION_HERE` with your **uv location** from Step 4.
   - Replace `PASTE_SERVER_FOLDER_HERE` with your **server folder location** from Step 7.
4. **IMPORTANT for Windows: you must double every backslash (`\`) in your paths.** In this kind of file, a single `\` has a special meaning, so you must type two. For example:

   - Wrong: `C:\Users\YourName\.local\bin\uv.exe`
   - Right: `C:\\Users\\YourName\\.local\\bin\\uv.exe`

   So after you paste your paths, go through and change every single `\` into `\\`.

   Your finished file should look **something like** this (with your own username):

   ```json
   {
     "mcpServers": {
       "whatsapp": {
         "command": "C:\\Users\\YourName\\.local\\bin\\uv.exe",
         "args": [
           "--directory",
           "C:\\Users\\YourName\\whatsapp-mcp\\whatsapp-mcp-server",
           "run",
           "main.py"
         ]
       }
     }
   }
   ```
5. Save the file: click **File → Save** (or press Ctrl+S). Then close Notepad.

> **Common mistake:** Make sure the file is saved as `claude_desktop_config.json` and **not** `claude_desktop_config.json.txt`. Because you opened it with the command above, it should already have the correct name - just use **Save**, not "Save As."

---

## Step 10: Restart Claude and test it

1. Make sure the **WhatsApp bridge window from Step 8 is still open and running.** (If you closed it, redo Step 8 first.)
2. **Fully quit Claude Desktop** if it is open: right-click the Claude icon near the clock (bottom-right), choose **Quit**. If you are not sure, restarting your normal way is fine too.
3. **Open Claude Desktop again.**
4. Look for a **tools / plug icon** (often a small hammer or slider icon) near the message box. Click it - you should see **whatsapp** listed as an available tool.
5. **Test it!** Type a message to Claude like:

   > *"Search my WhatsApp contacts for someone named [a friend's name]."*
   >

   or

   > *"What are my most recent WhatsApp chats?"*
   >
6. Claude will ask for permission to use the WhatsApp tool the first time. Approve it, and you should see results from your real WhatsApp.

**Congratulations - you are done!** 🎉

---

## Everyday use: how to start it again next time

You do **not** need to reinstall anything. Each time you want to use WhatsApp with Claude:

1. **Start the WhatsApp bridge:**
   - Open PowerShell.
   - Type:

     ```powershell
     cd $HOME\whatsapp-mcp\whatsapp-bridge
     ```
   - Then type:

     ```powershell
     go run main.go
     ```
   - Leave this window open. (You usually will **not** need to scan the QR code again, unless it has been about 20 days.)
2. **Open Claude Desktop.** It will automatically connect to the bridge.

That is it. When you are finished for the day, you can close the PowerShell window to disconnect.

---

## Troubleshooting

### "go run main.go" gives an error mentioning CGO or go-sqlite3

The full error looks like:

> `Binary was compiled with 'CGO_ENABLED=0', go-sqlite3 requires cgo to work.`

This means the C compiler is not set up. Go back and carefully redo [Step 5](#step-5-install-a-c-compiler-msys2), especially the PATH part (5c), and make sure you ran `go env -w CGO_ENABLED=1` in Step 8. Remember to open a **fresh** PowerShell window after changing PATH.

### The QR code does not appear or looks broken

- Make the PowerShell window **bigger** (drag its corner) so the whole QR code fits.
- Press the up arrow and Enter to run `go run main.go` again.
- Try a different terminal if needed, but PowerShell usually works fine when the window is large enough.

### Claude does not show WhatsApp as a tool

- Make sure the **bridge window (Step 8) is running**.
- Double-check your settings file from [Step 9](#step-9-connect-the-project-to-claude): every `\` must be doubled to `\\`, and the paths must be exactly correct.
- Make sure you **fully quit and reopened** Claude Desktop (not just closed the window).
- Make sure the file is named exactly `claude_desktop_config.json` with no extra `.txt` at the end.

### "uv is not recognized" or Claude reports it cannot find the command

- Confirm the `uv` location in your settings file is correct. Run `(Get-Command uv).Source` in PowerShell again to get the exact path, and make sure it is in the file with doubled backslashes.

### No messages are loading

After you first connect, it can take several minutes (sometimes longer if you have many chats) for your message history to sync. Give it time and try again.

### My WhatsApp messages seem out of sync

Close the bridge window. Then delete the two database files and start over:

1. In PowerShell, type:

   ```powershell
   Remove-Item $HOME\whatsapp-mcp\whatsapp-bridge\store\messages.db, $HOME\whatsapp-mcp\whatsapp-bridge\store\whatsapp.db
   ```
2. Then redo [Step 8](#step-8-start-the-whatsapp-bridge-and-scan-the-qr-code) (you will scan the QR code again).

### "Device Limit Reached" when scanning

WhatsApp limits how many devices can be linked. On your phone, go to **WhatsApp → Linked Devices**, remove one you no longer use, then try scanning again.

---

## Optional: FFmpeg for voice messages

You only need this if you want Claude to send **playable voice messages**. You can skip it otherwise - regular audio files can still be sent as file attachments without it.

1. Go to **https://www.gyan.dev/ffmpeg/builds/** in your browser.
2. Under "release builds," download the file named **`ffmpeg-release-essentials.zip`**.
3. Right-click the downloaded zip and choose **Extract All**. Extract it to a simple location, for example `C:\ffmpeg`.
4. Inside, find the `bin` folder (e.g. `C:\ffmpeg\ffmpeg-x.x-essentials_build\bin`).
5. Add that `bin` folder to your PATH, using the **same method as [Step 5c](#5c-tell-windows-where-to-find-the-compiler-add-it-to-path)** (Environment Variables → edit Path → New → paste the folder location).
6. Open a **new** PowerShell window and check it worked:

   ```powershell
   ffmpeg -version
   ```

   You should see version information printed.

---

### That is everything.

If something does not work, re-read the step carefully - the most common issues are (1) forgetting to open a **new** PowerShell window after installing something, (2) forgetting to double the backslashes (`\\`) in the settings file, and (3) closing the bridge window from Step 8. Take your time and you will get there.
