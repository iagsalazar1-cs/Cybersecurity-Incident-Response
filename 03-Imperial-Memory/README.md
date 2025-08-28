# Imperial Memory - TDX Arena Certification Report

### Executive Summary

The primary goal of this investigation was to analyze a memory dump and a `7-Zip` archive with an encrypted file to uncover a hidden secret. This involved inspecting the memory dump for clues, with a focus on process memory, and then identifying and extracting the password for the encrypted file within the `7-zip` archive. Following by analyzing the decrypted file and discovering a secret file hidden inside. The investigation concluded with the hashing of contents of the secret file, as its `MD5` hash served as the flag for the challenge.

### Findings and Analysis

| Finding | Finding Details | Description |
| :--- | :--- | :--- |
| **User Process** | `powershell.exe` (PID `5496`) | Running process found using `Volatility` on the memory and listing the running processes during its creation. |
| **Password** | `gift.7z` archive passkey `G6Vmc$Qd5cpM8ee#Ca=x&A3` | Found after running `strings` on the `powershell.exe` dump file and observing the output. |
| **File** | `secrets.txt` | Hidden file within the `suspicious.docx` document. Extracted using the `unzip` utility on the `suspicious.docx` file. |
| **Hash** | MD5 `0f235385d25ade312a2d151a2cc43865` | Flag required to complete the challenge, generated using `md5sum` to hash the contents of the `secrets.txt` file. |

### Methodology

#### Tools and Technologies Used

* **Volatility Framework 2.6.1 (`vol.py`):** Open-source memory forensics framework used for analyzing the `Emperor.vmem` memory dump with specific plugins.
* **PeaZip:** Archiving utility used for initial inspection of the `gift.7z` archive.
* `7-Zip`: Command-line utility used for extracting the encrypted `gift.7z` archive.
* `strings`: Command-line utility for extracting human-readable character sequences from binary files.
* `unzip`: Command-line utility for extracting files from `ZIP` archives (used for `.docx` files).
* `md5sum`: Command-line utility for computing and checking `MD5` hashes.
* **LibreOffice Writer:** A word processing application used to open and view the `suspicious.docx` file.

#### Investigation Process

1.  **Initial Files on Desktop:** Inspected the files on the desktop and identified `Emperor.vmem` as the memory dump and `gift.7z` as the archive with the encrypted file.
   
    ![Figure 1 - Initial File Identification](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/03-Imperial-Memory/images/Figure01_Initial_File_Identification.png)

2.  **Inspecting the `gift.7z` Archive:** Verified the contents of the archive using the **PeaZip** utility, and observed that it contained the encrypted `suspicious.docx` file.
   
    ![Figure 2 - Inspecting the gift.7z Archive](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/03-Imperial-Memory/images/Figure02_Inspecting_the_gift_7z_Archive.png)

3.  **Checking Volatility Version:** Confirmed the **Volatility Framework** version installed in the lab environment (version `2.6.1`).
   
    ![Figure 3 - Checking Volatility Version](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/03-Imperial-Memory/images/Figure03_Checking_Volatility_Version.png)

4.  **Identifying Memory Dump Profile:** Ran `vol.py` on `Emperor.vmem` to identify the suggested profile `Win10x64_15063`, which was then instantiated.
   
    ![Figure 4 - Identifying Memory Dump Profile](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/03-Imperial-Memory/images/Figure04_Identifying_Memory_Dump_Profile.png)

5.  **Listing Running Processes:** Ran `vol.py` on `Emperor.vmem` using its profile and observed the list of running processes, noting `powershell.exe` (PID `5496`).
   
    ![Figure 5 - Listing Running Processes](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/03-Imperial-Memory/images/Figure05_Listing_Running_Processes.png)
    
    ![Figure 6 - powershell.exe Process](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/03-Imperial-Memory/images/Figure06_powershell_exe_Process.png)

6.  **Dumping PowerShell Process Memory:** Ran `vol.py` on `Emperor.vmem` to extract the memory data of the `powershell.exe` process and verified the dump file was created.
  
    ![Figure 7 - Dumping PowerShell Process Memory](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/03-Imperial-Memory/images/Figure07_Dumping_PowerShell_Process_Memory.png)

7.  **String Analysis of PowerShell Memory Dump:** Ran `strings` on the `5496.dmp` file, observing the output included the `7-Zip` command and password used to create the `gift.7z` archive.

    ![Figure 8 - String Analysis of PowerShell Memory Dump](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/03-Imperial-Memory/images/Figure08_String_Analysis_of_PowerShell_Memory_Dump.png)

8.  **Extracting the `gift.7z` Archive Contents:** Confirmed the availability of the `7-zip` command-line utility and ran `7z` to extract the contents of the `gift.7z` archive into the `extracted_gift` directory, observing a successful command execution.
   
    ![Figure 9 - 7-zip Utility Availability](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/03-Imperial-Memory/images/Figure09_7zip_Utility_Availability.png)
    
    ![Figure 10 - Extracting the gift.7z Archive Contents](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/03-Imperial-Memory/images/Figure10_Extracting_gift_7z_Archive.png)

9.  **Verifying Extraction Contents:** Listed the contents of the `extracted_gift` directory, confirming the presence of the `suspicious.docx` file.

    ![Figure 11 - Verifying Extraction Contents](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/03-Imperial-Memory/images/Figure11_Verifying_Extraction_Contents.png)

10. **Opening the DOCX File:** Opened `suspicious.docx` in **LibreOffice Writer** and observed the content: "I am empty!".

    ![Figure 12 - DOCX File Contents](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/03-Imperial-Memory/images/Figure12_DOCX_File_Contents.png)

11. **String Analysis of DOCX:** Ran `strings` on the `suspicious.docx` file, observing the `strings` output and noting the line "secrets.txt".

    ![Figure 13 - String Analysis of DOCX](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/03-Imperial-Memory/images/Figure13_String_Analysis_DOCX.png)
    
    ![Figure 14 - secrets.txt Hidden File Discovery](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/03-Imperial-Memory/images/Figure14_secrets_txt_Hidden_File_Discovery.png)

12. **Unzipping the DOCX File:** Ran the `unzip` utility on the `suspicious.docx` file, saving the output into the `extracted_docx` directory. Listed the contents of `extracted_docx`, confirming the presence of `secrets.txt` file.
    
    ![Figure 15 - Unzipping the DOCX File](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/03-Imperial-Memory/images/Figure15_Unzipping_DOCX_File.png)

13. **Viewing the Content of `secrets.txt`:** Navigated to the `extracted_docx` directory and displayed the contents of the `secrets.txt` file.
    
    ![Figure 16 - Viewing the Content of secrets.txt](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/03-Imperial-Memory/images/Figure16_Viewing_secrets_txt_Content.png)

14. **Hashing `secrets.txt` and Identifying the Flag:** Ran `md5sum` on the `secrets.txt` file to get its contents hash which was the required flag to complete the challenge.
    
    ![Figure 17 - Hashing secrets.txt and Identifying the Flag](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/03-Imperial-Memory/images/Figure17_Hashing_secrets_txt_Flag.png)

### Recommendations

1.  **Systematic Approach:** Always follow a structured methodology to ensure no stone is left unturned. Start broad and then narrow down.
2.  **Leverage Multiple Tools:** Don't rely on a single tool. If one doesn't yield results, try others.
3.  **Understand File Formats:** Knowledge of common file structures can reveal hidden data.
4.  **Don't Dismiss "Empty" Clues:** Investigate seemingly benign files for hidden content.
5.  **String Analysis is Powerful:** The `strings` utility can be very effective in revealing text-based secrets.
6.  **Document Everything:** Keep a detailed record of commands and findings.

### Appendix A: Detailed Findings

#### Forensic-related Findings

| Item | Details | Description |
| :--- | :--- | :--- |
| **User Process** | `powershell.exe` (PID `5496`) | Computer program that was being executed at the time of the `Emperor.vmem` memory creation. |
| **Memory Dump** | `5496.dmp` | Extracted memory data of the `powershell.exe` process from the `Emperor.vmem` image. |

#### Credential Findings

| Item | Details | Description |
| :--- | :--- | :--- |
| **Password** | `G6Vmc$Qd5cpM8ee#Ca=x&A3` | Found using `strings` on the `5496.dmp` file, observing the output included the passkey used to create the `gift.7z` archive. |

#### File-related Findings

| File | Contents | MD5 Hash |
| :--- | :--- | :--- |
| `secrets.txt` | Law of Individuality and Law of Probability explanatory text. The content’s hash served as the flag needed to complete the challenge. | `0f235385d25ade312a2d151a2cc43865` |

### Appendix B: Command History

| Syntax | Description |
| :--- | :--- |
| `ls` | Lists files and directories in the current directory (used throughout the investigation). |
| `vol.py -h` | Displays the **Volatility Framework 2** help menu, including its version and general options available before plugin execution. |
| `vol.py -f Emperor.vmem imageinfo` | Specifies the image file (`-f Emperor.vmem`). `imageinfo` identifies basic memory image information, including the suggested `OS` profile for proper command execution. |
| `vol.py -f Emperor.vmem --profile=Win10x64_15063 pslist` | Displays a snapshot of running processes (`pslist`) at the time of the `Emperor.vmem` image acquisition, using the suggested profile. |
| `vol.py -f Emperor.vmem --profile=Win10x64_15063 memdump -p 5496 --dump-dir .` | Extracts (`memdump`) memory contents of PID `5496` from the image to the current directory (`--dump-dir .`). |
| `strings 5496.dmp | grep -i "7z\|password\|gift.7z\|suspicious.docx"` | Prints strings from `5496.dmp`, then filters with `grep -i` for "7z|password|gift.7z|suspicious.docx". |
| `which 7z` | Tells you where the `7-zip` program is installed on your system. |
| `7z x gift.7z -p 'G6Vmc$Qd5cpM8ee#Ca=x&A3' -o./extracted_gift` | Extracts (`x`) all files from `gift.7z` using password (`-p`) `'G6Vmc$Qd5cpM8ee#Ca=x&A3'` and outputs (`-o`) to `./extracted_gift`. |
| `strings extracted_gift/suspicious.docx` | Uses `strings` to display printable characters from `suspicious.docx` in the `extracted_gift` directory. |
| `unzip extracted_gift/suspicious.docx -d extracted_docx` | Used to extract the contents of the `suspicious.docx` file, treating it as a `zip` archive, into a new directory named `extracted_docx`. |
| `cd extracted_docx/` | Changes the current directory to the specified path. |
| `cat secrets.txt` | Displays the content of the `secrets.txt` file directly in the terminal. |
| `md5sum secrets.txt` | Used to calculate and display the `MD5` checksum (or hash) of the `secrets.txt` file. |
