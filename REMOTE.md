# RDP vs VNC
Between **RDP (Remote Desktop Protocol)** and **VNC (Virtual Network Computing)**, **RDP** typically consumes **lower bandwidth**.

Here’s why:

### 1. **RDP (Remote Desktop Protocol)**:
   - RDP is optimized for remote desktop access, particularly over slower network connections. It uses a more efficient compression and data transmission method, adjusting the quality based on the available bandwidth.
   - RDP transmits only screen changes (rather than sending the entire screen image) and uses techniques like caching and compression to minimize bandwidth usage.
   - It also can transmit vector graphics and text efficiently, so it works well for displaying business applications and simple graphical environments.
   - **Overall**, RDP is more bandwidth-efficient and provides better performance on low-bandwidth connections.

### 2. **VNC (Virtual Network Computing)**:
   - VNC is simpler and generally works by sending pixel-by-pixel updates of the entire screen, which can be more bandwidth-intensive, especially in high-resolution displays or when running graphical applications.
   - While VNC can be configured with various encodings and compression options (e.g., TightVNC, UltraVNC), it still tends to use more bandwidth compared to RDP because it lacks the sophisticated optimizations RDP uses to handle screen updates.
   
### Summary:
- **RDP** tends to be more bandwidth-efficient, particularly on slower connections, because of its optimized compression, data transmission strategies, and selective screen updates.
- **VNC** can use more bandwidth, especially for graphical updates, as it transmits more raw pixel data.

If bandwidth is a primary concern, **RDP** would generally be the better choice.

---
The previous section was written by ChatGPT.
