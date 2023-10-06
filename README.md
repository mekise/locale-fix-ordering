# locale-fix-ordering

This is a guide to add a new locale file with little mods like hidden files first, underscored files second, and a case-insensitive alphabetical order.

⚠️ **If system updates break the ordering or the terminal, just rerun point 7 of the guide! You will need access to another terminal, like terminator, konsole, etc.**

⚠️ **For this guide, you will need superuser permissions.**

## Ordering
1. Hidden files (.)
2. Underscored (_)
3. Alphabet is case insensitive

*Ordering example:*
1. .jkl
2. _ghi
3. ABC
4. abc
5. DEF
6. def

## Guide
1. Ensure you have the locale files in `/usr/share/i18n/locales`. If the directory is empty, install the package `glibc-locale-source` e.g., in Fedora,
```console
sudo dnf install glibc-locale-source
```

2. Copy the files you want your mod to be based on. In my case, I will use `en_US` and make
a copy as `en_US_mks`. Do the same for the other files as in the example below,
```console
cd /usr/share/i18n/locales
sudo cp en_US en_US_mks
sudo cp iso14651_t1 iso14651_t1_mks
sudo cp iso14651_t1_common iso14651_t1_common_mks
```

3. Open the file `en_US_mks` and change the line `copy "iso14651_t1"` into `copy "iso14651_t1_mks"`

4. Open the file `iso14651_t1` and change the line
`copy "iso14651_t1_common"` into `copy "iso14651_t1_common_mks"`

5. Now, we change the entries in the iso common file. Open the file `iso14651_t1_common_mks` and change the line of the character `<U002E>` (full stop) as follows
```console
<U002E> <RES-1>;IGNORE;IGNORE;<U002E> % FULL STOP
```

6. Change the line of the character `<U005F>` (underscore/low line) as follows
```console
<U005F> <BLK>;IGNORE;IGNORE;<U005F> % LOW LINE
```

7. Compile the new locale
```console
sudo localedef -c -i en_US_mks -f UTF-8 en_US_mks.UTF-8
```

8. Add/change the LANG and LC_COLLATE lines in the `/etc/locale.conf` file
```console
LANG=en_US_mks.utf8
LC_COLLATE=en_US_mks.utf8
```

9. Finally, reboot the system (depending on the system, logging off probably will not be enough).
