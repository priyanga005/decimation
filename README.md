import numpy as np
import matplotlib.pyplot as plt
from scipy import signal

# Create a sample signal (sinusoidal) for demonstration
fs = 1000  # Original sampling rate (Hz)
t = np.arange(0, 1, 1/fs)  # Time vector (1 second duration)
x = np.sin(2 * np.pi * 50 * t) + 0.5 * np.sin(2 * np.pi * 150 * t)  # Signal with two frequencies

# Decimation factor
M = 4  # Decimation factor

# Step 1: Low-pass filter (anti-aliasing filter)
# Design a low-pass filter with a cutoff frequency below Nyquist of the downsampled signal
nyquist = fs / 2
cutoff = nyquist / M  # Choose cutoff frequency

# Design a low-pass filter using Butterworth filter design
order = 4  # Filter order
b, a = signal.butter(order, cutoff / nyquist, btype='low')

# Apply the filter to the signal
x_filtered = signal.filtfilt(b, a, x)

# Step 2: Downsample the signal by a factor of M
x_decimated = x_filtered[::M]  # Take every Mth sample

# Plotting the results
plt.figure(figsize=(12, 6))

# Original signal
plt.subplot(3, 1, 1)
plt.plot(t, x)
plt.title("Original Signal")
plt.xlabel("Time [seconds]")
plt.ylabel("Amplitude")

# Filtered signal
plt.subplot(3, 1, 2)
plt.plot(t, x_filtered)
plt.title("Filtered Signal (Anti-aliasing)")
plt.xlabel("Time [seconds]")
plt.ylabel("Amplitude")

# Decimated signal
t_decimated = np.arange(0, len(x_decimated)) * M / fs  # Time vector for decimated signal
plt.subplot(3, 1, 3)
plt.plot(t_decimated, x_decimated)
plt.title(f"Decimated Signal (Factor M={M})")
plt.xlabel("Time [seconds]")
plt.ylabel("Amplitude")

plt.tight_layout()
plt.show()
