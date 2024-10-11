# Brief guides to adding basic [MTS-ESP](https://github.com/ODDSound/MTS-ESP) support to open source synthesizers

Many open source synthesizers don't support [MTS-ESP](https://github.com/ODDSound/MTS-ESP). These examples are provided to help those who wish to add basic support to such synths.

# [odin2](https://github.com/TheWaveWarden/odin2) version 2.3.4

**NOTE!** After cloning the odin2 repo, it was necessary to add ```#include <utility>``` to libs/JUCELV2/modules/juce_gui_basics/juce_gui_basics.h to fix a bug in JUCELV2 that prevents building. This may not be required on all systems.

Odin2 can be tuned by loading SCL and KBM files. After the following changes, using an MTS-ESP master plugin will override those tunings.  Note that tuning is not continuous - any scale changes will only take effect on a new midi note. There is no note filtering.

Altered source files: **Source/audio/Voice.h**  (Updated file provided)<br>
New directories: **libs/oddsound-mts**<br>
New modules: MTS-ESP source code in **libs/oddsound-mts**<br>
New CMakeLists.txt files: **libs/oddsound-mts/CMakeLists.txt**  (Provided)<br>
Edited CMakeLists: **CMakeLists.txt** in the root odin2 directory.  (Updated file provided)

**Brief instructions (more details are [here](ODIN2.md)):**

1. Clone [odin2](https://github.com/TheWaveWarden/odin2) recursively with all submodules.
```
git clone --recurse-submodules https://github.com/TheWaveWarden/odin2.git
```
It may be necessary to add ```#include <utility>``` to libs/JUCELV2/modules/juce_gui_basics/juce_gui_basics.h   (see NOTE above).

2. Replace CMakeLists.txt in the main odin2 folder with the one from this repository.

3. Replace Source/audio/Voice.h with the one from this repository.

4. Create a new folder named oddsound-mts in libs/

5. Add oddsound-mts/CMakeLists.txt to the new folder, so **libs/oddsound-mts/** contains the new CMakeLists.txt

6. Clone the MTS-ESP repository inside the oddsound-mts folder:
```
git clone https://github.com/ODDSound/MTS-ESP.git
```
**libs/oddsound-mts/** should now contain a CMakeLists.txt, and a folder named **MTS-ESP** which contains the MTS-ESP source code.

7. Build odin2 with CMake as described in the [odin2 README](https://github.com/TheWaveWarden/odin2)

# [Wavetable](https://github.com/FigBug/Wavetable) version 1.0.21

Altered source files: **plugin/Source/WavetableVoice.h plugin/Source/WavetableVoice.cpp**  (Updated files provided)<br>
New directories: **modules/oddsound-mts**<br>
New modules: MTS-ESP source code in **modules/oddsound-mts**<br>
New CMakeLists.txt files: **modules/oddsound-mts/CMakeLists.txt**  (Provided)<br>
Edited CMakeLists: **CMakeLists.txt** in the root Wavetable directory.  (Updated file provided)

**Brief instructions:**

1. After cloning Wavetable, replace CMakeLists.txt in the main Wavetable folder with the one from this repository.

2. Replace plugin/Source/WavetableVoice.h and plugin/Source/WavetableVoice.cpp with the ones from this repository.

3. Create a new folder named oddsound-mts in modules/

4. Add oddsound-mts/CMakeLists.txt to the new folder, so **modules/oddsound-mts/** contains the new CMakeLists.txt

6. Clone the MTS-ESP repository inside the oddsound-mts folder:
```
git clone https://github.com/ODDSound/MTS-ESP.git
```
**modules/oddsound-mts/** should now contain a CMakeLists.txt, and a folder named **MTS-ESP** which contains the MTS-ESP source code.

7. Build step 1:
```
cmake -B build -D CMAKE_BUILD_TYPE=Release
```
8. Build step 2:
```
cmake --build build --config Release
```

# Credits and attribution
[odin2](https://github.com/TheWaveWarden/odin2): GNU General Public License Version 3. Copyright (C) 2020 - 2021 TheWaveWarden

[Wavetable](https://github.com/FigBug/Wavetable): BSD-3-Clause License. Copyright (c) 2020, Roland Rabien

[MTS-ESP](https://github.com/ODDSound/MTS-ESP): [BSD Zero Clause License](https://github.com/ODDSound/MTS-ESP/blob/main/LICENSE) Copyright (C) 2021 by ODDSound Ltd. info@oddsound.com

