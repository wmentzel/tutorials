1. Boot windows from USB stick

2. Go to Troubleshoot > Command Prompt

3. List all discs

        diskpart

4. Select the disc with windows (and the EFI partition ) on it

       select disk 0

5. List partitions of selected disk

        list partition

6. Select "SYSTEM" partition

        select partition 1

7. Assign a driver letter

        assign letter=S

8. **Optional:** Make sure it is the correct partition

        dir S:

9. **Optional:** Make sure windows is mounted at C:

        dir C:

10. Copy over the UEFI boot files

         bcdboot C:\Windows /s S: /f UEFI

11. Done
