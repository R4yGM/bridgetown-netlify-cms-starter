---
layout: post
title: Stegbrute — Fast Steganography brute-force tool
date: 2020-07-23T11:09:12.271Z
---

stegbrute that stands for steganography and bruteforce is a fast and simple steganography brute-force tool written in rust


# What is this?
This program tries to extract hidden files using steganography inside another file or files, and to extract these files you need a password so by using bruteforce you can try as many passwords as you want very quickly, and this program automates all of this and makes it more faster using threads.
How to use it


First of all you need to download the program, you can download it in 4 ways they are all documentated in the Github repository or just download them from the releases section.


now that you have stegbrute installed run it with -h or — help option, this is what you should see
```
============================================================
     ____  _             ____             _
    / ___|| |_ ___  __ _| __ ) _ __ _   _| |_ ___
    \___ \| __/ _ \/ _` |  _ \| '__| | | | __/ _ \
     ___) | ||  __/ (_| | |_) | |  | |_| | ||  __/
    |____/ \__\___|\__, |____/|_|   \__,_|\__\___|
                   |___/

StegBrute v0.1.1 - By R4yan
https://github.com/R4yGM/StegBrute

StegBrute 0.1.1
R4yan <yessou.rayan@gmail.com>
Steganography bruteforce tool

USAGE:
    stegbrute [FLAGS] [OPTIONS] --file-name <file-name> --wordlist <wordlist>

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information
    -v, --verbose    shows every try the program does

OPTIONS:
    -x, --extract-file <extract-file>    the file name path where you want to write the results [default:
                                         stegbrute_results.txt]
    -f, --file-name <file-name>          the file name path you want to crack
    -t, --threads <threads>              number of threads to bruteforce the file [default: 3]
    -w, --wordlist <wordlist>            path of the wordlist
```
now you are ready to crack any image!,

example on how to use stegbrute
```
stegbrute -f image.jpg -w wordlist.txt -x results.txt
```

where -f is the file you want to crack, -w is the wordlist (a big list of passwords), -x is where you want to save your results, output :

```
============================================================
     ____  _             ____             _
    / ___|| |_ ___  __ _| __ ) _ __ _   _| |_ ___
    \___ \| __/ _ \/ _` |  _ \| '__| | | | __/ _ \
     ___) | ||  __/ (_| | |_) | |  | |_| | ||  __/
    |____/ \__\___|\__, |____/|_|   \__,_|\__\___|
                   |___/StegBrute v0.1.1 - By R4yan
https://github.com/R4yGM/StegBrute
Bruteforcing the file 'image.jpg' with the wordlist 'wordlist.txt' using 3 threads
(thread-0) Failed to crack the file, finished the passwords 178.02ms
(thread-2) Failed to crack the file, finished the passwords 284.84ms
password try: cool123 - Success
File extracted!
Password: cool123
Results written in: results.txt
Tried passwords : 62
Successfully cracked in 499.64ms
============================================================
```

it’s pretty fast, stegbrute tried 62 passords in half a second! as you can see it also shows the status of every thread
Comparision


I saw around also stegcracker wich is also a great program so i tried to compare it to stegbrute, and these are the results:

```
stegcracker :

# time stegcracker image.jpg wordlist.txt | grep -E "real|user|sys"
real    0m1.754s
user    0m0.420s
sys     0m0.362s
```

```
stegbrute :

# time stegbrute -f image.jpg -w wordlist.txt | grep -E "real|user|sys"
real    0m0.553s
user    0m0.366s
sys     0m0.460s
```

this says that stegbrute is more faster than stegcracker (i also tried to compare using other large wordlists)



# Benchmark

stegbrute benchmark on different wordlists using 3 threads

| Wordlist passwords   | Time  |  
|---|---|
| 100   | 841.12ms  |  
| 1000  | 8.57s  |   
| 10000 | 77.79s |
| 100000 | 775.93s  |  

these values of course can vary on due to your machine power