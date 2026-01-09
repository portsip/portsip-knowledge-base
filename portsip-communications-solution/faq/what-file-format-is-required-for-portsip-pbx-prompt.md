# What File Format Is Required for PortSIP PBX Prompt?

### Supported Audio File Format

PortSIP PBX supports **WAV audio files** using the following specifications:

* **Codec:** PCM (uncompressed)
* **Sample rate:** 16 kHz, 32 kHz, or 48 kHz
* **Bit depth:** 16-bit
* **Channels:** Mono

Only audio files that meet these requirements can be uploaded and used as system prompts.

***

### Applicable Features

The supported audio file format applies to the following PortSIP PBX features:

* Virtual Receptionist (IVR)
* Call Queue
* Conference
* Voicemail
* Music on Hold (MOH)
* Call Parking

***

### Converting Unsupported Audio Files to WAV Format

If your audio file does not meet the required format, you must convert it before uploading it to PortSIP PBX.

The steps below use [Audacity](https://www.audacityteam.org/download/windows/), a free and widely available audio editor.

***

#### Prerequisites

* [Audacity ](https://www.audacityteam.org/download/windows/)is installed on your computer
* The source audio file you want to convert

***

#### Step-by-Step Instructions

1. **Install Audacity**\
   Download and install the free Audacity audio editor.
2. **Open the audio file**
   * Launch Audacity.
   * Go to **File > Open**, then select the audio file you want to convert.
3. **Export the file as WAV**
   * Go to **File > Export Audio**.
4. **Configure the export settings**\
   In the **Export Audio** window, under **Audio Options**:
   * **Channels:** Select **Mono**
   * **Sample Rate:** Choose **16000 Hz**, **32000 Hz**, or **48000 Hz**
   * **Encoding:** Select **Signed 16-bit PCM**
5. **Save the converted file**\
   Click **Export** to save the WAV file to your computer.

***

#### Expected Outcome

* The exported WAV file fully complies with PortSIP PBX audio requirements.
* The file can be uploaded and used successfully as a prompt in supported PBX features.

<figure><img src="../../.gitbook/assets/portsip-pbx-convert-wav-file.png" alt=""><figcaption></figcaption></figure>





