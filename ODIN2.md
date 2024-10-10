# A brief guide to add basic [MTS-ESP](https://github.com/ODDSound/MTS-ESP) support to odin2

# [odin2](https://github.com/TheWaveWarden/odin2) version 2.3.4

NOTE! After cloning the odin2 repo, it was necessary to add ```#include <utility>``` to libs/JUCELV2/modules/juce_gui_basics/juce_gui_basics.h to fix a bug in JUCELV2 that prevents building. This may not be required on all systems.

After cloning the odin2 repository, we need to add MTS-ESP as a submodule. Add a folder to **libs/** named **oddsound-mts**. Then add the necessary **CMakeLists.txt** to **libs/oddsound-mts** (provided in this repository). Now download the MTS-ESP source and place within **libs/oddsound-mts/** (or clone MTS-ESP inside the libs/oddsound-mts/ folder).

To clone the MTS-ESP repository, use this command within libs/oddsound-mts.

```
git clone https://github.com/ODDSound/MTS-ESP.git
```

There should now be a directory  **libs/oddsound-mts/MTS-ESP**  containing the MTS-ESP source code.

Now we need to update CMakeLists.txt in the root odin2 directory with the following additions. The updated CMakeLists.txt is also provided in this repository.

(Under ================== CMake Subdirs ======================)
```
add_subdirectory(libs/oddsound-mts)
```

(Under ==================== Check OS =======================)
```
target_include_directories(Odin2 PRIVATE libs/oddsound-mts)
```

(Under ==================== Binary Files ======================= after the set_target_properties(Odin2_BinaryData...  line )
```
set_target_properties(oddsound-mts PROPERTIES POSITION_INDEPENDENT_CODE TRUE)
```

(In ==================== Linkage ======================= add the following to target_link_libraries)
```
oddsound-mts
```

Finally, update Source/audio/Voice.h :

Add the following #include:
```
#include "MTS-ESP/Client/libMTSClient.h"
```

In struct Voice, before Voice() add:
```
	MTSClient *mtsClient = 0;
```

In the Voice() contructor add:
```
		mtsClient = MTS_RegisterClient();
```

Add a destructor after the constructor:
```
	virtual ~Voice()
	{
		MTS_DeregisterClient(mtsClient);
	}
```

Replace the existing float MIDINoteToFreq function with the following:
```
	float MIDINoteToFreq(int p_MIDI_note) {
		float freq = m_tuning_ptr->frequencyForMidiNote(p_MIDI_note);
		if (mtsClient && MTS_HasMaster(mtsClient))
		{
			freq = MTS_NoteToFrequency(mtsClient, p_MIDI_note, -1);
		}
		return freq;
		//return 27.5f * pow(2.f, (float)(p_MIDI_note - 21) / 12.f);
	}
```

Finally, build odin2 as usual.

# Credits and attribution
[odin2](https://github.com/TheWaveWarden/odin2): GNU General Public License Version 3. Copyright (C) 2020 - 2021 TheWaveWarden

[MTS-ESP](https://github.com/ODDSound/MTS-ESP): [BSD Zero Clause License](https://github.com/ODDSound/MTS-ESP/blob/main/LICENSE) Copyright (C) 2021 by ODDSound Ltd. info@oddsound.com

