frequency modulation


- Intro + Aim 
- FM vs AM
- Derivation of FM
- FM variables
- Success Criteria
- Assumptions


```
import numpy as np
import matplotlib.pyplot as plt
from scipy.fft import fft

# Constants
f1 = 261.63  # Frequency of the first cosine component
f2 = 329.63  # Frequency of the second cosine component
f3 = 392.00  # Frequency of the third cosine component
f_signal = 1000  # Frequency of the sine component

# Sampling
fs = 120000  # Sampling frequency, >2*max(f_signal, f1, f2, f3, 60kHz) to satisfy Nyquist
T = 1/fs  # Sampling period
L = 1  # Length of signal in seconds
N = int(fs * L)  # Number of samples

t = np.linspace(0, L, N, endpoint=False)  # Time vector

# f(t) function
f_t = np.sin(2*np.pi*f_signal*t + 5e3*(-(np.cos(2*np.pi*f1*t)/(2*np.pi*f1)) - (np.cos(2*np.pi*f2*t)/(2*np.pi*f2)) - (np.cos(2*np.pi*f3*t)/(2*np.pi*f3))))

#Modulation index is the 5e3 tingie

# Compute FFT
F_f_t = fft(f_t)
freqs = np.fft.fftfreq(N, T)[:N//2]
magnitude = 2/N * np.abs(F_f_t[:N//2])

# Filter magnitudes below 0.01
filtered_freqs = freqs[magnitude > 0.01]
filtered_magnitude = magnitude[magnitude > 0.01]

# Plot the magnitude of the FFT for magnitudes > 0.01
plt.figure(figsize=(10, 6))
plt.plot(filtered_freqs, filtered_magnitude)
plt.title('FFT of f(t) with Magnitude > 0.01')
plt.xlabel('Frequency (Hz)')
plt.ylabel('Magnitude')
plt.xlim(0, 7500)  # Limiting to 60kHz
plt.grid(True)
plt.show()
