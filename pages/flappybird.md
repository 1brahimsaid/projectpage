# Flappy Bird with EMG-Controlled Mouse 🐦


<p align="center">
  <img src="/img/setup.jpeg"/>
</p>

## What is this?

Modified a computer mouse so that its left button is operated by the electromyographic (EMG) signal. This signal is produced when the EMG electrodes are connected on the left forearm, so that left-clicks would be produced by closing the hand. To verify and test this system video game, Flappy Bird, was used to test the output.

## Block Diagram

<p align="center">
  <img src="/img/blockdiagram.jpeg"/>
</p>

1. Three **electrode leads** are  connected from the **forearm muscles** to an **instrumentation amplifier** incorporating a circuit for common-mode
rejection.
2. The signal will then be **band-pass filtered** in accordance with the EMG signal bandwidth. Since the EMG signal is not rhythmic in nature, typical signal processing usually consists in **rectification** and **integration**.
3. A **comparator with hysteresis** will then compare the integrator output to a threshold before triggering a **monostable** circuit that will send a pulse through an **optoisolator** to a **relay** that will activate the **mouse left button** while playing a game of _Flappy Bird_.

## Requirements

- Six **AA Batteries** 
  - Four used Batteries to Generate **(+3V -3V)** for all the Circuits
  - Two Batteries for **+5V** to the **Mouse**
  - This is done to make sure electrical circuits are isolated from anything that connects to the **human body**.
- **Mouse**
- **Relay**
- **Optoisolator** (Transfers Electric Signals from the Body to System while isolating the user from harm)
- **Instrumentation Amplifier** (AD620A Instrumentation Amplifier and an OP97 Operational Amplifier)
<p align="center">
  <img src="/img/instrumentalamplifier.jpeg"/>
</p>
- **Band-Pass Filter** (EMG Signal Filter/Envelope Detector)
- **Rectifier/Integrator** (EMG Signals are not Rythmic, Rectification and Integration is needed)
- **Comparator** (A circuit that triggers when forearm intensity reaches threshold)
- **Monostable** (Turns Comparator Signal into well-defined pulses or discrete values)

...return to [`projects.ibrahimsaid.ca`](https://projects.ibrahimsaid.ca/) or [`ibrahimsaid.ca`](https://www.ibrahimsaid.ca/).