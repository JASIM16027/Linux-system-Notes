# Linux-system-Notes

This script creates and configures a swap file on your system. Here’s a step-by-step explanation of each part and instructions for running it:

```sh

#!/bin/bash

# Size of the swap file in GB
SWAP_SIZE_GB=6

# Swap file location
SWAPFILE=/swapfile

# Turn off the swap
sudo swapoff -a

# Remove the existing swap file if it exists
if [ -f "$SWAPFILE" ]; then
    sudo rm -f $SWAPFILE
    echo "Removed existing swap file."
fi

# Create a new swap file with the specified size
sudo fallocate -l ${SWAP_SIZE_GB}G $SWAPFILE

# Set the correct permissions for the swap file
sudo chmod 600 $SWAPFILE

# Set up the swap area
sudo mkswap $SWAPFILE

# Enable the swap file
sudo swapon $SWAPFILE

# Add the swap file to /etc/fstab for persistence across reboots
if ! grep -q "$SWAPFILE" /etc/fstab; then
    echo "$SWAPFILE none swap sw 0 0" | sudo tee -a /etc/fstab
    echo "Swap file added to /etc/fstab."
fi

# Show the new swap status
sudo swapon --show
free -h

echo "Swap memory size increased to ${SWAP_SIZE_GB}GB."

```

### What the Script Does

1. **Defines Swap Size and File Location**: Sets the size of the swap file to 6 GB and specifies its location (`/swapfile`).
   
2. **Turns Off Existing Swap**: Temporarily disables any active swap files.

3. **Removes Old Swap File**: Checks if a swap file exists at `/swapfile`, and removes it if found.

4. **Creates New Swap File**: Allocates space for a new swap file of the specified size (6 GB).

5. **Sets Permissions**: Restricts access to the swap file to ensure security.

6. **Sets Up the Swap Area**: Formats the swap file to make it usable as swap space.

7. **Activates the Swap File**: Enables the swap file so it’s immediately available for use.

8. **Ensures Persistence Across Reboots**: Adds the swap file to `/etc/fstab`, so it’s mounted automatically on reboot.

9. **Displays Swap Status**: Shows the new swap status and the total memory available.

### Running the Script

1. **Open a Terminal**: Press `Ctrl + Alt + T` to open a terminal in Ubuntu or any Linux distribution.

2. **Create the Script File**:
   - Use a text editor to create a new file, such as `swap_setup.sh`.
     ```bash
     nano swap_setup.sh
     ```
   - Paste the script into this file.

3. **Save and Close** the file in `nano` by pressing `Ctrl + X`, then `Y`, and `Enter`.

4. **Make the Script Executable**:
   - Run the following command to give the script execute permissions:
     ```bash
     chmod +x swap_setup.sh
     ```

5. **Run the Script with Root Privileges**:
   - Execute the script with `sudo` to ensure it has the necessary permissions:
     ```bash
     sudo ./swap_setup.sh
     ```

### After Running

Once the script finishes, you’ll see the swap status and total memory available. The swap file is now set to 6 GB, and it will be automatically enabled on reboot.
