# VenomPremium
Documentation for Venom Premium Plugins for VCV Rack


# Sofia's Daughter
![Sofia's Daughter module image](doc/SofiasDaughter.png)  
Sofia's Daughter is a complex polyphonic formant oscillator inspired by the wonderful [XAOC Devices Sofia "1955 Transcendent Analog Waveform Oscillator"](http://xaocdevices.com/main/sofia). Sofia's Daughter implements all the basic functionality (though not necessarily the exact sound) of the XAOC Eurorack hardware module, and then extends the functionality with additional controls, inputs, and outputs.

The underlying principle behind the module is FOF (fonction d'onde formantique) synthesis, a method of producing formant sounds through short bursts of decaying sinusoidal waveforms. The primary output of Sofia's Daughter consists of two such decaying sinusoidal waveforms, called Ripple elements, combined with a saturated sine wave called the Fundamental. The Fundamental triggers (hard syncs) the Ripple elements as well as their decay envelopes. The Ripple frequencies are measured as ratios of the Fundamenal frequency, and the Ripple decay length is proportional to the Fundamental wavelength. Sofia's Daughter extends the FOF synthesis by allowing waveforms other than sine for the Ripple elements.

Because the Ripple elements are always phase aligned with the Fundamental, the output can remain harmonious, regardless what frequency ratios are used for the Ripples.

Here is one example of an output waveform, along with the three component elements.
![Example output waveform image](doc/FinalMixComponents.png)  

With these three basic building blocks, plus a bunch of modulation possibilities, Sofia's Daughter can produce an astonishing range of sounds. Of course the output works well with VCAs and Filters, but rich resonant sounds can be produced without a filter. And the Ripple decay envelopes coupled with available one shot modes allow for the creation of percussive and bell-like voices without the need of external VCAs or envelope generators.

All inputs can be driven at audio rates. Oversampling is available to mitigate aliasing that might otherwise be present at the outputs.

Because all inputs and outputs are polyphonic, a single instance of Sofia's Daughter can create up to 16 independent voices. The total number of output channels at each port is the maximum number of channels found across all inputs. If an input is monophonic, than the single channel is replicated to match the output channel count. If a polyphonic input has fewer channels than the outputs, then missing channels are treated as constant 0V.

## Module Organization
Sofia's Daughter can be divided into 5 distinct sections, each with its own purpose
- **Fundamental** ***(top center)***: Controls the frequency, shape, and timing of the fundamental sine wave
- **Ripple A** ***(left)***: Controls the frequency, decay, and shape of the first ripple element
- **Ripple B** ***(right)***: Controls the frequency, decay, and shape of the second ripple element
- **Global** ***(mid center)***: Controls the mix of the three elements, plus additional controls that effect all three elements
- **Outputs** ***(bottom)***: Nine different outputs are available

## Fundamental Section
![Fundamental section image](doc/FundamentalSection.png)  
The Fundamental has six basic types of control
- Frequency
- Mode
- Saturation
- Frequency Modulation
- Phase Modulation
- Sync

### Fundamental Frequency
The **OCTAVE** knob establishes the range of the Pitch knob. The Octave ranges from -2 to 6, with each integral value representing a particular octave for the note C. The default noon value is C2.

The **PITCH** knob applies an offset to the value established by the Octave knob. The offset is measured in cents, with 100&cent; representing one semitone. The range is -1200&cent; to 1200&cent; (+/- 1 octave), with zero offset at the default noon position.

The **V/OCT** CV input further offsets the fundamental frequency. Each volt represents one octave offset.

### Fundamental Mode

The **1 SHOT** button controls the mode of the Fundamental oscillator. It has three states.
- **Off** ***(white, default)***: The fundamental oscillates continuously
- **Retriggered one shot** ***(yellow)***: The fundamental produces one single wave cycle upon receiving a trigger at the Hard Sync input, then stops and waits for the next trigger. The oscillator can be retriggered to start afresh mid-cycle.
- **Triggered one shot** ***(blue)***: Same as Retriggered one shot, except triggers received mid-cycle are ignored.

### Fundamental Saturation

The top left small **SATURATE** knob controls the base amount of saturation to apply to the sine wave. Fully counter-clockwise applies no saturation, and fully clockwise applies the maximum amount of saturation allowed. The default at noon is 50% of the maximum.

The associated CV input is attenuated by the small attenuverter in the upper right, with 1V representing 10% saturation.

The net sum of base saturation plus attenuated CV is clamped to a value between 0% and 100%.

![Fundamental saturation image](doc/FundamentalSaturation.png)

### Fundamental Frequency Modulation

Frequency modulation can be applied to the Fundamental wave via the **FM** CV input, with its own dedicated attenuater.

There are three FM modes controlled by the small associated **LIN** button.
- **Off** ***(gray, default)***: Exponential FM is used. The CV is DC coupled.
- **AC coupled** ***(yellow)***: Through zero linear FM is used. The CV is AC coupled so that CV with any bias can still produce harmonious results.
- **DC coupled** ***(blue)***: Through zero linear FM is used, but this time the CV is DC coupled.

Note that Fundamental FM never applies to the Ripple frequency or decay length computations. However, FM can cause folding of the fundamental wave such that it changes the rate at which the Ripple elements are triggered.

### Fundamental Phase Modulation

Phase modulation can be applied to the Fundamental wave via the **PM** CV input, with its own dedicated attenuator. Phase modulation is often incorrectly referred to as through zero linear FM, and with sinusoidal CV it can give the same results. But other waveforms produce different results than true linear FM.

Phase modulation can cause folding of the fundamental wave such that it changes the rate at which the Ripple elements are triggered.

### Fundamental Sync

The **HARD SYNC** CV input resets the fundamental wave to phase 0 upon receipt of a trigger. It is also used to trigger the Fundamental when using a one shot mode.

The **SOFT SYNC** CV input reverses the direction of the fundamental wave upon receipt of a trigger.

Both sync inputs use Schmitt triggers that are triggered at 2V and reset at 0.2V so they can be used with both unipolar and bipolar inputs.

## Ripple Sections
![Ripple sections image](doc/RippleSections.png)  

![Warp Effects image](doc/WarpEffects.png)

## Global Section
![Global section image](doc/GlobalSection.png)

## Outputs Section
![Outputs section image](doc/OutputsSection.png)  



