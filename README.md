# Kate can be used as a full fledged IDE for C (and C++).
Note: I use clang as my compiler. 

You can use existing plugins for your compilation but then you have limited control over what happens. To address that I've developed a intelligent makefile generator that:
- discovers all included headers, indepent of location, autonomously.
- can build 3 targets: normal, debug and release (optimised binary) depending on invocation (see shortcuts).
- cleans up afterwards
- reports progress and results in the kate output pane

# Makefile generator
Intelligent bash script to create a C makefile that supports 3 build targets (debug, regular, release) where the release build delivers a highly optimised binary. copy the script in your C project folder and run the script (don't forget to set the execute permission. A makefile will be generated. Tested and approved (by me) on 10 different C projects, small and large.

# Kate configuration
Kate can execute external programs and commands. We use this to configure each build command and save it so we can assign a shortcut to each one.

Add 3 new external tools:
- Name: C Build (or C Build Debug, C Build Release)
- Executable: sh

- For C Build use: Arguments: -c "./makefile_gen.sh && if [ -f makefile ]; then make; else clang -Wall -Wextra -O2 '%{Document:FileName}' -o '%{Document:FileBaseName}'; fi"
- For C Build Debug use: Arguments: -c "./makefile_gen.sh && if [ -f makefile ]; then make debug; else clang -Wall -Wextra -g -O0 '%{Document:FileName}' -o '%{Document:FileBaseName}'; fi"
- For C Build Release use: Arguments: -c "./makefile_gen.sh && if [ -f makefile ]; then make release; else clang -Wall -Wextra -O3 -march=native -s '%{Document:FileName}' -o '%{Document:FileBaseName}_release'; fi"

- Working Directory: %{Document:Path}
- Mime Types: text/x-chdr; text/x-cmake; text/x-csrc; text/x-makefile
- Save: All Documents
- Output: Display in Pane

# Kate shortcuts
Assign a shortcut to each of the external tools you've just added: Kate > Menu > Settings > Configure Keyboard Shortcust.

# Project snapshot generator
Another tool I use frequently is snapshot generator. This bash script creates one file with meta data and all project source (.c, .h, .conf, .sh, .qml, .json, (readme) text and make) files. This can be extremely useful if you want an LLM to do a project audit so I've included this one as well.
