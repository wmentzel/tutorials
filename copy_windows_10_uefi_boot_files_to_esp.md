1. Boot windows from USB stick

1. Go to Troubleshoot > Command Prompt

1. List all discs

        diskpart

1.  Select the disc with windows (and the EFI partition ) on it

        select disk 0

1. List partitions of selected disk

        list partition

1. Select "SYSTEM" partition

        select partition 1

1. Assign a driver letter

        assign letter=S

1. **Optional:** Make sure it is the correct partition

        dir S:

1. **Optional:** Make sure windows is mounted at C:

        dir C:

1. Copy over the UEFI boot files

        bcdboot C:\Windows /s S: /f UEFI

1. Done
