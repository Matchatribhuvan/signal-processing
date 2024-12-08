# signal-processing
This Python code demonstrates how to apply a low-pass filter to a noisy signal in order to remove high-frequency noise. The code generates a noisy sine wave, applies a low-pass filter to smooth the signal, and then plots the original, noisy, and filtered signals for comparison. It uses numpy for numerical calculations and matplotlib for visualization. The low-pass filter is implemented using a windowed-sinc function to calculate the filter coefficients.

What the Code Does

Signal Generation:
The code generates a time vector t ranging from 0 to 1 second, divided into 1000 equally spaced points. A sine wave (signal) with a frequency of 5 Hz is generated using np.sin(2 * np.pi * 5 * t).
Random Gaussian noise is added to the sine wave to create a noisy signal (noisy_signal), simulating real-world data where noise is present.

Low-Pass Filter Definition:
The low_pass_filter function defines the filter and applies it to the noisy signal.

Filter Design: It uses a windowed-sinc function to create the filter coefficients. The sinc function is a mathematical function that is used for designing ideal filters. The np.sinc function creates a filter based on the cutoff frequency and sample rate, and the windowing function (np.hamming) is applied to smooth the coefficients and reduce side lobes (ripples) in the frequency response.

Convolution: The filter is applied to the noisy signal via convolution using np.convolve, which combines the signal with the filter kernel, effectively removing high-frequency noise.

Filter Application:
The low-pass filter is applied to the noisy signal using the specified cutoff frequency and sample rate. The result is stored in the variable filtered_signal.

Plotting:
The code then plots three signals for comparison:
The original signal (pure sine wave).
The noisy signal (original signal with added Gaussian noise).
The filtered signal (the noisy signal after applying the low-pass filter).
The plot is displayed using matplotlib, with appropriate labels and a legend to differentiate the three signals.

What is the Use of the Code?

Noise Reduction: The code demonstrates how to filter out high-frequency noise from a signal, which is a common task in signal processing. The low-pass filter smooths the signal and removes unwanted high-frequency components.

Signal Processing: This code is useful in applications where signals are contaminated with noise, such as audio processing, sensor data analysis, and communications. It helps in extracting the desired low-frequency components of the signal.

Educational Tool: The code can be used to teach concepts of signal processing, especially filtering techniques, and how different types of filters affect a signal.
Concepts That Are Used in the Code

Signal Generation:
Sine Wave: The original signal is a sine wave, which is a fundamental periodic function used in signal processing.
Gaussian Noise: Noise is generated using a normal distribution (np.random.randn), representing random variations typically seen in real-world data.

Low-Pass Filtering:
Windowed-Sinc Filter: The filter is designed using a sinc function, which is ideal for constructing a low-pass filter. The window function (np.hamming) is applied to reduce side effects like ripples in the frequency response.
Convolution: The process of applying the filter to the signal is performed by convolution, which is a standard method in signal processing for filtering operations.
Cutoff Frequency: The cutoff frequency determines which frequency components of the signal will be passed through the filter. Frequencies above the cutoff are attenuated, and those below are retained.

Signal Plotting:
Matplotlib: The signals are visualized using matplotlib, a powerful library for creating static, animated, and interactive plots. It helps in comparing the original, noisy, and filtered signals.
Time Domain Visualization: The signals are plotted in the time domain, allowing for a clear understanding of how the filtering process smooths the noisy signal.
Takeaways

Low-Pass Filter Basics:
A low-pass filter allows low-frequency components of a signal to pass through while attenuating high-frequency noise. This is particularly useful when you need to remove unwanted noise from a signal.

Filter Design:
The code illustrates how a windowed-sinc function is used to design a low-pass filter. The np.sinc function is used for creating the ideal filter response, and a windowing function is applied to smooth the filter coefficients and reduce side effects.

Signal Convolution:
Convolution is a key operation in signal processing that applies the filter to the signal. Understanding how convolution works is essential for implementing custom filters.

Practical Application in Noise Reduction:
The code shows how noise can be effectively removed from a signal using a low-pass filter. This is a fundamental technique in fields such as audio processing, sensor data analysis, and communications.

Visualization of Signal Processing:
Visualizing the original, noisy, and filtered signals is crucial for understanding the effect of the filter. It provides insight into how noise is removed and how the signal is smoothed over time.

Educational Value:
The code can serve as an excellent educational tool for those learning about signal processing. It provides a hands-on demonstration of filtering techniques, showing how theory is applied in practice.

In summary, this code demonstrates the application of a low-pass filter to remove noise from a noisy signal. The process of designing and applying a filter using convolution is clearly shown, and the results are visualized to help understand how the filter affects the signal.



code

import numpy as np
import matplotlib.pyplot as plt

# Generate a noisy signal
t = np.linspace(0, 1, 1000)
signal = np.sin(2 * np.pi * 5 * t)  # Original signal
noise = 0.5 * np.random.randn(1000)  # Gaussian noise
noisy_signal = signal + noise

# Define the low-pass filter function
def low_pass_filter(signal, cutoff_freq, fs):
    # Calculate the filter coefficients using a windowed-sinc function
    num_taps = 101
    taps = np.sinc(2 * cutoff_freq * (np.arange(num_taps) - (num_taps - 1) / 2) / fs)
    window = np.hamming(num_taps)
    taps *= window
    taps /= np.sum(taps)
    
    # Apply the filter using convolution
    filtered_signal = np.convolve(signal, taps, mode='same')
    return filtered_signal

# Apply the low-pass filter to the noisy signal
cutoff_frequency = 5  # Adjust cutoff frequency as needed
sample_rate = 1000  # Adjust sample rate as needed
filtered_signal = low_pass_filter(noisy_signal, cutoff_frequency, sample_rate)

# Plot the original signal, noisy signal, and filtered signal
plt.figure(figsize=(10, 6))
plt.plot(t, signal, label='Original Signal')
plt.plot(t, noisy_signal, label='Noisy Signal')
plt.plot(t, filtered_signal, label='Filtered Signal')
plt.xlabel('Time')
plt.ylabel('Amplitude')
plt.title('Signal Processing: Low-Pass Filtering')
plt.legend()
plt.grid(True)
plt.show()
