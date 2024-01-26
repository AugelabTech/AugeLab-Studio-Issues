Here's the documentation for a `main.py` script, which utilizes the `StudioScenario` class. To execute your main loop, you must use the python (python3.9) provided with the distributed AugeLab Studio.

---

# Scripting Documentation

This script demonstrates how to use the `StudioScenario` class to load, run, and clean up a scenario.

## Basic Usage

### 1. Importing and Initializing
```python
from studio import StudioScenario

scenario = StudioScenario(verification_code="<verification-code>")
```
- Imports the `StudioScenario` class.
- Initializes a `StudioScenario` object with a provided verification code. Verification code can be acquired from [account.augelab.com](https://account.augelab.com)

### 2. Loading a Scenario
```python
scenario.load_scenario("<path_to.pmod file>")
```
- Loads a scenario from the specified file path (replace `"path to pmod"` with the actual file path).

### 3. Running the Scenario in a Loop
```python
while True:
    res = scenario.run((,))  # The first argument is always a tuple
    if res[0]:
        break
```
- Runs the scenario in a loop. The `scenario.run` method expects a tuple as its argument.
- The loop breaks when the first element of the returned tuple (`res[0]`) is truthy, indicating completion or a specific condition.

### 4. Cleanup
```python
scenario.cleanup()
```
- Cleans up the scenario, preparing for a clean exit or for loading a new scenario.

## Variations

### Variation 1: Providing Arguments to `run`
```python
args = (arg1, arg2, ...)  # Define your arguments here
while True:
    res = scenario.run(args)
    if res[0]:
        break
```
- This variation includes passing arguments to the `scenario.run` method. `arg1, arg2, ...` should be replaced with actual arguments expected by your scenario.

### Variation 2: Handling Results
```python
while True:
    res = scenario.run((,))
    if res[0]:
        print("Scenario completed with result:", res[1])
        break
```
- In this variation, the script prints out the results after the scenario completes. `res[1]` represents additional return values from the scenario.

### Variation 3: Loading Multiple Scenarios
```python
scenarios = ["path to scenario1", "path to scenario2", ...]

for scenario_path in scenarios:
    scenario.load_scenario(scenario_path)
    while True:
        res = scenario.run((,))
        if res[0]:
            break
    scenario.cleanup()
```
- This variation demonstrates how to load and run multiple scenarios sequentially.

### Variation 4: Show Image Results
```python
from studio import StudioScenario
scenario = StudioScenario(verification_code="<verification-code>")

import cv2  # provided with AugeLab Studio, import after initializing an instance of `StudioScenario`.
scenario.load_scenario(<path-to-model>)
while True:
    res = scenario.run((,))
    cv2.imshow('Result', res[0][0])  # assuming that first output is an image (numpy ndarray of a BGR uint8 image)
    key = cv2.waitKey(1) & 0xFF
    if key == ord("q"):
        break
```

Replace placeholders like `<verification-code>`, `"path to pmod"`, and `arg1, arg2, ...` with actual values relevant to your specific use case.

### Full Example

Save your scenario from AugeLab Studio:

![image](https://github.com/AugelabTech/AugeLab-Studio-Issues/assets/18147158/0560bd14-750e-47ba-9624-b771d73c053e)


Run this line from command line:
````cmd
<path to AugeLab Studio>\Python\python.exe main.py
````

> 💡If you are using another OS, the line below would suffice:
> ```bash
> python3 main.py
> ```

Contents of a full featured main.py:
```python

from studio import StudioScenario
scenario = StudioScenario(verification_code="<verification-code>")  # provide verification code from your account.augelab.com

import cv2

scenario.load_scenario(<path-to-model>)
args = (True, 3,)  # Define your arguments here, here is assumed that scenario takes two arguments
while True:
    res = scenario.run(args)
    print(res[1][0])  # we get the number from here

    cv2.imwrite('result.png', res[0][0], [cv2.IMWRITE_PNG_COMPRESSION, 0])  # save it with lossless compression
    if res[0]:
        break
```

## Installing Packages

Simply, run this command for command line:

````cmdline
<path to AugeLab Studio>\Python\python.exe -m pip install <package>
````

Be careful not to create any version conflicts with AugeLab Studio when installing new packages.

---

# StudioScenario API Documentation

## Overview
`StudioScenario` is a class designed to manage and manipulate scenarios in a node-based environment. It provides methods for loading, saving, manipulating, and running scenarios.

## Methods

### `load_custom_nodes(path: str) -> bool`
Loads custom nodes from a specified directory.
- **Parameters:**
  - `path`: The directory path where custom nodes are located.
- **Returns:** `True` if successful, `False` otherwise.

### `load_scenario(path: str) -> Optional[StudioScenario]`
Loads a scenario from a file.
- **Parameters:**
  - `path`: File path of the scenario to be loaded.
- **Returns:** The `StudioScenario` instance if successful, `None` otherwise.

### `load_scenario_raw(content: str) -> bool`
Loads a scenario from a raw string representation.
- **Parameters:**
  - `content`: Raw string content of the scenario.
- **Returns:** `True` if successful, `False` otherwise.

### `save_scenario(path: str) -> bool`
Saves the current scenario to a file.
- **Parameters:**
  - `path`: File path where the scenario will be saved.
- **Returns:** `True` if successful, `False` otherwise.

### `clear_scenario() -> bool`
Clears the current scenario.
- **Returns:** `True` if successful, `False` otherwise.

### `remove_block(studio_block: StudioBlock) -> bool`
Removes a specific block from the scenario.
- **Parameters:**
  - `studio_block`: The `StudioBlock` instance to be removed.
- **Returns:** `True` if successful, `False` otherwise.

### `add_block(*widget_params, op_code: Union[str, int] = "", full_name: str = "") -> Union[StudioBlock, None]`
Adds a new block to the scenario.
- **Parameters:**
  - `widget_params`: Parameters for the widget.
  - `op_code`: Operation code of the block.
  - `full_name`: Full name of the block.
- **Returns:** The `StudioBlock` instance if successful, `None` otherwise.

### `get_ports(input_op_title: str = 'Subsystem In', output_op_title: str = "Subsystem Out")`
Retrieves input and output ports of the scenario.
- **Parameters:**
  - `input_op_title`: The title of the input operation.
  - `output_op_title`: The title of the output operation.
- **Returns:** A dictionary with keys "inputs", "outputs", "outputs_image", and "outputs_non_image".

### `get_blocks(node_type: str = '', node_name: str = '', op_code: Union[int, str] = '', _first: bool = False) -> Tuple[StudioBlock, ...]`
Retrieves blocks from the scenario.
- **Parameters:**
  - `node_type`: Type of node.
  - `node_name`: Name of the node.
  - `op_code`: Operation code.
  - `_first`: If `True`, returns the first matching block.
- **Returns:** A tuple of `StudioBlock` instances.

### `get_block(block_type: str = '', block_name: str = '', op_code: Union[int, str] = '') -> Union[StudioBlock, None]`
Retrieves a single block from the scenario.
- **Parameters:** Same as `get_blocks`, but returns the first matching block or `None`.

### `get_name() -> str`
Retrieves the title of the current scenario.
- **Returns:** The title of the scenario as a string.

### `installStartEvent(event: Callable[[...], None])`
Installs a start event in the scenario.
- **Parameters:**
  - `event`: The event to be installed.

### `installEndEvent(event: Callable[[...], None])`
Installs an end event in the scenario.
- **Parameters:** Same as `installStartEvent`.

### `installStepStartEvent(event: Callable[[...], None])`
Installs a step start event in the scenario.
- **Parameters:** Same as `installStartEvent`.

### `installStepEndEvent(event: Callable[[...], None])`
Installs a step end event in the scenario.
- **Parameters:** Same as `installStartEvent`.

### `run(args: tuple[Any, ...]) -> tuple[list[Any], ...]`
Runs the scenario.
- **Parameters:**
  - `args`: Arguments to supply to input nodes.
- **Returns:** Output of each separate node.

### `cleanup() -> bool`
Cleans up the scenario.
- **Returns:** `True` if successful, `False` otherwise.
