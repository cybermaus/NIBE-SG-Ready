# NIBE-SG-Ready
ESPHome control board driving NIBE SMO S40 SG ready state

Drives Enhanced state during low-price windows
Provides button to Home Assistant (or other interface) to also
drive Surplus state during plentiful sun hours.

Initially I had a joined pump PWM drive and SG Ready device, but now 
SG Ready is kept on its own device, separate from the
pump drive, so a pump-side bug or reboot can never move the SG contacts.
Also it means the board can live inside the NIBE SMO S40, without any wires outside
and importantly, without any wires that are also connected to 230V devices.

SG Ready contacts, ON = contact closed (official levels):
   Blocked (1,0)  Normal (0,0)  Enhanced (0,1)  Surplus (1,1)

Additional AUX switch functions are to be determined what is useful. For now I take
"More hot water" and "Away mode", but they are also on the RMU40 thermostat
Maybe replace later with "Allow cooling". I just seemed a waste not to add 
a few option switches.

"SG Ready mode" select:
   Auto      1. HA Surplus switch ON -> Surplus
             2. else Day cycle: Enhanced inside the two usually low price windows, Normal outside
             3. Day cycle off or time not valid yet -> Normal
   for all other modes (Normal / Blocked / Enhanced / Surplus)
             1. Manual override, forced. The HA Surplus switch and Day cycle are both ignored.

HA Surplus: HA must turn it ON again at least every 10 minutes, else it
drops itself after ${surplus_timeout}. It is not restored after a reboot.
It is to be expected HA will switch this on during the occasional winter sun, 
usually between 10:00-16:00 on the rare sunny days, but that logic is at HA logic, not here.

"Current mode" text sensor shows what the contacts are really set to.

Day cycle windows (hidden config, defaults): Enhanced 22:00-06:30 and
09:30-17:00 default, but settable. Code exist to handle midnight transition inside a window.
The timer is on a minute interval, so is unlike to happen exactly at the 00 second mark.

Everything defaults to the safe state at boot: contacts (0,0) = Normal.
Built with ESPHome 2026.7.2, ESP12F (esp12e).


ESPHome preview

<img width="702" height="890" alt="image" src="https://github.com/user-attachments/assets/3faca533-91b5-45c9-8c83-84d9ea2f6f0b" />


HA device preview

<img width="420" height="727" alt="image" src="https://github.com/user-attachments/assets/ca3abb96-2416-4d7a-949e-b6f9e1e498b3" />


HA automation rule example preview

<img width="616" height="898" alt="image" src="https://github.com/user-attachments/assets/f225ae1f-9db9-434e-9f14-0bf8c3393c95" />


HA dashboard example preview

<img width="511" height="562" alt="image" src="https://github.com/user-attachments/assets/af08c9da-90e1-4f0d-9c5e-383f11c409e0" />
