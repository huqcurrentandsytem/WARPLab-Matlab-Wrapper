# WARPLab Matlab Wrapper
This is the way I like to use WARPLab with the WARPv3 board. [Checkout the WARP website for more info on WARP.](https://warpproject.org/trac)

This repo has a WARP class that handles most of the details. To use, just put in your directory. 

### How to use the WARP class
Initialize with:
`board = WARP(params);`

The 'params' stuct needs to contain the following:
  `params.nBoards`: Number of boards. Currently, I only use 1 board, so I haven't fully set this up for more boards.
  `params.RF_port`: "A2B" sets RF A as the TX and RF B as RX. "B2A" does the opposite.
  
To send something through the WARP board, simply use the transmit method, `rxData = board.transmit(txData)`. This assumes that we are doing loopback. It'll therefore try to sync the samples and the phase of the rxData to the txData.   

Checkout the example.m to see more. 
  
### More settings
  There are various settings that are set up in the class constructor. These include gain and channel settings. Set these up for your purposes then just run with it.

```matlab
 % RX variables
obj.channels.RX        = 6;
obj.gains.RxGainRF     = 1;  % Rx RF Gain in [1:3] (ignored if USE_AGC is true)
obj.gains.RxGainBB     = 20; % Rx Baseband Gain in [0:31] (ignored if USE_AGC is true)
obj.filters.RX_LPF     = 3;  % [0,1,2,3] for approx ![7.5,9.5,14,18]MHz corner
obj.filters.RX_LPFFine = 2;  % Must be integer in [0,1,2,3,4,5] for [90,95,100,105,110]% scaling to LPF corner frequency
obj.filters.RX_HPF     = 0;  % Must be 0 (HPF corner of 100 Hz) or 1 (default; HPF corner of 30 kHz) This filter setting is only used when RXHP is 'disable' (ie 0)
obj.filters.RX_HP      = 'disable';  % (boolean MODE) MODE: true enables RXHP on the node when in manual gain control false disables RXHP on the node when in manual gain control
            
% TX variables
obj.channels.TX     = 6;
obj.gains.TxGainBB  = 3;  % [0,1,2,3] for approx ![-5, -3, -1.5, 0]dB baseband gain
obj.gains.TXGainRF  = 55; % [0:63] for approx [0:31]dB RF gain
obj.filters.TX_LPF  = 1;  % [1,2,3] for approx [12,18,24]MHz corner frequencies ([24,36,48]MHz bandwidths)
```            
              
