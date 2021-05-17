# ovenCtrl
LabView code to control "Omega iSeries temperature controller (CNi16D)" and log time-series temperature values via InfluxDB.

<a id ="A1"> </a>
### Problem Statement
To run our experiment, we need a constant flux of sodium vapor. The vapor pressure needs to be high enough to trap enough sodium atoms such that they could be cooled to a Bose-Einstein Condensate. To achieve this, we heat chunks of solid sodium in a crucible in our vacuum system. Below are two imporatant requirements for this process
<ul>
<li>The temperature needs to be within a range of 5 C, around 250 C. A lower temperature will not produce enough sodium flux. A higher temperature will coat the vacuum windows restricting optical access to the system.</li>
<li>The temperature needs to be ramped at a rate of 1 C/min to avoid vacuum leaks in our UHV system.</li>
</ul>
Additionally, we would ideally want to automate the oven turn-on process such that the experiment is ready before the start of the day. 

### Solution
We heat the sodium crucible with resistive heater tapes wound outside the vacuum system. Variacs which are voltage-adjustable sources of alternating current, are used to power the heater tapes. The voltage on these variacs can be manually controlled by turning a knob. The previous turn-on procedure involved gradually turning this knob while manually ensuring that the above two requirements were fulfilled. However, since we wish to automate the process, we decided to run the variacs at full voltage but turn the current on/ off via a solid-state relay. An "Omega iSeries temperature controller (CNi16D)" is used to control the relay. These controllers provide a PID feedback control loop that takes the crucible temperature as its process variable.

### Detailed Working
Here are a list of the hardware components.

| Function | Developer | Part Number | Description |
| --- | --- | --- | --- |
| Variable Voltage Source | Staco Energy Products | 3PN10108 | Output: 140V, 10 AMP, 1.4 KVA |
| Solid State Relay | Developer | Part Number | Description |
| Relay Controller | Omega | CNiD16 | PID control loop |

<ul>
    <li>The omega controller is connected to the local network via ethernet cables.</li>
    <li>The omega controller is commanded and monitored remotely via LabView codes. Omega controllers use TCP/IP transmission protocol to send/ recieve data.</li>
    <li>The time series temperature values are stored via a time series database manager (TSDM). These values could be viewed via the web application 'Grafana' which provides an interactive visualization tool.</li>
</ul>

<table><tr><td><img src='/figure/ovenControl_1.png' alt="Drawing" width="400" height="400" ></td><td><img src='/figure/ovenControl_2.png' alt="Drawing" width="600" height="400" ></td></tr></table>


## Contents
This repo contains the source code, and the main VI *ovenCtrl_Na.vi*.
- *../src/* contains the VI and sub VIs.
- *../ovenCtrl.lvproj* is the labView project file.
- *../src/frontPanel/ovenCtrl_Na.vi* is the main VI to be run.

## Getting Started
- Clone this repository. 
```git clone https://github.com/sbanik1/ovenCtrl <lcl_dir>```
- Modify the sub VI *../src/subVIs/NaOvenTempToInflux.vi* to assign appropriate IP address, InfluxDB database and InfluxDB crendentials. 
- Open *../ovenCtrl.lvproj* and run the code *ovenCtrl_Na.vi*

