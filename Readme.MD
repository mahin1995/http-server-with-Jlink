

# Creating and Packaging a Small HTTP Server with Java Modules and `jlink`

This guide outlines the steps to create a small HTTP server in Java using modules and package it into a custom runtime image using `jlink`.

## Project Structure

Assume the following directory structure for your project:


project
└── src
└── httpServerModule
├── module-info.java
└── com
└── example
└── httpserver
└── SimpleHttpServer.java


## Steps

### Step 1: Compile `module-info.java`

Compile the module descriptor (`module-info.java`) to the `out` directory:

```bash
javac -d out .\src\httpServerModule\module-info.java
```

### Step 2: Compile HTTP Server Class

Compile the HTTP server class (`SimpleHttpServer.java`) and place the compiled classes into the `out` directory:

```bash
javac -d out --module-path out src\httpServerModule\com\example\httpserver\SimpleHttpServer.java
```

### Step 3: Test the Module

Verify that the module and its dependencies are correctly set up:

```bash
java --module-path out --module httpServerModule/com.example.httpserver.SimpleHttpServer
```

### Step 4: Check Module Dependencies

Check if the module dependencies are correctly resolved:

```bash
jdeps --module-path out -s --module httpServerModule
```

### Step 5: Create Custom Runtime Image with `jlink`

Package the application into a custom runtime image (`customjre`) using `jlink`:

```bash
jlink --launcher customjrelauncher=httpServerModule/com.example.httpserver.SimpleHttpServer --module-path "%JAVA_HOME%\jmods;out" --add-modules httpServerModule --output customjre
```

### Step 6: Run the Custom JRE Bundle

Execute the HTTP server using the custom launcher created by `jlink`:

```bash
customjre\bin\customjrelauncher
```
