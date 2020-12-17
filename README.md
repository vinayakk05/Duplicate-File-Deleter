# Duplicate-File-Deleter
#Calculating Each File Check Sum and Deleting Duplicate Files From Folder.

import os
from sys import *
import hashlib

#Calculate Checksum
def hashfile(path,blocksize=1024):
    afile=open(path,'rb');
    hasher=hashlib.md5();
    buf=afile.read(blocksize);
    while len(buf)>0:
        hasher.update(buf);
        buf=afile.read(blocksize);
    afile.close();
    return hasher.hexdigest();
    
#Provides Files In Folder    
def findduplicates(path):
    flag = os.path.isabs(path);
    if flag == False:
        path = os.path.abspath(path);

    exist = os.path.isdir(path);
    dups={};
    if exist:
        for Foldername, Subfolder, Filename in os.walk(path):
            for Filen in Filename:
                path=os.path.join(Foldername,Filen);
                haf=hashfile(path);
                if haf in dups:
                    dups[haf].append(path);
                else:
                    dups[haf]=[path];
            print(" ");

        return dups;
    else:
        print("Invalid Input");

#Prints Duplicates And Delete It
def printduplicate(dict1):
    results=list(filter(lambda x:len(x)>1,dict1.values()))
    if len(results)>0:
        print("Duplicates Found as Follows ");
        icnt = 0;
        for result in results:
            for subre in result:
                icnt=icnt+1;
                if icnt>=2:
                    print("Duplicates",subre);
                    os.remove(subre);
            icnt=0;
    else:
        print("No Duplicates");



#Main Function Provide Folder Name Using Command Line
def main():
    print("A Script By Vinayak");
    print("Application name", argv[0]);

    if (len(argv) != 2):
        print("Enter valid input");
        exit()
    if (argv[1]) == '-h' or (argv[1]) == '-H':
        print("Script to watch folder");
        exit()
    if (argv[1]) == '-u' or (argv[1]) == '-U':
        print("Application Name is Directory watcher");
        exit()

    try:
        arr={}
        arr=findduplicates(argv[1]);
        print(arr)
        printduplicate(arr);

    except Exception:
        print("Exception Occured");


if __name__ == "__main__":
    main();

