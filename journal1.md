## Journal for the week of Sept. 25 - Oct. 1 2023
# Work on the "Dependable" Project
I began implementing the NDP protocol. I wrote functions to handle incoming and outgoing NDP-ICMP packets, functions to handle next-hop determination for a destination IP address, and implemented the data structures for the neighbor cache, router list, address list and destination cache. I wrote modules for an interface abstraction to transmit packets from and a lightweight socket module for the application layer. I wrote a small driver for the stack and I wrote an integration test using all of these components to receive UDP packets from another host on my local network and observe that the host enters the stack's neighbor cache and vice-versa.

In the coming week I intend to refactor the NDP code I have already written for simplicity, maintainability and correctness. I intend to port the platform-specfic code in the codebase to the Odroid-C4, write a driver for the hardware Random Number Generator of the same, and begin the research for writing an Ethernet Network Interface Controller (NIC) driver by studying the [driver](https://github.com/lucypa/sDDF) written by Lucy Parker for the NXP imx8mm board and studying the chapter on Network Device Drivers in [Linux Device Drivers](https://static.lwn.net/images/pdf/LDD3/ch17.pdf) [Corbett, Rubini, Hartman].

# Work on Desktop Application
To date I have implemented proposed features including:
- Nametable (tilemap) editor,
- Tile selector interface,
- Switch between banks A and B of pattern-table ROM,
- Pixel editor for nametable,
- Export nametable to 6502 ASM, C Code, or C Code with Run-Length Encoding (RLE),
- Rectangular selection on the tilemap, middle-click paste, and fill selection,
- Picture Processing Unit (PPU) color channel emphasis masking to observe the nametable under different color emphases.
