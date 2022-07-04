# Python Source Code Obfuscation


In software development, Obfuscation is the deliberate act of creating Source Code that is difficult for humans to understand. The Code is often obfuscated to protect intellectual property or trade secrets, and to prevent an attacker from reverse engineering a proprietary software program.

Encrypting some or all of a program’s code is one obfuscation method.

In Python, There are multiple packages available using which you can obfuscate your code base and secure your intellectual property.

pyarmor — full obfuscation with hex-encoding; apparently doesn’t allow partial obfuscation of variable/function names only.
python-minifier — minifies the code and obfuscates function/variable names.
pyminifier — does a good job in obfuscating names of functions, variables, literals; can also perform hex-encoding (compression) similar as pyarmor. Problem: after obfuscation the code may contain syntax errors and not run.
While researching, I found another option to obfuscate my Python Code using Cython which was quite useful and difficult to Reverse Engineer.

Obfuscation is one of the many features Cython package provides. Cython is an optimizing static compiler that takes your .py modules and translates them to high-performant C files. Resulting C files can be compiled into native binary libraries with no effort. When the compilation is done there’s no way to reverse compiled libraries back to readable Python source code.

Let me walk you through steps towards Python source code obfuscation using Cython. We’ll use a Simple project to Showcase the same.

### Sample Code to Demonstrate the Code Obfuscation
I have written a small python Flask based code which takes numeric equation as an Input, validates the equation and if valid, resolved the equation. Nothing Logical, just for tutorial purpose. The idea is to showcase the code obfuscation of the business logic.

The Code repo looks like…

```js
.
├── README.md
├── __init__.py
├── main.py
├── routes.py
├── swagger.yml
├── src
│   ├── __init__.py
│   └── solver.py
└── compile.py
```

### Code Walk-through
- main.py : This file is the flask bootstrap file through which the flask app starts.
- routes.py : This file hosts all the API routes provided by the application
- swagger.yml : This file hosts the API Details which can be accessed in the form of formatted documentation via Browser.
- src/solver.py : This file hosts the business logic of verifying the equation and if valid, then solving the equation.
- compile.py : This file hosts the code base to build the .so files from the .py files.

### Code Execution
To run the Flask application execute the below command in the terminal
```js
python main.py
```

Now you can access the Swagger API Documentation using the URL
```js
http://<public-ip>/ui
```

### Run compile.py

Depending on the Python version you use, run:

```js
python3 compile.py build_ext --inplace
```

The above command will generate `.so` and `.c` files next to your `.py` source files:
```js
.
├── README.md
├── __init__.py
├── main.py
├── routes.py
├── swagger.yml
├── src
│   ├── __init__.py
│   ├── solver.py
│   ├── solver.c
│   └── solver.cpython-37m-x86_64-linux-gnu.so
└── compile.py
```

The `.c` files are intermediate sources used to generate `.so` files, which are binary modules you want to distribute. When building on Windows these files will probably have the .pyd extension.

You can delete `.pyc`, `.c` and `.py` files after a successful build and keep the `.so` files only. A method has been added for the same.

Note that `.so`-files contain the target platform in their names (e.g. darwin on my MacOS). Obviously, the compiled modules are not cross-platform. If you distribute your program to Ubuntu Linux users, you should compile it on Linux. Otherwise you won’t be able to load these binaries. So you’ll have to compile a platform-specific version of your code for each of your targeted platforms.

The trigger of this script can be added in the setup files or Dockerfile which supports the building of the application package. The Final package that needs to be shipped will have an obfuscated Intellectual Property which is really hard to reverse Engineer.
