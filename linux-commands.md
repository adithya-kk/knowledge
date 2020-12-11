# Linux Commands Quick Guide

## [GREP](grep.md)
- grep -i 'pattern' *  

## Find file
- find -name "ssh*"  
- find -name 'Jenkins*' -print  
    - prints the location  

## How to check if port is in use in
sudo lsof -i -P -n | grep LISTEN   

## Find file and execute grep
find -name "ssh*" -exec grep '#ssh' {} \;  

## adb pull
adb pull /storage/storageName/Foldername /destination/Foldername/  

## rsync
- The -a option is a combination flag. It stands for “archive” and syncs recursively and preserves symbolic links, special and device files, modification times, group, owner, and permissions. It is more commonly used than -r and is usually what you want to use.  
- -v is for verbose  
- -n is for dry run(double checking uses)  
- -z is for compression  
- -P is for progress  
- --delete is for deleting things present in dest but not in source  
- --exclude=pattern_to_exclude --include=pattern_to_include  
- --backup --backup-dir=/path/to/backups  
    - rsync -anvPb --backup=/backupDir/ dir1/ dir2/  
    - rsync -avPb --backup-dir=../BackupDir/ --suffix=~ --delete /src/ /dst/  
- Use --fuzzy to look in the same directory as the destination file for either a file  that  has  an identical size and modified-time, or a similarly-named file.  
    - rsync -avzPb --backup-dir=../BackupDir/ --fuzzy --delay-updates --delete-delay /src/ /dst/  
- [f in rsync logs](https://stackoverflow.com/questions/4493525/what-does-f-mean-in-rsync-logs)

## lspci
- Display info about pci bus and all devices connected to it  
    - sudo lspci -v  

## df
- Shows mount point and space utilized or free space available.  

## lsattr  
- Shows the attributes of the file. Use chattr to change attributes.  

## chattr  
- Change attribute of a given file.  
- For immutabiltiy:  
	- chattr +i filename   
- For appending:  
	- chattr +a filename  
- For removing attribute use -  
	- chattr -i filename  

## PDF to JPG  
(pdf to jpg)[https://stackoverflow.com/questions/43085889/how-to-convert-a-pdf-into-jpg-with-command-line-in-linux]
1.[Produces ~1MB-sized files per pg] Output in .jpg format at 300 DPI:

     mkdir -p images && pdftoppm -jpeg -r 300 mypdf.pdf images/pg  

2.[Produces ~2MB-sized files per pg] Output in .jpg format at highest quality (least compression) and still at 300 DPI:

     mkdir -p images && pdftoppm -jpeg -jpegopt quality=100 -r 300 mypdf.pdf images/pg

3.If you need more resolution, you can try 600 DPI:

     mkdir -p images && pdftoppm -jpeg -r 600 mypdf.pdf images/pg

4....or 1200 DPI:

     mkdir -p images && pdftoppm -jpeg -r 1200 mypdf.pdf images/pg

## Rclone
rclone sync --transfers 5 --checkers 5 --contimeout 60s --timeout 300s --retries 3 --low-level-retries 10 --stats 1s --stats-file-name-length 0 --log-level INFO --log-file=rclone-log.txt "/src/dir/" "remote:dest_dir/"
