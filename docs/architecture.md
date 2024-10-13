# KeySync Project Architecture

## Overview
The **KeySync** project facilitates the remote synchronization of keystrokes between two machines: **Point A** and **Point B**. When keystrokes are entered on **Point B**, they are transferred over a network and injected into the **X server** on **Point A**, making it appear as though the keystrokes originated locally. This allows remote control of **Point A's** graphical environment via **Point B**'s keyboard.

## Components

### 1. Proc A (Key Input Handler)
- **Purpose**: Captures keyboard input on **Point B**.
- **Responsibilities**:
  - Listen for keystrokes.
  - Send captured keystrokes to the Networking Module.

### 2. Proc B (Screen Output Handler)
- **Purpose**: Handles feedback from **Point A's** screen to ensure proper rendering of the remote actions.
- **Responsibilities**:
  - Process and display feedback on **Point B**.

### 3. Networking Module
- **Purpose**: Facilitates secure communication between **Point A** and **Point B** over the network.
- **Responsibilities**:
  - Serialize and transmit keystroke data.
  - Ensure reliability and encryption of data (via the Security Module).
  - Handle retransmission in case of packet loss or delay.

### 4. XHandler (X Server Injector)
- **Purpose**: Injects the keystrokes received at **Point A** into the local X server.
- **Responsibilities**:
  - Convert received keycodes into X server-compatible events.
  - Handle injection of key press/release events.

### 5. Security Module
- **Purpose**: Provides encryption and decryption for keystroke data transferred between **Point A** and **Point B**.
- **Responsibilities**:
  - Encrypt outgoing data from **Point B**.
  - Decrypt incoming data at **Point A**.
  - Ensure secure communication to prevent eavesdropping.

### 6. Event Queue
- **Purpose**: Buffers and queues keystroke events before they are injected into **Point A's** X server.
- **Responsibilities**:
  - Ensure smooth injection of keystrokes, preventing event loss.
  - Maintain proper typing speed and event ordering.

### 7. State Sync Module
- **Purpose**: Maintains synchronization between **Point A** and **Point B**'s processes.
- **Responsibilities**:
  - Handle state consistency between both points (e.g., connection status).
  - Synchronize the states in case of connection interruptions.

### 8. Monitoring Component
- **Purpose**: Logs and tracks the system's performance and network reliability.
- **Responsibilities**:
  - Log keystroke transmission times and network delays.
  - Report issues such as packet loss or high latency.

## Relationships and Data Flow

1. **Proc A** captures keystrokes from **Point B's** keyboard and sends them to the **Networking Module**.
2. The **Networking Module** securely transmits the keystrokes to **Point A**, assisted by the **Security Module** for encryption and decryption.
3. At **Point A**, the **XHandler** injects the keystrokes into the **X server**.
4. The **X server** processes the keystrokes, providing graphical feedback via the **Screen Output**.
5. The **Monitoring Component** logs the process for debugging and performance monitoring.

## Future Considerations

- **Error Handling**: Ensure the system can gracefully handle network interruptions and incorrect key mappings.
- **Optimizations**: Explore latency improvements for real-time input feedback.
- **Cross-Platform Support**: Investigate possible expansions to non-X11 environments for greater flexibility.
