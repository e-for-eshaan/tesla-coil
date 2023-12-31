# Wireless Power Transmission and Data Communication using Slayer Exciter Circuits

This project aims to demonstrate a novel approach to wireless power transmission and data communication using Slayer exciter circuits. The project utilizes the principles of electromagnetic induction to transmit power wirelessly, while simultaneously encoding and transmitting data through high-frequency AC signals. The system incorporates an encoding scheme based on Hamming (7,4) code to enhance resilience to noise and ensure accurate data retrieval.

## System Components

The project utilizes the following components:

- Slayer Exciter Circuit: The Slayer exciter circuit consists of an air-cored transformer and a transistor acting as a switch. It converts a low DC input voltage into a high-frequency, high-voltage AC output, generating electromagnetic waves.

- Raspberry Pi: A Raspberry Pi microcomputer is used for data encoding and modulation. It receives user input, encodes it using the Hamming (7,4) code, and drives the modulating circuit.

- Modulating Circuit: The modulating circuit employs a MOSFET or relay to modulate the carrier wave generated by the Slayer exciter circuit. The Raspberry Pi controls the switching of the MOSFET/relay to encode the digital data as bursts of electromagnetic waves.

- Hamming (7,4) Encoding: The Hamming (7,4) code is used to add parity bits to the 4 message bits, enhancing the system's resilience to noise and enabling error detection and correction.

## Working Principle

1. Wireless Power Transmission: The Slayer exciter circuit generates high-frequency AC signals, creating electromagnetic waves around its secondary coil. These waves can power devices within their vicinity, such as fluorescent lamps, by exciting the mercury atoms within the lamp.

2. Data Encoding and Modulation: The Raspberry Pi receives user input and encodes it using the Hamming (7,4) code. The encoded message is then modulated onto the carrier wave generated by the Slayer exciter circuit. Amplitude Shift Keying (ASK) is employed, where the ON state of the MOSFET/relay represents a binary 1, and the OFF state represents a binary 0.

3. Data Transmission and Reception: The modulated signal, consisting of bursts of electromagnetic waves, carries the encoded data. A loop of wire connected to an oscilloscope can detect and visualize the waveform of the modulated signal. The receiver can demodulate the received signal, decode the Hamming (7,4) code, and retrieve the original data.

## Implementation

To implement this project, follow these steps:

1. Set up the Slayer exciter circuit with the primary and secondary coils.
2. Connect the Raspberry Pi to the modulating circuit and configure the GPIO pins.
3. Write the encoding and modulation code on the Raspberry Pi.
4. Run the code and provide user input for data transmission.
5. Use a loop of wire connected to an oscilloscope to visualize the modulated signal.
6. Set up a receiver circuit to demodulate the received signal and decode the Hamming (7,4) code to retrieve the transmitted data.

## Conclusion

The wireless power transmission and data communication system using Slayer exciter circuits provide a unique approach to simultaneously transmit power and data. By incorporating Hamming (7,4) encoding, the system ensures robustness against noise and enables error detection and correction. This project opens up possibilities for various applications, including wireless charging, remote sensing, wireless communication, and Li-Fi.

Please refer to the code snippets in the respective sections for the detailed implementation of the project.
