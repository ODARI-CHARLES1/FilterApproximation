
![involved-banner.png](attachment:d5bbd29f-41f2-488b-9e3c-728245d82a38.png)

# Butterworth Filter Design Using Python (SciPy, NumPy, Control)

##  Overview

This project demonstrates how to design and analyze an **analog Butterworth filter** using Python libraries such as:

* `scipy.signal`
* `numpy`
* `control` (python-control system library)
* `matplotlib` (for visualization)

The Butterworth filter is known as a **maximally flat magnitude response filter**, meaning it has no ripples in the passband or stopband.

---

##  Objectives

* Understand Butterworth filter approximation
* Compute filter order and cutoff frequency
* Design analog low-pass filter
* Convert transfer function into frequency response
* Visualize magnitude and phase response
* Optionally convert to digital filter using bilinear transform

---

##  Theory (Butterworth Approximation)

The magnitude response of a Butterworth filter is:

$[
|H(j\omega)|^2 = \frac{1}{1 + (\frac{\omega}{\omega_c})^{2n}}
]$

Where:

* $( \omega_c ) = cutoff frequency$
* ( n ) = filter order
* Flat response in passband

###  Key properties:

* No ripples in passband/stopband
* Smooth monotonic response
* Controlled roll-off: ( -20n , dB/decade )

---

##  Required Libraries

```bash
pip install numpy scipy matplotlib control
```

---

## 🧪 Implementation Steps

### 1. Import Libraries

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy import signal
import control as ctrl
```

---

### 2. Define Filter Specifications

```python
# Specifications
rp = 1        # passband ripple (not used in Butterworth but kept for comparison)
rs = 40       # stopband attenuation (dB)
wp = 1000     # passband frequency (rad/s)
ws = 2000     # stopband frequency (rad/s)
```

---

### 3. Compute Butterworth Order

```python
N, Wn = signal.buttord(wp, ws, rp, rs, analog=True)
print("Filter Order:", N)
print("Cutoff Frequency:", Wn)
```

---

### 4. Design Transfer Function

```python
b, a = signal.butter(N, Wn, btype='low', analog=True)
system = signal.TransferFunction(b, a)
print(system)
```

---

### 5. Frequency Response (Bode Plot)

```python
w, mag, phase = signal.bode(system)

plt.figure()
plt.semilogx(w, mag)
plt.title("Butterworth Filter Magnitude Response")
plt.xlabel("Frequency (rad/s)")
plt.ylabel("Magnitude (dB)")
plt.grid(True)
plt.show()
```

---

### 6. Using Control Library (Alternative Representation)

```python
sys_ctrl = ctrl.TransferFunction(b, a)

mag, phase, omega = ctrl.bode(sys_ctrl, dB=True, Plot=True)
```

---

##  Optional: Digital Conversion

```python
fs = 10000  # sampling frequency
bz, az = signal.bilinear(b, a, fs=fs)

print("Digital Numerator:", bz)
print("Digital Denominator:", az)
```

---

##  Expected Results

* Smooth magnitude response
* No ripples in passband
* Sharp attenuation after cutoff frequency
* Increasing order → steeper roll-off

---

##  Engineering Insight

In real systems (communications, audio, power electronics):

* Butterworth filters are used when **signal smoothness matters more than sharp cutoff**
* Common in:

  * Audio filtering
  * Sensor signal conditioning
  * Anti-aliasing filters in ADC systems

---

## Extensions

You can extend this project by:

* Comparing Butterworth vs Chebyshev vs Elliptic
* Implementing bandpass/bandstop filters
* Designing real PCB analog filters
* Simulating noise rejection in communication systems

---

## Author Notes

This project is suitable for:

* Electrical & Electronic Engineering coursework
* DSP/filter design labs
* Control systems and signal processing practice

