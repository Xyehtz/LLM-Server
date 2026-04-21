# Considerations for other operating systems
If you are going to use other Operating Systems (from MacOS with Apple Silicon) keep in mind that the way models work with the hardware is slightly different.

In this project I am using Mac and the models will work slightly different from Linux and Windows.

Apple Silicon (Apple's M chips) computers have an architecture called *Unified Memory Architecture* (UMA) which provides a memory pool for the CPU, GPU and Neural Engine which is shared between the three, this means that on M chip computers you will be looking at the RAM and how big of a model can fit in the available RAM rather than looking at the VRAM like you would on Linux and Windows.

If you are going to follow the instructions of this project on Linux/Windows make sure to look at the VRAM when memory sizes are mentioned, not the RAM.