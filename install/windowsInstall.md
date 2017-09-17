* TOC
{:toc}

# Important

If you are running a University administered machine, you will hopefully have pre-configured your machine.
The following installations will only be able to be performed if you have Administration privileges on your computer.
If you have not pre-configured your machine, we will provide you with an USB containing a live install of Ubuntu for you to use.

# Windows 10 (64-bit)

Your PC must be running a 64-bit version of Windows 10 Anniversary Update or later (build 1607+).

To find your PC's architecture and Windows build number, open
`Settings > System > About`

Look for the OS Build and System Type fields.

![](https://i-msdn.sec.s-msft.com/en-us/commandline/wsl/media/system.png)

If you have a compatible system, you are able to install the additional Operating System, known as Ubuntu.
This is a Linux operating system which usually runs independently of Windows, however this version has recently been made available which runs just like a normal app or program within Windows.
To install this, please follow this link (https://www.microsoft.com/en-au/store/p/ubuntu/9nblggh4msv6?rtc=1) and install the app.
Unfortunately you'll have to join up to the Windows Insider program for some ridiculous reason.

After installation, this will open a terminal running bash whenever you open Ubuntu.
If you are having troubles, call an instructor over.

# Other Versions of Windows

If you are running Windows 7 or 8, you can install `git bash` by going to the following site: https://git-for-windows.github.io/
Please install using all default settings, so just enter OK when asked any confusing questions.

After installation, you will have to install the additional tool called `wget`.
To perform this installation

1. download the `v1.19.1` 32 or 62-bit file ending in `.exe`
2. Move this file to `C:\Program Files\Git\mingw64\bin\`
3. If you downloaded the file `wget64.exe`, please rename the file as `wget.exe`

Once you have completed this installation, open Git Bash and enter the following in the terminal which opens

```
wget --version
```

If you receive an error message use the post-it notes to ask for help from an instructor
