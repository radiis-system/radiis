.. _SCPI-label:

SCPI commands
==================================

documentation of the (advanced) usage of RadIIS via SCPI Commands:

.. TODO: Document the connection and (working) serial tools


SYStem:BIAS
-----------
    
Set the SiPM bias, given in [mV]. Higher bias means a higher signal amplification. If a very low bias is set it might be impossible to digitize the signals. It is not possible to choose a bias that damages the system if the enclosure is intact.

The query, SYStem:BIAS?, returns the configured bias voltage in mV.

    | **Command Syntax:** ``SYStem:BIAS <int>``
    | **Parameters:** 24304 mV to 29950 mV
    | **Default:** 27000 mV
    | **Example:** ``SYS:BIAS 26000`` (Sets the bias to 26.00V)
    | **Query Syntax:** ``SYStem:BIAS?``
    | **Return Param:** SiPM bias in mV [int]
    

SYStem:ATC
-----------

Activates or deactivates the **A** utomatic **T** emperature **C** ompensation. If enabled the bias of the SiPM is automatically adjusted to maintain a constant gain during temperature variations. It is very recommended to acctivate the ATC during measurements.   

The query, SYStem:ATC?, returns the state if the ATC.


    | **Command Syntax:** ``SYStem:ATC <bool>``
    | **Parameters:** 0 (OFF) | 1 (ON)
    | **Default:** 1 (ON)
    | **Example:** ``SYS:ATC ON`` (Activates the ATC)
    | **Query Syntax:** ``SYStem:ATC?``
    | **Return Param:** State of the ATC [bool]


SYStem:TEMPerature?
-----------

Query the sensor (SiPM) temperature in mC.

    | **Query Syntax:** ``SYStem:TEMPerature?``
    | **Return Param:** SiPM temperature in mC [int]
    

SYStem:BATtery:LEVEL?
-----------
    
Query the internal battery level in mV [int].

    | **Query Syntax:** ``SYStem:BATtery:LEVEL?``
    | **Return Param:** Internal battery level in mV [int]
        
.. function:: SYStem:COMParator:THReshold <INT>
    
    Set the comparator threshold for the external and internal channel. The number is given in DAC counts. Higher numbers mean a higher signal level that is needed to trigger a detection of a pulse. 
    
    *Maximum = 4095*
    *Minimum = 0*
    *Default = 0*
    
    Example: SYS:COMP:THR 100 

.. function:: SYStem:COMParator:THReshold?
    
    *Returns:* Get the comparator threshold for the external and internal channel. [INT]


.. function:: SYStem:COMPerator:STate?
    
    *Returns:* Get the comparator state (enabled/disabled). [BOOL]


.. function:: SYStem:COMParator:STATE <BOOL>
    
    Enable/disable the comparator

    Example: SYS:COMP ON (Activates the comparator/trigger).

.. function:: SYStem:GATEtime <INT>
    
    Set the system gate time (time to measure the rates) in milliseconds

    *Default = 1000*
    
    Example: SYS:GATE 10000 
    
.. function:: SYStem:GATEtime?
    
    *Returns:* Get the system gate time. [INT]

.. function:: SYStem:Rate?
    
    *Returns:* Get the internal trigger rate in Hz (during the last full gate time). [INT]

.. function:: SYStem:EXRate?
    
    *Returns:* Get the external trigger rate in Hz (during the last full gate time). [INT]
   
.. function:: SYStem:BGRate?
    
    *Returns:* Get the internal background-corrected trigger rate in Hz (during the last full gate time). [INT]

.. function:: MEASurement:START <INT>,<INT>,<INT> 
    
    Start a measurement, arguments are:
    Runtime [ms], Maximum Counts, Channel
    
    Example: MEASurement:START 10000,100000,0 (Start a measurment with max. 10s duration, max. 100k counts using channel 0)

.. function:: MEASurement:STOP

    Stop all running measurements.

.. function:: MEASurement:STate?
    
    *Returns:* Get the current state of the measurement (running/idle) . [BOOL]

.. function:: MEASurement:GET?
    
    *Returns:* Transmit (current) the measurement result. [LIST]

	
.. function:: BACKGround:DATA:SET <BINARY BLOB>
    
    Set the background spectrum (not persistent, will be overwritten by ESP32!) 

    The binary data need to be formatted as following:
    channel0 = byte[0]+byte[1]<<8
    channel1 = byte[2]+byte[3]<<8
    ....
    channel511 = byte[1024]+byte[1025]<<8

    WARNING: The background function are not yet well tested

    Example: BACKG:DATA:SET <BINARY BLOB>

.. function:: BACKGround:DATA:GET?
    
    *Returns:* Transmit the curruntly configured background spectrum. [LIST]
	
    {
		.levels = {"BACKGround", "INFO","SET"},
		.params = {SCPI_DT_INT,SCPI_DT_INT,SCPI_DT_STRING},
		.callback = cmd_BACKG_INFO_SET_cb
	},

.. function:: BACKGround:INFO:GET?
    
    *Returns:* Get information about the currently configured background (Livetime, date, comment). [LIST]

.. function:: SYStem:BOOTLoader
    
    Reset the STM32 measurement controller into DFU bootloader mode for firmware flashing
    See :ref:`flashing-label` for more details.
	
.. function:: SYStem:ESP:FLASHMode
    
    Reset the ESP32 controller into bootloader mode for firmware flashing and relay the USB interface to the ESP.
    See :ref:`flashing-label` for more details.

.. function:: SYStem:ESP:TRANSMode
    
    Relay the USB interface to the ESP32. The interface is closed after 5s of inactivity.

.. function:: SYStem:ESP:Reset
    
    Reset the the ESP32. 

.. function:: SYStem:ESP:RTOFlashmode
    
    Reset the the ESP32 and boot the ESP32 into bootloader mode without starting the USB relay. 


.. function:: SYStem:DEBUGmode <BOOL>
    
    Disable or enable the Debug mode (higher verbosity)

    Example: SYS:DEBUG ON (Enable debug mode)

.. function:: SYStem:DEBUGmode?
    
    *Returns:* Get the state of the debug mode (higher verbosity). [BOOL]

.. function:: SYStem:ACKnowledge?
    
    *Returns:* Answer with "ACK" . [STRING]
