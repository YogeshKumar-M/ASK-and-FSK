# ASK AND FSK
# Aim
Write a simple Python program for the modulation and demodulation of ASK and FSK.
# Tools required
Python colab.
# Program
ASK
```asm
import numpy as np
import matplotlib.pyplot as plt

# -----------------------------
# Parameters
# -----------------------------
fs = 1000          # sampling frequency
bit_duration = 1   # duration of one bit
fc = 5             # carrier frequency

data = [1,0,1,1,0,1]   # message bits

t = np.arange(0, bit_duration, 1/fs)

# -----------------------------
# Message Signal
# -----------------------------
message = []

for bit in data:
    message.extend([bit]*len(t))

message = np.array(message)

# -----------------------------
# Carrier Signal
# -----------------------------
time = np.arange(0, len(message))/fs
carrier = np.sin(2*np.pi*fc*time)

# -----------------------------
# ASK Modulation
# -----------------------------
ask_signal = message * carrier

# -----------------------------
# ASK Demodulation
# (Envelope Detection)
# -----------------------------
demodulated = np.abs(ask_signal)

# simple threshold detection
threshold = 0.5
reconstructed = (demodulated > threshold).astype(int)

# -----------------------------
# Plotting
# -----------------------------
plt.figure(figsize=(12,8))

# Message
plt.subplot(4,1,1)
plt.plot(time, message)
plt.title("Message Signal")
plt.ylim(-0.5,1.5)
plt.grid()

# Carrier
plt.subplot(4,1,2)
plt.plot(time, carrier)
plt.title("Carrier Signal")
plt.grid()

# ASK Modulated
plt.subplot(4,1,3)
plt.plot(time, ask_signal)
plt.title("ASK Modulated Signal")
plt.grid()

# Demodulated
plt.subplot(4,1,4)
plt.plot(time, reconstructed)
plt.title("Demodulated (Reconstructed) Signal")
plt.ylim(-0.5,1.5)
plt.grid()

plt.tight_layout()
plt.show()

```
FSK
```asm
import numpy as np
import matplotlib.pyplot as plt

# -----------------------------
# Parameters
# -----------------------------
fs = 1000          # sampling frequency
bit_duration = 1
f1 = 8             # frequency for bit 1
f0 = 2             # frequency for bit 0

data = [1,0,1,1,0,1]

t = np.arange(0, bit_duration, 1/fs)

# -----------------------------
# Message Signal
# -----------------------------
message = []

for bit in data:
    message.extend([bit]*len(t))

message = np.array(message)
time = np.arange(0, len(message))/fs

# -----------------------------
# Carrier Signals
# -----------------------------
carrier1 = np.sin(2*np.pi*f1*time)   # for bit 1
carrier0 = np.sin(2*np.pi*f0*time)   # for bit 0

# -----------------------------
# FSK Modulation
# -----------------------------
fsk_signal = []

for bit in data:
    if bit == 1:
        signal = np.sin(2*np.pi*f1*t)
    else:
        signal = np.sin(2*np.pi*f0*t)
    fsk_signal.extend(signal)

fsk_signal = np.array(fsk_signal)

# -----------------------------
# FSK Demodulation (Simple Detection)
# -----------------------------
demodulated = []

samples_per_bit = len(t)

for i in range(0, len(fsk_signal), samples_per_bit):
    segment = fsk_signal[i:i+samples_per_bit]

    # correlate with both frequencies
    ref1 = np.sin(2*np.pi*f1*t)
    ref0 = np.sin(2*np.pi*f0*t)

    energy1 = np.sum(segment * ref1)
    energy0 = np.sum(segment * ref0)

    if energy1 > energy0:
        demodulated.extend([1]*samples_per_bit)
    else:
        demodulated.extend([0]*samples_per_bit)

reconstructed = np.array(demodulated)

# -----------------------------
# Plotting
# -----------------------------
plt.figure(figsize=(12,8))

# Message
plt.subplot(4,1,1)
plt.plot(time, message)
plt.title("Message Signal")
plt.ylim(-0.5,1.5)
plt.grid()

# Carrier (show one reference)
plt.subplot(4,1,2)
plt.plot(time, carrier1)
plt.title("Carrier Signal (Example Frequency)")
plt.grid()

# FSK Modulated
plt.subplot(4,1,3)
plt.plot(time, fsk_signal)
plt.title("FSK Modulated Signal")
plt.grid()

# Demodulated
plt.subplot(4,1,4)
plt.plot(time, reconstructed)
plt.title("Demodulated (Reconstructed) Signal")
plt.ylim(-0.5,1.5)
plt.grid()

plt.tight_layout()
plt.show()


```
# Output Waveform
ASK
<img width="1270" height="843" alt="image" src="https://github.com/user-attachments/assets/6115d4ad-8ca0-44c6-80b4-c9e24de961da" />

FSK

<img width="1274" height="846" alt="image" src="https://github.com/user-attachments/assets/64d468e8-4cf6-4418-9a60-70175a2bd8d4" />


# Results
Thus the program for ASK and FSM is done using Phyton.
