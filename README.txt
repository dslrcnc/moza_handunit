# MOZA HANDUNIT
SPI protocol used in the NRF24L01 of the MOZA iFocus Handunit 

Using a handunit and a logic analyzer I found the next SPI commands sent to the NRF24L01 that may enable future comunication with the moza iFocus motors. 

at the Start of the power on the controller sent 

0x2A receive data address pipe 0 
0x34 
0x43 
0x10 
0x10 
0x10 
 
then 
 
0x0A read receive data addres pipe 0 
nrf sent 0x34 
nrf sent 0x43 
nrf sent 0x10  
nrf sent 0x10  
nrf sent 0x10  
 
then 

0x2A writes again receive data address pipe 0 for some reason
0x34 
0x43
0x10
0x10
0x10

then

0x30 writes Transmit address
0x34 the address seems the same as the RX address
0x43
0x10
0x10
0x10

then

0x21 writes EN_AA Auto acknowledgement 
0x01 enables auto acknowledgement para pipe 0
then
0x22 (read 00001110) Writes EN_RXADDR enable RX address
0x01 enables it for pipe 0
then
0x24 (read 00001110) writes SETUP_RETR Transmition setup
0x1A 1: wait 500us A: Re Transmit 10 times
then
0x25 (read 00001110) RF channels
0x28 RF channel hex 0x28
then
0x26 (read 00001110) Setup RF register
0x07 no continous transmition, 1mbps, 0dbm
then
0x20 (read 00001110) Writes Configuracion Register
0x0E enable CRC, CRC scheme 2 bytes, POWER UP, PTX, enables pin IRQ for TX RX y MAX_RETX
then
0xE1 (read 00001110) Flush TX FIFO
0xFF
then
0xE2 (read 00001110) Flush RX FIFO
0xFF
then
(CSN is HIGH for some reason but still sent a message)
0x50 maybe this is for another sensor inside the handunit
0x73
then (here CSN goes LOW)
0x3C writes DYNPD enable dynamic payload length
0x01 enable DYNPD for pipe 0
then
0x3D writes Feature register 
0x07 enable Dynamic Payload length, payload with ack, and W_TX_PAYLOAD_NOACK command 
