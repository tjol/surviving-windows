# Surviving Anaconda & pip on Windows

Since WinPython inexplicably blew up in my face some months ago (SciPy lost a DLL or something)
and I couldn't for the life of me figure out how to fix it), I've been using Anaconda
for my scientific Python needs on Windows. It appears to work. More or less.

## conda

**Conda** is Anaconda's package manager. Why they have something other than pip to manage
Python packages is beyond me. Apparently, it is recommended to use it to install packages
when possible, but *conda does not have all PyPI packages*.

    conda install pandas

and so on. To update all packages, `conda` has a handy command:

    conda update --all

**THIS WILL PROBABLY DOWNGRADE SOME `pip`-INSTALLED PACKAGES, INCLUDING pip ITSELF.**

## pip + PowerShell

Pip doesn't ([yet][1]) have an `upgrade-all` command, so you have to let your shell help you.
On Linux, I might do something like (untested)

    pip list --outdated | gawk '// { print $1; }' | xargs pip install --upgrade

On Windows PowerShell, this works:

    $outdated_packages = pip list --outdated | foreach { $_.split()[0] }
    pip install --upgrade $outdated_packages


[1]: https://github.com/pypa/pip/issues/59