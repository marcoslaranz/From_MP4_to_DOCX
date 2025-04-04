![FFmpeg](https://img.shields.io/badge/Tool-FFmpeg-green)
![Tesseract](https://img.shields.io/badge/Tool-Tesseract-blue)
![OCR](https://img.shields.io/badge/Category-OCR-yellow)

## Tags
- Edit MP4 as Word Document (TXT)
- Video Processing
- OCR (Optical Character Recognition)
- FFmpeg
- Tesseract
- Automation

# Edit MP4 as Word Document
---

# Contents
1. **Why this script was created**  
2. **Problem**  
3. **Prerequisites**  
4. **Use this script to**  
5. **Steps**  
    5.1. Create a small MP4 segment  
    5.2. Generate PNG frames  
    5.3. Extract text with Tesseract  
    5.4. Combine TXT files  
6. **Installing prerequisite programs**  
    6.1. ffmpeg.exe  
    6.2. tesseract.exe  

---

## 1. Why this script was created

---

### 2. Problem
You are reading some documents on your laptop that you will no longer have access to after a specific time. Therefore, you won't be able to read them later, and you cannot copy or email this content to yourself. However, you want to keep these documents because you might want to read them later, perhaps to print or create personalized material for yourself.

---

### 3. Prerequisites
- Install **ffmpeg.exe**: This is open-source software that can be installed on Windows 11 and accessed through the MS-DOS command line. (See instructions in section 6.1).
- Install **tesseract.exe**: This is open-source OCR software for text recognition. It can also be installed on Windows 11 and accessed via the MS-DOS command line. (See instructions in section 6.2).

---

### 4. Use this script to:
- Break an MP4 file into smaller segments.
- Extract frames from an MP4 file
- Convert them into text documents using OCR.
- For example, suppose you have an MP4 file (`GH020138.MP4`) that you filmed of a PDF document using your GoPro camera or mobile phone.
  You can extract the portion that contains the document, such as from 00:00:00 to 00:02:04 usging the command:

  create_seg.bat GH020138.MP4 00:00:00 00:02:04

- Suggest use:
- 
  Copy your MP4 file to an empty directory,(folder).
  Example:
  
      C:\> mkdir convert_mp4
  
      C:\> cd convert_mp4
  
      C:\convert_mp4> copy C:\Users\%username%\Documents\GH020138.MP4 .
  
      C:\convert_mp4> create_seg.bat GH020138.MP4 00:00:00 00:02:04

      This assumes your file is in C:\Users\%username%\Documents and its name is GH020138.MP4
  
      Reason: The convert_mp4 directory will contain only the resulting .txt file of the converted file.
  This command assume your create_seg.bat is in the path, if not please add it in your PATH variable, or
  temporary change it, exaple:

#### 4.1 Change your PATH:

      Supposed you downloaded and copy the BAT file create_seg.bat to the following:

      C:\BATS> dir 
      Volume in drive C is Windows-SSD
      Volume Serial Number is 3893-E587
    
      Directory of C:\BATS
    
      04/04/2025  10:58 am    <DIR>          .
      04/03/2025  11:27 am    <DIR>          ..
      05/03/2025  10:51 am    <DIR>          .vagrant
                        1 Dir(s)  14,729,920,512 bytes free

    - Before use the script you need to change the PATH
    
      cd \convert_mp4

      C:\convert_mp4> set PATH=%PATH%;C:\BATS
    
               
---

### 5. Steps
#### 5.1. Create a Small MP4 Segment
- Use the script to extract only the part of the video that interests you.

#### 5.2. Generate PNG Frames
- Create a PNG file for every second of the MP4 segment.

#### 5.3. Extract Text from PNG Files
- Use **Tesseract OCR** to convert PNG files into `.TXT` files:
   ```cmd
   for %i in (output_frame_*.png) do tesseract %i %~ni
   ```

#### 5.4. Combine TXT Files
- Combine all generated `.TXT` files into one large `.TXT` file.
- Open the combined file in MS Word for editing and corrections.

---

### Script: `create_seg.bat`
```bat
@echo Off
set PATH=%PATH%;"C:\Users\marco\AppData\Roaming\Digiarty\VideoProc Converter AI"

REM Check if parameters are provided
if "%1"=="" (
    echo Syntax: create_seg.bat [input_file] [start_time] [end_time]
    echo Example: create_seg.bat GH020138.MP4 00:00:00 00:02:04
    exit /b
)

if "%2"=="" (
    echo Syntax: create_seg.bat [input_file] [start_time] [end_time]
    echo Example: create_seg.bat GH020138.MP4 00:00:00 00:02:04
    exit /b
)

if "%3"=="" (
    echo Syntax: create_seg.bat [input_file] [start_time] [end_time]
    echo Example: create_seg.bat GH020138.MP4 00:00:00 00:02:04
    exit /b
)

REM Run ffmpeg command to extract video segment
echo Extracting segment from %1...
ffmpeg.exe -i "%1" -ss %2 -to %3 -c copy "output_segment_%~n1.mp4"

REM Create PNG frames from the extracted video segment
echo Creating PNG files...
ffmpeg.exe -i "output_segment_%~n1.mp4" -vf fps=1 "output_frame_%~n1_%%03d.png"

REM Process PNG files with Tesseract to extract text
echo Generating TXT files from PNG frames...
for %%i in (output_frame_%~n1_*.png) do tesseract "%%i" "%%~ni"

REM Combine all TXT files into one big TXT file
echo Combining all TXT files into one file...
type output_frame_%~n1_*.txt > combined_output_%~n1.txt

REM Clean up temporary files
echo Cleaning up PNG and intermediary TXT files...
del output_frame_%~n1_*.png
del output_frame_%~n1_*.txt
del "output_segment_%~n1.mp4"

echo Process completed. Combined TXT file created: combined_output_%~n1.txt.

```

---

### 6. Installing Prerequisite Programs

#### 6.1. Installing ffmpeg.exe
1. **Download FFmpeg**:
   - Visit the [official FFmpeg builds page](https://ffmpeg.org/) or a trusted source like [gyan.dev](https://www.gyan.dev/ffmpeg/).
   - Download the latest release build (usually the 64-bit static version).

2. **Extract the Files**:
   - Use [7-Zip](https://www.7-zip.org/) or a similar tool to extract the downloaded `.7z` file.
   - Extract the contents to a folder, e.g., `C:\ffmpeg`.

3. **Add FFmpeg to the System PATH**:
   - Open the Start Menu and search for **Environment Variables**.
   - Click **Edit the system environment variables**.
   - In the System Properties window, click the **Environment Variables** button.
   - Under **System Variables**, find the `Path` variable, then click **Edit**.
   - Add the path to the `bin` folder inside your FFmpeg directory (e.g., `C:\ffmpeg\bin`).
   - Click **OK** to save.

4. **Verify Installation**:
   - Open the Command Prompt (MS-DOS) and type:
     ```cmd
     ffmpeg -version
     ```
   - If installed correctly, you’ll see FFmpeg’s version and configuration details.

---

#### 6.2. Installing tesseract.exe
1. **Download Tesseract**:
   - Visit the [Tesseract GitHub page](https://github.com/tesseract-ocr/tesseract) or the [UB Mannheim Tesseract page](https://github.com/UB-Mannheim/tesseract/wiki) for pre-built binaries.
   - Download the latest stable version of Tesseract for Windows.

2. **Install Tesseract**:
   - Run the installer (`.exe` file).
   - During installation:
     - Choose the installation directory (e.g., `C:\Program Files\Tesseract-OCR`).
     - Select the language data files you need (e.g., English).

3. **Add Tesseract to the System PATH**:
   - Open the Start Menu and search for **Environment Variables**.
   - Click **Edit the system environment variables**.
   - In the System Properties window, click the **Environment Variables** button.
   - Under **System Variables**, find the `Path` variable, then click **Edit**.
   - Add the path to the Tesseract directory (e.g., `C:\Program Files\Tesseract-OCR`).

4. **Verify Installation**:
   - Open the Command Prompt (MS-DOS) and type:
     ```cmd
     tesseract --version
     ```
   - If installed correctly, you’ll see the Tesseract version and configuration details.

5. **Test Tesseract**:
   - Place an image file (e.g., `example.png`) in the same directory as your Command Prompt.
   - Run the following command to extract text from the image:
     ```cmd
     tesseract example.png output
     ```
   - A file named `output.txt` will be created with the extracted text.

---
