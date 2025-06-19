# Example On How To Utilize DRP To Give Registers Values Before The Core Has Initialized #



    // ----------------------------------------------------------------------------
    // COMMUNICATION CORE INITIAL ON-BOARD DEFAULT RX-DATA
    // Sometimes there is a need to perform actions - such as setting DRP-related
    // values before the PCIe core is brought online. This is possible by specify
    // "virtual" COM-core initial transmitted values below.
    // ----------------------------------------------------------------------------
    
    bit [63:0] initial_rx [5] = '{
            // Modify data below to set own actions - examples:
            // - send some initial TLP on core startup.
            // - set initial VID/PID if PCIe core has been modified.
            // - write to DRP memory space to alter the core.
            // replace / expand on dummy values below - for syntax of each 64-bit word
            // please consult sources and also device_fpga.c in the LeechCore project.
            64'h00000000_00000000,
            64'h00000000_00000000,

            // Set x1 width (addr 0x061, data 0x01 for bits [5:0])
            64'h0100_3F00_0061_2377,
        
            // Set Gen1 speed (addr 0x025, data 0x10 for bits [13:10])
            64'h0010_00F0_0025_2377,

            // Bring the PCIe core online from initial hot-reset state. This is done by
            // setting control bit in PCIleech FIFO CMD register. This should ideally be
            // done after DRP&Config actions are completed - but before sending PCIe TLPs.
            64'h00000003_80182377
        };
        
