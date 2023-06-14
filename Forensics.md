# CDDC 2023 - Forensics Challenges

## #1: Audio steganography (100 pts)

### Description

<img width="794" alt="Screenshot 2023-06-14 at 14 21 28" src="https://github.com/hollowcrust/CDDC2023/assets/72879387/6f48fb8b-651d-4b9b-b880-086073e4c5fe"><br/>

### Solution

  - Usually audio steganography involves embedding data in Least Significant Bits of the file. So I wrote a script that combines all LSB in the file and print them out.

### LSB script
```Python
import wave

cover_wav = wave.open("problem.wav", mode='rb')
frames = bytearray(cover_wav.readframes(cover_wav.getnframes()))

message_bits = [int(f)%2 for f in frames ]

message_ex = []
value = 0
for i in range(len(message_bits)):
    if i%8==0 and i!=0:
        message_ex.append(value)
        value = 0
    value |= message_bits[i] << (7-i)%8

print(''.join([chr(l) for l in message_ex]))
```
<br/>
<img width="307" alt="Screenshot 2023-06-14 at 14 35 29" src="https://github.com/hollowcrust/CDDC2023/assets/72879387/6e84c8d7-f5b2-45c5-8dbb-8292885dc8f2">

### Flag
`CDDC2023{tH15_15_AuD10_5t39aNo9rAphy}`

<br/>

## #2: Obtaining Confidential Files (50 pts)

### Description

<img width="799" alt="Screenshot 2023-06-14 at 14 10 26" src="https://github.com/hollowcrust/CDDC2023/assets/72879387/fcb7628b-37c7-4336-adb8-653f766d45d6"><br/>

### Solution
  - Download and open the attached `Confidential.xlsx` then go to `File -> Properties... -> Content`, there are 2 sheets `Sheet1` and `Sheet2` but only `Sheet1` is visible.<br/>
<img width="1440" alt="Screenshot 2023-06-14 at 14 13 17" src="https://github.com/hollowcrust/CDDC2023/assets/72879387/f2025c0b-c7d5-4345-b895-9baabf9e9eee"><br/>
  - Right-click on `Sheet1` at the bottom-left corner then select `Unhide... -> Sheet2 -> OK`. `Sheet2` is now visible with the flag value. <br/>
<img width="295" alt="Screenshot 2023-06-14 at 14 15 08" src="https://github.com/hollowcrust/CDDC2023/assets/72879387/b8a0f0a4-ce85-4248-9f15-a0056235f49c">
<img width="448" alt="Screenshot 2023-06-14 at 14 16 49" src="https://github.com/hollowcrust/CDDC2023/assets/72879387/076918f2-084e-44b4-9faa-3a2e4c0ce66c"><br/>

### Flag
`CDDC2023{M1cr050ft_0ff1c3_D0cum3Nt_15_00xML_StRuctur3}`

<br/>

## #3: Owner of the dog chew (50 pts)

### Description

<img width="793" alt="Screenshot 2023-06-14 at 14 36 44" src="https://github.com/hollowcrust/CDDC2023/assets/72879387/ad0f98fb-b606-4399-9625-fb90fccaf8c6">

### Solution

  - We are given an image of a piece of bone named `cddc2023_image.png`.<br/>
![cddc2023_image](https://github.com/hollowcrust/CDDC2023/assets/72879387/b515c9e7-65fc-4738-bda8-f6d4ab113f4d)<br/>
  - Running `strings cddc2023_image.png` gives `pw: i'm hungry` on the last line.
  - Running `binwalk --dd='.*' cddc2023_image.png` will extract an embedded a ZIP file.
  - Extract the ZIP file using the password `i'm hungry` gives an image named `cddc2023_puppy.jpg`.
  - Running `strings cddc2023_puppy.jpg` gives the flag on the last line.

### Flag
`CDDC2023{H4pPy_puPpy}`
