## Solution Guide:  Restoring Data with `tar`

The goal of this activity was to locate the correct archive to restore the patient directory and files. 

Completing the activity required the following steps:

- List the contents of the archive to determine what it contains.

- Using the `grep` command to search the archive for two patient names.

- Creating a directory to restore the patient directories and files. 

- Extracting only the patient directory and files to the directory for review before restoring the files to the E-Prescription Treatment system.

- Validating that the archive does not contain errors, using the **`-R`** option.

1. First, we move to and list the contents of the `~/Documents/epscript/backup` directory, specifically the `20190814epscript.tar` file. This file is used to search for the patient information.

- Run `cd ~/Documents/epscript/backup`

- Run `ls -l`

    Your output should look like:

    ```
    -rw--r-r--  1   sysadmin  sysadmin  4341760    Jul 17   01:45   20190814epscript.tar
    ```

2. Next, list the contents of the patient overview, the patient directory, and files in the archive, and pipe the output to the screen.

- Run: `tar tvvf 20190814epscript.tar | less`

3. The clinical technician supplied names for the patient files that were missing. After decompressing the file, use `grep` to find a patient file in the archive. 

- Run: 

    - `tar xvf 20190814epscript.tar` to e**x**tact the archive, verbosely.

For `Mark Lopez`, we'll do the following:

- Let's look in the `doctors` folder:

    - `cd epscript/doctors`  

    - `cat * | grep "Mark,Lopez"`  

      - No results

- We'll check the `treatments` folder:

    - `cd ../treatments`  

    - `cat * | grep "Mark,Lopez"`
    
      - No results again!

- Now we'll check the `patients` folder:

    - `cd ../patients`  

    - `cat * | grep "Mark,Lopez"`  

- Two results in the `patients` directory for Mark Lopez!

      ```
      809,Mark,Lopez,male,O,31,577.511.1054x23935,jeffrey93@jones.net,model,"673 Schultz Spur Apt. 244
      809,Mark,Lopez,male,O,31,577.511.1054x23935,jeffrey93@jones.net,model,"673 Schultz Spur Apt. 244
      ```

For `Megan Patel`, we'll do the following:

- Let's look in the `doctors` folder:

    - `cd ../doctors`  

    - `cat * | grep "Megan,Patel"`  

      - No results

- We'll check the `treatments` folder:

    - `cd ../treatments`  

    - `cat * | grep "Megan,Patel"`
    
      - No results again!

- Now we'll check the `patients` folder:

    - `cd ../patients`  

    - `cat * | grep "Megan,Patel"`  

- Two results in the `patients` directory for Megan Patel!

      ```
      699,Megan,Patel,female,AB,43,001-684-391-7956,pjohnson@gmail.com,develop,"53082 Lopez IslandChavezchester, CT 11475"
      699,Megan,Patel,female,AB,43,001-684-391-7956,pjohnson@gmail.com,develop,"53082 Lopez Island
      ```

- **BONUS** 

  - Using a single command for `Mark Lopez`:

    - `grep -R "Mark,Lopez" epscript/`

  - Using a single command for `Megan Patel`:

    - `grep -R "Megan,Patel" epscript/`

- Note that a hacker can steal these and other millions of medical records to [sell on the web](<https://www.cbsnews.com/news/hackers-steal-medical-records-sell-them-on-dark-web/>).

4. Next we’ll extract the patient files from the archive, test for errors, and save them in the `patient_search` directory. 

The general syntax of the command is: 

- `tar [options][archive_name][option][option][save_directory][objects_to_archive]`

In the `~/Documents/epscript/backup` directory, make a new directory called `patient_search`.

- Run  `mkdir patient_search`

- Run  `tar xvvf 20190814epscript.tar -R -C patient_search/ epscript/patients`

    - Syntax breakdown:

        - `xvvf` are the options.
        - `20190814epscript.tar` is the archive name.
        - The `-R` option tells `tar`to print error messages for any errors with the block number in the archive file when extracting files.  
          - We should always use the `R` option while extracting data to verify that there are no file errors.
        - The `-C` option is followed by the directory to save the patient directory and files. 
        - `patient_search/` is the directory where files are saved.
        - `epscript/patients` is the directory that contains the patient files.  We are extracting files from this directory in the `tar` archive.

    At the end of the output, you should see:

    ```
    block 1576: ** Block of NULs **
    ```

5. Lastly, list the contents of the `patient_search` directory to check that the patient directory and files were extracted there.  

- Run  `ls -l patient_search/epscript/patients`

    - Your output should look similar to this:

    ```
    total 276
    -rw-r--r-- 1 sysadmin sysadmin  12193 Aug 14  2019 patients.10.csv
    -rw-r--r-- 1 sysadmin sysadmin  12334 Aug 14  2019 patients.1.csv
    -rw-r--r-- 1 sysadmin sysadmin  12534 Aug 14  2019 patients.2.csv
    -rw-r--r-- 1 sysadmin sysadmin  12398 Aug 14  2019 patients.3.csv
    ...
    ...
    ```

--- 

© 2020 Trilogy Education Services, a 2U, Inc. brand.  All Rights Reserved.
