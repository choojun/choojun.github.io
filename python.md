# Python with XCode

To run a Python script in Xcode with a virtual environment, you need to configure Xcode's build settings and scheme to use the Python interpreter within your virtual environment rather than the system's default Python. 


## Step 1: Create the Virtual Environment 
First, create a virtual environment in your project's directory using your Mac's Terminal. 
Open Terminal (Cmd + Space, type "Terminal").
Navigate to your project directory (e.g., cd path/to/your/xcode_project).
Create a virtual environment (named venv in this example): 
~~~bash
python3 -m venv venv
~~~

Activate the environment (optional for this setup, but useful for installing packages in the terminal): 
~~~bash
source venv/bin/activate
~~~

Install any necessary packages:
~~~bash
pip install package_name
~~~

Deactivate the environment:
~~~bash
deactivate
~~~
 
## Step 2: Set up the Xcode Project 
1. Open Xcode and select Create a new Xcode project.
2. Choose the Command Line Tool template under the macOS section and click Next.
3. Name your project and set the Language to a generic option like C (you will override this with Python).
4. Save the project in the same folder where you created your venv in Step 1. 

## Step 3: Configure Xcode to use the Virtual Environment Python 
1. In Xcode, select your project in the Project Navigator.
2. Go to the Build Phases tab of your target's settings.
3. Click the + button at the top and select Add a Run Script Phase.
4. Drag the new Run Script phase below the Compile Sources phase (or ensure it runs when you want the script to execute).
5. In the script text area, enter the following, replacing your_script.py with the name of your Python file:
~~~bash
#!/bin/bash

# Define the path to the virtual environment's Python interpreter
VENV_PATH="${PROJECT_DIR}/venv/bin/python3"

# Path to your Python script
SCRIPT_PATH="${PROJECT_DIR}/main.py"

# Execute the script using the virtual environment's interpreter
"$VENV_PATH" "$SCRIPT_PATH"
~~~
6. This script specifically calls the Python executable located within your venv folder, ensuring all installed packages are accessible. 

## Step 4: Write and Run the Python Code
1. In the Project Navigator, right-click on your project folder and select New File....
2. Choose the Empty template and name the file main.py.
3. Write your Python code in main.py.
4. To run your script, click the Build and Run button (the play icon, or Cmd + R). The output will appear in the Xcode console. 

