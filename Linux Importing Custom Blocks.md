# How to Load Custom Blocks into AugeLab Studio in Linux distros

## Summary
Summary of this tutorial is basically:

* Copy custom block file from Windows to Ubuntu
* Place it in a folder
* Modify `main.py` (using a command to import the custom block)
* Run your headless scenario.

### Prerequisites
- AugeLab Studio is already installed.
- You have a custom .py file that you want to load into AugeLab Studio.
- You will transfer the custom Python file from a Windows machine to an Ubuntu machine, where the Python script will be run.

## Step-by-Step Instructions
### Step 1: Locate the Custom Python File on Windows
On your Windows machine, open File Explorer.

- Locate the custom block file from(Replace <USERNAME> with your Windows username):
     ```C:\Users\<USERNAME>\AppData\Roaming\AugeLab Studio\marketplace\custom_blocks```
- Copy the xxx .py file from this location to your Ubuntu machine.

### Step 2: Transfer the Custom Python File to Ubuntu
On your Ubuntu machine, create a folder named custom_blocks in your project directory. For example:

- Create a folder by executing the command on the command window:
```bash
mkdir custom_blocks
```
- Transfer the custom .py file to this location. For `xxx.py` file:
```bash
mv xxx.py custom_blocks/
```

### Step 3: Use the Provided Python Script snippet
Your folder structure should look like this:

````markdown
~current_folder~/
    custom_blocks/
        my_custom_blocks.py
    main.py
````

You will be provided with a snippet in step-4 that will load your custom blocks.

### Step 4: Modify-Run the Python Script
Modify the example script provided in [full example](https://github.com/AugelabTech/AugeLab-Studio-Issues/blob/main/StudioAPI.md#full-example) and insert the snippet below.

- Copy this snippet:
````python
from studio import StudioScenario

s = StudioScenario()
dir_custom_block_py = "custom_blocks/" # you may modify this path depending on where you copy the file
s.load_custom_nodes(dir_custom_block_py)

... # 
````

- Insert into the script you are running. In your case, you should be using `main.py`.

````python
from studio import StudioScenario

## Importing custom blocks

scenario = StudioScenario(verification_code="<verification-code>")  # provide verification code from your account.augelab.com
dir_custom_block_py = "custom_blocks/" # you may modify this path depending on where you copy the file
s.load_custom_nodes(dir_custom_block_py)

## Importing custom blocks

import cv2

scenario.load_scenario(<path-to-model>)
args = (True, 3,)  # Define your arguments here, here is assumed that scenario takes two arguments
while True:
    res = scenario.run(args)
    print(res[1][0])  # we get the number from here

    cv2.imwrite('result.png', res[0][0], [cv2.IMWRITE_PNG_COMPRESSION, 0])  # save it with lossless compression
    if res[0]:
        break

... # 
````

- Follow [full example](https://github.com/AugelabTech/AugeLab-Studio-Issues/blob/main/StudioAPI.md#full-example) to run AugeLab Studio.

## Troubleshooting
- File Not Found: Ensure the path to the .py file is correct and the file exists in the custom_blocks folder.
- Permission Issues: Ensure you have the necessary read permissions for the directory and file.
