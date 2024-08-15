---
title: "Stacktile on River: A Guide to Efficient Window Management"
date: 2023-05-29
---
# Configuring Stacktile
To begin working with Stacktile on River, we need to configure the necessary settings. In the River config file, add the following lines:
```
riverctl spawn stacktile
riverctl default-layout stacktile
```

These lines initialize Stacktile and set it as the default layout. However, if you decide to remove Stacktile later, remember to modify the changes to the main window's placement and resize the window accordingly.

# Creating a Stacktile Config
To customize Stacktile's behavior, we can create a configuration file. Here's an example of a Stacktile config:
```
[config]
runtime-changes=per-output;

[output:eDP-1]
layouts=default;

[layout:default]
metalayout=linear;
gravity=left;
sublayouts=columns:2 rows:2 stack;
main-ratio=0.6;
outer-padding=10;
main-padding=10;
```

In this config, we define various parameters such as the runtime change behavior, output-specific layouts, and specific settings for the default layout. Feel free to adjust these values to suit your preferences.

# Using wlr-randr to Identify Outputs
To determine the appropriate output identifier for Stacktile's configuration, you can utilize a tool called `wlr-randr`. It provides information about available outputs and their identifiers. Execute the command `wlr-randr` in your terminal to retrieve the necessary details.

# Modifying Layouts during Runtime
One of the standout features of Stacktile is the ability to change layouts dynamically while your system is running. To achieve this, you can use the following command:

```
riverctl send-layout-cmd stacktile "set <variable> = <value>"
```

By replacing `<variable>` and `<value>` with the desired layout parameters, you can easily modify the layout to suit your workflow on the fly.
