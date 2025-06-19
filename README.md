# VenomPremium
Documentation for Venom Premium Plugins for VCV Rack


# Sofia's Daughter
![Sofia's Daughter module image](doc/SofiasDaughter.png)  
A complex polyphonic formant oscillator inspired by the wonderful [XAOC Devices Sofia "1955 Transcendent Analog Waveform Oscillator"](http://xaocdevices.com/main/sofia). Sofia's Daughter implements all the basic functionality (though not necessarily the exact sound) of the XAOC Eurorack hardware module, and then extends the functionality with additional controls, inputs, and outputs.

The underlying principle behind the module is FOF (fonction d'onde formantique) synthesis, a method of producing formant sounds through short bursts of decaying sinusoidal waveforms. The primary output of Sofia's Daughter consists of two such decaying sinusoidal waveforms, called Ripple elements, combined with a saturated sine wave called the Fundamental. The Fundamental triggers (hard syncs) the Ripple elements as well as their decay envelopes. The Ripple frequencies are measured as ratios of the Fundamenal frequency, and the Ripple decay length is proportional to the Fundamental wavelength. Sofia's Daughter extends the FOF synthesis by allowing waveforms other than sine for the Ripple elements.

Because the Ripple elements are always phase aligned with the Fundamental, the output can remain harmonious, regardless what frequency ratios are used for the Ripples.

Here is one example of an output waveform, along with the three component elements.
![Example output waveform image](doc/FinalMixComponents.png)  

With these three basic building blocks, plus a bunch of modulation possibilities, Sofia's Daughter can produce an astonishing range of sounds. Because all inputs and outputs are polyphonic, a single instance of Sofia's Daughter can create up to 16 independent voices. Of course the output works well with VCAs and Filters, but rich resonant sounds can be produced without a filter. And the Ripple decay envelopes coupled with available one shot modes allow for the creation of percussive and bell-like voices without the need of external VCAs or envelope generators.

## Module Organization
Sofia's Daughter can be divided into 5 distinct regions, each with its own purpose
- Fundamental (top center): Controls the frequency, shape, and timing of the fundamental sine wave
- Ripple A (left): Controls the frequency, decay, and shape of the first ripple element
- Ripple B (right): Controls the frequency, decay, and shape of the second ripple element
- Global (mid center): Controls the mix of the three elements, plus additional controls that effect all three elements
- Outputs (bottom): Nine different outputs are available

## Fundamental Section
![Fundamental section image](doc/FundamentalSection.png)  

## Ripple Sections
![Ripple sections image](doc/RippleSections.png)  

## Global Section
![Global section image](doc/GlobalSection.png)

## Outputs Section
![Outputs section image](doc/OutputsSection.png)  

![Warp Effects image](doc/WarpEffects.png)



