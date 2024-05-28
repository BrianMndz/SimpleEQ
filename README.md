# Audio-Plugin-Template

This is a template repository for a JUCE project using CMake and Xcode.
_Based on the Pamplejuce by Sudara but for beginners_

## Getting Started
git clone your new repo

```
git clone --recurse-submodules my-cloned-repo
```

Populate the JUCE by running git submodule update --init in your repository directory. By default, this will track JUCE's develop branch, which is a good default until you are at the point of releasing a plugin. It will also pull in the CMake needed and an example module, my component inspector.

Replace (refactor) with the name of your project in CMakeLists.txt where the PROJECT_NAME variable is first set. Make this all one word, no spaces.

Adjust which plugin formats you want built as needed (VST3, AU, etc).

Set the correct flags for your plugin juce_add_plugin. Check out the API https://github.com/juce-framework/JUCE/blob/master/docs/CMake%20API.md and be sure to change things like PLUGIN_CODE and PLUGIN_MANUFACTURER_CODE and everything that says Change me!.

Build n' Run! If you want to generate an Xcode project, run cmake -B Builds -G Xcode. Or just open the project in CLion or VS2022. Running the standalone might be easiest, but you can also build the AudioPluginHost that comes with JUCE. Out of the box, Pamplejuce's VST3/AU targets should already be pointing to it's built location.

### How do I add another module?
Additional 3rd party JUCE modules go in /modules. You can add third party git submodules there (like the inspector is set up). Remember to not only call juce_add_module but add it to the target_link_libraries list!

I (and others, including some of the JUCE team) recommend moving as much as your application code into modules as possible. For example, if you tend to roll your own widgets, pop those into a module, you'll thank yourself later.

A few reasons to do so:

Re-usability. You can use modules across projects.
Testability. You can test modules in isolation from each other. When sticking test in modules, it's common to guard .cpp files with something like #ifdef RUN_MY_TESTS and set via target_compile_definitions in Tests target.
Sanity. You can keep the root project tidy, focused on the application logic.
Compile-friendliness. Each JUCE module is its own compilation unit. If you change a file in a module, only that one module needs to rebuild. It also means you can work on only the module in a separate CMake project, which is a very nice/fast life.
Don't worry about all of this if you are new to JUCE. Just keep it in mind as you grow.


Update the submodules
```sh
git submodule update --init
cmake -G Xcode -H. -B Xcode
````

or
```sh
cmake -B Builds -G Xcode
````

What's the deal with BinaryData?
Your binary data CMake target is called Assets.

You need to include BinaryData.h to access it.

Important

You may have to configure the project (just hit build in your IDE) to build juceaide before the header will be available.