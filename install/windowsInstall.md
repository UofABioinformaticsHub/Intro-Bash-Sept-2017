# Important

If you are running a University administered machine, you will hopefully have pre-configured your machine.
The following installations will only be able to be performed if you have Administration privileges on your computer.
If you have not pre-configured your machine, we will provide you with an USB containing a live install of Ubuntu for you to use.

## Installing Bash on Windows

If you are running Windows 10 and have already installed Ubuntu as an app, please use this in preference to the below.
Otherwise, please follow these instructions to install a working version of bash on your computer.

1. Install `git bash` by going to the following site: [https://git-for-windows.github.io/](https://git-for-windows.github.io/)
2. When presented with this screen, select the first option

![](https://blog.assembla.com/hs-fs/hub/365/file-2182891772-png/Blog/Git_on_windows_blog/Git_adjustPath.png?t=1505570223016)

- Accept defaults for all other options, especially the following:

![](https://blog.assembla.com/hs-fs/hub/365/file-2181997909-png/Blog/Git_on_windows_blog/Git_Configure_LineEndings.png?t=1505570223016)

After installation, you will need to install the additional tool called `wget`.
To perform this installation

1. Download the [32-bit](https://eternallybored.org/misc/wget/current/wget.exe) or [62-bit](https://eternallybored.org/misc/wget/current/wget64.exe) file ending in `.exe`
2. If you downloaded the file `wget64.exe`, please rename the file as `wget.exe`
3. Move this file to `C:\Program Files\Git\mingw64\bin\`

Once you have completed the installation, open Git Bash and enter the following in the terminal

```
wget --version
```

If you receive an error message **use the post-it notes** to ask for help from an instructor

## Installing Notepad++

Although we may not use this much today, an excellent editor for scripts on the Windows OS is *Notepad++*.
A key difference between Windows and Linux/OSX systems is the hidden characters used to denote `end-of-line` (EOL).
Most editors such as Wordpad, MS Word etc will change these without you knowing and scripts will not be able to be run if edited using these programs.
*Notepad++* does not do this and using this software will allow you to edit scripts conveniently without breaking them.

This software can be obtained from [here](https://notepad-plus-plus.org/download/v7.5.1.html)

[Home](../)
