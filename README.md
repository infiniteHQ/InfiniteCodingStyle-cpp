# Infinite C++ coding style and convention
Coding conventions for Infinite projects written in C++

This document presents C++ code conventions we use at Infinite. This can help contributors understand our principles to read and write better code.

The goal of this repository is to keep everything simple and understandable.

# Table of Contents
- [Main goal](#main-goal)
- [Brief](#brief)
- [Rules](#rules)
- [Style](#style-convention)
    - [Files extension](#files-extension)
    - [Files name](#files-name)
    - [Project structures](#project-structures)
    - [Brackets and formats](#brackets-and-formats)
    - [Casing](#casing)
    - [Names](#names)
    - [Variables](#variables)
    - [Functions](#functions)
    - [Classes](#classes)
    - [Members](#members)
    - [Methods](#methods)
    - [Structures](#structures)
    - [Namespaces](#namespaces)
    - [Code separation](#code-organisation-and-separation)
    - [Comments](#comments)
    - [Indentation](#indentation)
- [Code](#code)
    - [Memory management](#memory-management)
    - [Thread management](#thread-management)
    - [Variables/Members management](#variables-and-members-management)
    - [Data Structures](#data-structures)
    - [Modern features](#modern-features)
- [About publication](#about-publication)


# Main goal
Keep the code as simple as possible and understandable. Have consistency by using the same coding style and the same semantic for all the codebase. Use modern C++

# Brief
In short, we name all variables, functions, methods and members in snake_case for better visibility and to follow the standard library of C++. Types, class enums, and struct names must be in PascalCase, members must end with _ to indicate that it is a member of a class.

Regarding brace placement, we prefer the K&R style.

A readable function should be fewer than 50 lines, and a well-structured file fewer than 600 lines.

Use the most modern C++ standard possible, within the constraints of the project.

Avoid manual memory management, prefer RAII and value semantics

Name your functions and variables with consistency and have logical naming.

Consistency is key, whatever casing convention a project uses, respect it and don't try to code in a different semantic.

Data structure and data organisation is one of the most important things. Use structs as function parameters instead of using 6+ parameters for example.

Do not comment everything, because good code is understandable by a beginner. But it is important to add a brief description at the beginning of files, definitions, groups of declarations and even on certain complex parts of the code.

Simple code that a beginner can understand is the goal.

# Rules
Here are the general rules to make the code safe, readable, and maintainable:

- A good function length is under 50 lines
- A good file length is under 600 lines
- One function does one thing
- One file contains functions and things belonging to the same feature group
- Avoid manual memory management, prefer RAII and value semantics
- Use modern C++, avoid old C++ or C-style code in a C++ context
- Whatever naming convention we choose, use the same convention for the entire codebase

# Standards and scope
This document was written around the C++20 standard, targeting modern applications and programs.

# Style convention
### Files extension

| File extension | For what |
|---|---|
| `.cpp` | for C++ sources |
| `.hpp` | for C++ headers (never use .h for C++ files) |
| `.inl` | for C++ inline features |
| `.c` | for C sources |
| `.h` | for C headers |

### Files name
We like using snake_case for files too. It is understandable, and we can divide a single header declaration into different source definitions.

```
content_browser.hpp
content_browser.cpp
content_browser_navigation.cpp
content_browser_rendering.cpp
```

### Project Structures
We use two different project structure styles:

- `main` folder style, containing all header and source files organized by functionality.
example :

```
    /main
    /main/engine
    /main/engine/renderer.hpp
    /main/engine/renderer.cpp
    /main/editor/viewport.hpp
    /main/editor/viewport.cpp
    /main/physics
    /main/physics/velocity.hpp
    /main/physics/velocity.cpp
```
- `src` and `include` folders style:

```
    /include
    /include/engine
    /include/engine/renderer.hpp
    /include/editor/viewport.hpp
    /include/physics
    /include/physics/velocity.hpp

    /src
    /src/engine
    /src/engine/renderer.cpp
    /src/editor/viewport.cpp
    /src/physics
    /src/physics/velocity.cpp
```

### Brackets and formats
Regarding brace placement, we prefer the K&R style.

```cpp
int foo() {
    return 42;
}
```

```cpp
if() {
    // code here
} else {
    // code here
}
```

We use brackets even if we have only one call in the condition

```cpp
// bad
if(user.points() > POINTS_TO_WIN)
   user.trigger_winner_menu(); 

// better
if(user.points() > POINTS_TO_WIN){
   user.trigger_winner_menu(); 
} 
```

### Casing
We prefer using snake_case for everything, it is easier to read. It is also the choice of the C++ standard library, and it is close to the STL. (Yes, it is a little bit more difficult with the typing of _, but no casing is perfect)

PascalCase and camelCase are acceptable, but consistency within the codebase is what matters. Do not use multiple casing styles in the same codebase.

```cpp
int vehicleSpeedInMph;    // Not easily readable
int VehicleSpeedInMph;    // Not easily readable
int mVehicleSpeedInMph;   // Even worse
int vehicle_speed_in_mph; // Better
```

```cpp
// Not easily readable
void SendMessageToServer(const std::string& clientMessage) {
    auto& server = GetServer();
    server->Send(clientMessage);
    mLatestMessage = clientMessage;
}

// Better
void send_message_to_server(const std::string& client_message) {
    auto& server = get_server();
    server->send(client_message);
    latest_message_ = client_message;
}
```
### Names
Correctly naming your functions, variables and structures is one of the keys to readability.
We need to know what a method or a function does only by reading its name, and we need to know what a variable or member is only by reading its name.

```cpp
// Bad
void draw(...)        // Draw what?
void send(...)        // Send what to who?
void initialize(...)  // Initialize what?

// Better
void draw_circle(...)
void send_to_server(...)
void initialize_interface(...)
```

```cpp
// Bad
int n;              // What is this integer, what is it doing?
std::string name;   // Whose name?
float radius_;      // Radius of what?

// Better
int number_of_users;
std::string user_name;
float rectangle_radius_;
```

But sometimes, simplicity is acceptable if it is comprehensible.

```cpp
int i;      // loop index
int x, y;   // 2D coordinates
float t;    // normalized time [0, 1]
```

### Variables
Variables must be named in snake_case and their name must clearly describe what they represent.

```cpp
int number;                      // Simple variable
int another_number;              // Variable with multiple words
constexpr int MAX_SPEED = 42;    // Constant number
static const int NUMBER = 42;    // Static constant number
```

### Functions
For all function types, we prefer `snake_case` to guarantee visibility and a clear distinction.

```cpp
void some_function();
static void some_static_function();
```

### Classes
Class names must be in PascalCase. Methods and members follow the same naming rules as functions and variables.

```cpp
class Car
{
public:
    void drive();
    void trigger_alarm();
    void set_color(const std::string& hex_color);

private:
    int mph_speed_;
};
```

### Members
Class members follow the same naming rules as variables, but must end with `_` to distinguish them from local variables.

```cpp
int number_;        // Member of a class
std::string name_;  // Member of a class
```

### Methods
For all method types, we prefer `snake_case` to guarantee visibility and a clear distinction.

```cpp
void some_method();
void another_method();
int another_method_with_int_return();
```
### Structures
Good use of structs is a great advantage for code coherence and consistency.
Structs help organize data and avoid code duplication.

For example, for functions or methods that take many parameters, use structures to aggregate those values instead of having 6+ parameters.

```cpp
// bad declaration
void draw_horizontal_item_card(
    const std::string &name,
    const std::string &path,
    const std::string &description,
    const std::string &size,
    const std::string &logo,
    const std::shared_ptr<ItemIdentifierInterface> &item_ident);

// bad use (magic parameters)
draw_horizontal_item_card(
    filename_string,
    path,
    item_entry.first->description,
    folder_size_string,
    item_entry.first->logo_path,
    item_entry.first,
);
```

This is better, more readable and understandable:

```cpp
// This struct is intended for call-site use only and must not outlive its parameters.
struct ItemCardContent {
    const std::string &name;
    const std::string &path;
    const std::string &description;
    const std::string &size;
    const std::string &logo;
    const std::shared_ptr<ItemIdentifierInterface> &item_ident;
};

// better declaration
void draw_horizontal_item_card(const ItemCardContent &content);

// better use
draw_horizontal_item_card({
    .name        = filename_string,
    .path        = path,
    .description = item_entry.first->description,
    .size        = folder_size_string,
    .logo        = item_entry.first->logo_path,
    .item_ident  = item_entry.first,
});
```

Structs help keep the code organized and limit code duplication.

### Namespace
Avoid using the `using` keyword. Prefer full namespaces to avoid confusion.
Use namespaces to separate the main APIs and contexts of your project. It makes it easy to distinguish between groups of features.

```cpp
project::scripting::...  // We know that this is scripting
project::rendering::...  // We know that this is rendering
```
### Code organisation and separation
Use scope brackets `{}` to isolate redundant code blocks that share similar variable names.

```cpp
// Save button
{
    const std::string label = "Save";
    if(ui::button(label)) {
        // code
    }
}

// Refresh button
{
    const std::string label = "Refresh";
    if(ui::button(label)) {
        // code
    }
}
```
### Comments
Good code should be self-documented.
Comments are important to help understanding, but good code does not need too many comments. Good code is simple, and simple code is comprehensible by itself.

```cpp
// Do we really need a comment here? No.
if(vehicle_speed_in_mph > 100) {
    catch_plate_with_photo();
}
```

Each function, class, struct or enum should have a brief description at the top.

```cpp
//
// print_hello_to_user
// Prints hello to the user name and returns a number.
//
int print_hello_to_user(const std::string& username) {
    std::cout << "Hello, " << username << std::endl;
    return 42;
}
```

Each file should have a brief description at the top.

```cpp
//
//  main.cpp
//  This file contains the main functions to print hello to a user name.
//
//  Copyright (c) 2022 Owner
//
//  This work is licensed under the terms of the LICENCE license.
//  For a copy, see <https://opensource.org/licenses/LICENCE>.
//
```
### Indentation
Do not use the Tab key. Because every editor handles it differently, prefer 2 or 4 spaces. You can use clang-format to do that automatically.

# Code

### Memory management
Good memory management will guarantee memory safety.

#### Our rules
- Do not use C style memory management in C++
- Have the same memory management method for an entire context
- Prefer smart pointers (`std::unique_ptr`, `std::shared_ptr`) over raw pointers
- Avoid manual `new` and `delete`

### Thread management
#### Our rules
- Use modern C++ features like mutex, semaphores or atomic members and variables to secure access and avoid race conditions

### Variables and members management
#### Our rules
- Always use `const` if possible (for a member, a variable, an object or whatever)
- Prefer references over copies to avoid unnecessary object duplication
- Use `auto` for complex types when it does not create confusion

### Data Structures
Data structure and data organisation is one of the most important things for code safety and quality.

#### Our rules
- For a group of variables or members, prefer using structures

### Modern features
C++ (in our choice C++20, and even more with C++23) offers a lot of modern features to create powerful and safe code.

#### Our rules
- Use modern loops
```cpp
// bad
for(int i = 0; i < words.size(); i++) {
    std::string word = words[i];
    // code here
}
// better
for(auto& word : words) {
    // code here
}
```
- Prefer structured bindings for pairs and tuples
```cpp
// bad
auto pair = get_user();
std::string name = pair.first;
int age = pair.second;
// better
auto& [name, age] = get_user();
```
- Use `std::optional` instead of returning null or error codes
```cpp
// bad
User* find_user(int id); // nullptr if not found
// better
std::optional<User> find_user(int id);
```
- Prefer `enum class` over raw `enum`
```cpp
// bad
enum Direction { Left, Right, Up, Down };
// better
enum class Direction { Left, Right, Up, Down };
```
- Use modern C++ standard features like `std::move`, `std::find`, `std::function` and more.

# About publication
This publication may change to add new things, or correct eventual errors and imprecisions.
We will keep this document clear and concise, without overloading it with unnecessary information.