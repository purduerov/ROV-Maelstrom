# PCB-Maelstrom-Application
The application board was intended to contains mission specific electronics including the special manipulators, temperature sensors, pressure sensors, LED drivers, and solenoids for the claw. These components would be grouped together on this board to simplify the more complex microcontroller board with their own chip, the STM 32F405-64 PIN, to provide the interface. 

This board was planned before the mission description, but then we realized that the mission required significantly fewer tools than expected and it was easier to relocate the tooling circuitry to the main microcontroller board. However, a board was still designed for some extra features but it was never tested or implemented due to time constraints. The application board ended up including the LED drivers and the backup temperature sensor’s circuit. We kept the STM32F405-64 pin microcontroller from the original design. It also included a micro-USB for programming the STM chip, a CAN communication chip and some debugging LEDs. The LED driver was modeled to use the LM3409HV FET buck controller.

# LED driver overview
- Controlled by Texas Instruments LM3409HV Buck Controller
- Steps down Application board 12V power to 6V, 0.5A max
- Output current adjustable via PWM to allow for LED dimming
- Drives two high power white LEDs

# Main considerations taken in design of the board:
	- No ground plane or traces opposite the inductor avoid effects of eddy currents
	- Input capacitors located as close to the chip power pins on the same layer
	- Drain of PFET connected as close as possible to power inductor, with minimal trace area
	- Place output capacitor close to the inductor
	- At least 4 thermal vias located under the buck controller thermal pad for heat dissipation	
	- Isolate small signal ground (IC ground) from the power ground
	- Minimise the use of vias for the power ground
	- Isolate noise-causing components (diode, inductor, PWM signal) away from other components
	- In particular, below are the recommendations for designing the feedback route. However the design of our particular driver makes them somewhat irrelevant:
		a). Feedback terminal of IC which inputs feedback signal, is normally designed with high impedance. Output of this terminal and
		resistor crossover network must be connected with short wire.
		b). Part which detects the output voltage must be connected after output capacitor or at both ends of output capacitor.
		c). Wiring the resistor-divider circuit nearby and parallel, makes it better for noise tolerance.
		d). Draw wire far away from switching node of inductor and diode. Do not wire directly below the inductor and diode, and not
		parallel to power supply line. Multilayer board must be also wired in the same way. 
	- Proper land pattern of power components to minimize impedance: (Application Notes 136, Page 7)
		http://cds.linear.com/docs/en/application-note/an136f.pdf
	

(recommendations drawn from: http://rohmfs.rohm.com/en/products/databook/applinote/ic/power/switching_regulator/converter_pcb_layout_appli-e.pdf)

# Possible changes/improvements:
	- decrease the board area
	
# Thoughts:
	- The board schematic was based off the 'Typical Applications' under section 8.2 of the Buck Controller datasheet. For some reason that design does not have a output capacitor, unlike standard buck controller designs
	- The design mentioned above also uses a different feedback system compared to the standard; namely it only uses 1 resistor instead of 2
