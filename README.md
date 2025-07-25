# VenomPremium
Documentation for Venom Premium Plugins for VCV Rack

# Venom Oscillations plugin
Venom Oscillations [version 2.0.0](VenomOscillationsChangeLog.md) for VCV Rack 2 is copyright 2025 Dave Benham and licensed under the [VCV Rack End User License Agreement](LICENSE.md).

Thank you for your interest in and/or purchase of the Venom Oscillations plugin for VCV Rack 2. This plugin is intended to be a set of complex oscillator modules that produce interesting sounds that are difficult to create otherwise.

Currently there is only one module, Sofia's Daughter.

I have tentative plans for at least one more complex oscillator. If/when a new oscillator is added, the plugin purchase price will likely increase in $5 increments. However, existing plugin owners will receive the new module for free.

If you are on the fence of purchasing, you can try out a [Sofia's Daughter prototype/emulation patch](https://patchstorage.com/orb_sq-and-venom-sophia/) created using only free modules. You can experiment with some of the types of sounds Sofia's Daughter can produce. If you like it, then you can be confident Venom Oscillations sounds at least as good, has greater tonal range, is more CPU efficient, and is much more convenient to patch than the emulation/prototype patch.

# Sofia's Daughter
*Quick Links: [Intro](#sofias-daughter) | [Fundamental](#fundamental-section) | [Ripples](#ripple-sections) | [Global](#global-section) | [Output](#output-section) | [ContextMenus](#context-menus) | [XAOS-Venom Differences](#differences-from-xaoc-devices-sofia)*

![Sofia's Daughter module image](doc/SofiasDaughter.png)  
Sofia's Daughter is a complex polyphonic formant oscillator inspired by the wonderful [XAOC Devices Sofia "1955 Transcendent Analog Waveform Oscillator"](http://xaocdevices.com/main/sofia). Sofia's Daughter implements all the basic functionality (though not necessarily the exact sound) of the XAOC Eurorack hardware module, and then extends the functionality with additional controls, inputs, and outputs. A [complete list of known differences](#differences-from-xaoc-devices-sofia) is listed at the end of this documentation.

The underlying principle behind the module is FOF (fonction d'onde formantique) synthesis, a method of producing formant sounds through short bursts of decaying sinusoidal waveforms. The primary output of Sofia's Daughter consists of two such decaying sinusoidal waveforms, called Ripple elements, combined with a saturated sine wave called the Fundamental. Sofia's Daughter extends the FOF synthesis by allowing waveforms other than sine for the Ripple elements.

The Fundamental wave hard syncs the Ripple elements and also triggers their decay envelopes upon the start of each Fundamental cycle. The Ripple frequencies are measured as ratios of the Fundamenal frequency, and the Ripple decay length is proportional to the Fundamental wavelength. Because the Ripple elements are always phase aligned with the Fundamental, the output can remain harmonious, regardless what frequency ratios are used for the Ripples.

Here is one example of an output waveform, along with the three component elements.

![Example output waveform image](doc/FinalMixComponents.png)  

With these three basic building blocks, plus a bunch of modulation possibilities, Sofia's Daughter can produce an astonishing range of sounds. Of course the output works well with VCAs and Filters, but rich resonant sounds can be produced without a filter. And the Ripple decay envelopes coupled with available one shot modes allow for the creation of percussive and bell-like voices without the need of external VCAs or envelope generators.

All inputs can be driven at audio rates. Oversampling is available to mitigate digital aliasing that might otherwise be present at the outputs.

Because all inputs and outputs are polyphonic, a single instance of Sofia's Daughter can create up to 16 independent voices. The total number of channels at each output port is the maximum number of channels found across all inputs. If an input is monophonic, than the single channel is replicated to match the output channel count. If a polyphonic input has fewer channels than the outputs, then missing channels are treated as constant 0V.

Polyphony also makes it possible to work with more than two Ripple elements for a single voice by applying polyphonic CV to Ripple inputs only, and using only monophonic CV for the Fundamental. There will still be multiple Fundamental waves, but they all will be running at the same rate with the same saturation. If the Fundamentals are out of phase, then simply patch a mono trigger to the Hard Sync input. The sync trigger can be removed once the Fundamentals are in phase.

Be sure to watch [Omri Cohen's overview of Sofia's Daughter](https://www.youtube.com/watch?v=yUtYXnmDmvg) that provides many ideas on techniques to explore. You might also check out video's about the XAOC Devices Sofia from [Tom Churchill](https://youtu.be/5lWf4N7jbbI) and [Monotrail Tech Talk](https://youtu.be/xdjGRF7Wtwg), as they may also provide inspiration for ways you might use Sofia's Daughter in your VCV patches.

*Quick Links: [Intro](#sofias-daughter) | [Fundamental](#fundamental-section) | [Ripples](#ripple-sections) | [Global](#global-section) | [Output](#output-section) | [ContextMenus](#context-menus) | [XAOS-Venom Differences](#differences-from-xaoc-devices-sofia)*

## Sofia's Daughter Layout
Sofia's Daughter can be divided into 5 distinct sections, each with its own purpose
- **[Fundamental](#fundamental-section)** ***(top center)***: Controls the frequency, shape, and timing of the fundamental sine wave
- **[Ripple A](#ripple-sections)** ***(left)***: Controls the frequency, decay, and shape of the first ripple element
- **[Ripple B](#ripple-sections)** ***(right)***: Controls the frequency, decay, and shape of the second ripple element
- **[Global](#global-section)** ***(mid center)***: Controls the mix of the three elements, plus additional controls that affect all three elements
- **[Outputs](#output-section)** ***(bottom)***: Nine different outputs are available

There are also [context menu options](#context-menus) that give access to some additional minor functionality.

## Fundamental Section
![Fundamental section image](doc/FundamentalSection.png)  
The Fundamental sine wave has six basic types of control
- Frequency
- Mode
- Saturation
- Frequency Modulation
- Phase Modulation
- Sync

### Fundamental Frequency
The **OCTAVE** knob establishes the range of the Pitch knob. The Octave ranges from -2 to 6, with each integral value representing a particular octave for the note C. The default noon value is C2.

The **PITCH** knob applies an offset to the value established by the Octave knob. The offset is measured in cents, with 100&cent; representing one semitone. The range is -1200&cent; to 1200&cent; (+/- 1 octave), with zero offset at the default noon position.

The bipolar **V/OCT** CV input further offsets the fundamental frequency. Each volt represents one octave offset.

### Fundamental Mode

The **1 SHOT** button controls the mode of the Fundamental oscillator. It has three states.
- **Off** ***(gray, default)***: The fundamental oscillates continuously
- **Retriggered one shot** ***(yellow)***: The fundamental produces one single wave cycle upon receiving a trigger at the Hard Sync input, then stops and waits for the next trigger. The oscillator can be retriggered to start afresh mid-cycle.
- **Triggered one shot** ***(blue)***: Same as Retriggered one shot, except triggers received mid-cycle are ignored.

Remember that the Fundamental triggers the Ripple envelopes every cycle. So if using a one shot mode, each time the Fundamental is triggered, it also triggers the Ripple envelopes. Upon envelope completion, the Ripple envelope will wait for the next trigger, just like the Fundamental.

### Fundamental Saturation

The left small knob below the **SATURATE** label controls the base amount of saturation to apply to the sine wave. Fully counter-clockwise applies no saturation, and fully clockwise applies the maximum amount of saturation allowed. The default at noon is 50% of the maximum.

The degree of saturation can be additively modulated by the associated bipolar CV input with its small attenuverter knob. One volt of CV at 100% represents 10% saturation.

The net sum of base saturation plus attenuated CV is clamped to a value between 0% and 100%.

![Fundamental saturation image](doc/FundamentalSaturation.png)

### Fundamental Frequency Modulation

Frequency modulation can be applied to the Fundamental wave via the **FM** CV input, with its own dedicated attenuator.

There are three FM modes controlled by the small associated **LIN** (linear) button.
- **Off** ***(gray, default)***: Exponential FM is used. The CV is DC coupled.
- **AC coupled** ***(yellow)***: Through zero linear FM is used. The CV is AC coupled so that CV with DC bias can still produce harmonious results. A high pass filter removes DC bias from the CV.
- **DC coupled** ***(blue)***: Through zero linear FM is used, but this time the CV is DC coupled, preserving any DC bias.

Note that Fundamental FM never applies to the Ripple frequency or decay length computations. However, FM can cause folding of the fundamental wave such that it changes the rate at which the Ripple elements are triggered.

### Fundamental Phase Modulation

Phase modulation can be applied to the Fundamental wave via the **PM** CV input, with its own dedicated attenuator. Phase modulation is often incorrectly referred to as through zero linear FM, and with sinusoidal CV it can give the same results. But other waveforms produce different results than true linear FM.

Phase modulation can cause folding of the fundamental wave. But unlike the frequency modulation, phase modulation never alters the rate at which the Ripple elements are triggered.

### Fundamental Sync

The **HARD SYNC** CV input resets the fundamental wave to phase 0 upon receipt of a trigger. It is also used to trigger the Fundamental when using a one shot mode.

The **SOFT SYNC** CV input reverses the direction of the fundamental wave upon receipt of a trigger.

Both sync inputs use Schmitt triggers that are triggered at 2V and reset at 0.2V so they can be used with both unipolar and bipolar inputs.

*Quick Links: [Intro](#sofias-daughter) | [Fundamental](#fundamental-section) | [Ripples](#ripple-sections) | [Global](#global-section) | [Output](#output-section) | [ContextMenus](#context-menus) | [XAOS-Venom Differences](#differences-from-xaoc-devices-sofia)*

## Ripple Sections
![Ripple sections image](doc/RippleSections.png)  

The Ripple A and Ripple B sections behave identically, other than being mirror images of each other.

Each Ripple has five main types of control:
- Waveform
- Envelope decay length
- Ripple frequency
- Warp modulation that has different effects, depending on the mode
- Phase modulation

### Ripple Waveform

The square **WAVE** button sets the basic waveform of the Ripple element. It has three possible values:
- **SIN** ***(default)*** - Sine
- **TRI** - Triangle
- **SQR** - Square

### Ripple Envelope Decay
The Ripple decay envelope controls how each Ripple envelope is damped. It always has an instantaneous attack, and the decay length is always measured as a ratio of the Fundamental wavelength.

The **DECAY TRACK** button controls whether the decay length tracks CV at the Fundamental V/Oct input. If off, then only the Fundamental Octave and Pitch knobs affect the decay length. If on, then the V/Oct CV is added to the computation.

The **DECAY** slider sets the ratio of the envelope decay length to the Fundamental wavelength. It ranges from 0.015625 (1/64) to 16 times the Fundamental wavelength.

The associated bipolar Decay CV input and attenuverter knob can additively modulate the decay length beyond the limits of the slider. Both the slider and the CV respond exponentially.

Note that envelopes with a ratio >1 normally never reach the end because the start of the next Fundamental cycle retriggers the Ripple envelopes. So the maximum decay ratio of 16 results in minimal damping of the Ripple element.

But if using a Fundamental one shot mode, the Ripple envelopes are triggered the same time as the Fundamental, and the envelopes always decay to completion unless another trigger is received. Upon completion, each envelope waits for the next Fundamental trigger.

### Ripple Frequency
The Ripple frequency is always measured as a ratio of the Fundamental frequency.

The **FREQ TRACK** button controls whether the ripple frequency tracks CV at the Fundamental V/Oct input. If off, then only the Fundamental Octave and Pitch knobs affect the frequency (as well as any Global FM). If on, then the V/Oct CV is added to the computation.

The **FREQ** slider sets the ratio of the Ripple frequency to the Fundamental frequency. It ranges from 1 to 256 times the Fundamental frequency.

The associated bipolar Frequency CV input and attenuverter knob can additively modulate the frequency ratio. Both the slider and CV respond exponentially. CV can modulate the frequency ratio above 256, but the minimum effective frequency ratio is 1.

### Ripple Warp Modulation
The small button above the **WARP** label controls the mode of the Warp modulation. There are a total of nine modes. In general, the Warp can either influence the Ripple frequency, the envelope decay curve, or the Ripple waveform shape. The button is color coded.

**Frequency ramp modulation available to all waveforms**
- **Frequency ramp** ***(white, default)***: The warp value specifies a ratio of the base frequency that each ripple starts at, and then modulates to the base frequency by the end of the envelope decay. This is the warp effect implemented by the XAOC Sofia hardware.
- **Inverse frequency ramp** ***(yellow)***: The ripple starts at the base frequency, and the frequency modulates to the warp ratio value by the end of the envelope decay.

The warp ratio value ranges from 0.25 (1/4) to 4 times the base frequency.

**Envelope decay curve modulation available to all waveforms**
- **Envelope J-curve** ***(brown or dark red)***
- **Envelope S-curve** ***(tan)***

The warp value ranges from an arbitrary -100% to 100%.

**Ripple shape modulation available to all waveforms**
- **Ripple PWM** ***(red)***: Pulse width modulation

The warp value ranges from 10% to 90%. For sine and triangle waveforms the percentage represents the width of the positive portion of the waveform. The negative portion is shrunk or expanded to total 100%.

**Ripple shape modulation available only to sine and triangle**  
The same colors are available to square, but they simply repeat the first four modes.
- **Ripple skew** ***(orange)***
- **Ripple J-curve** ***(dark blue)***
- **Ripple S-curve** ***(light blue)***
- **Ripple rectify** ***(green)***

The warp value ranges from an arbitrary -100% to 100%

The **WARP** knob sets the base Warp value. The associated bipolar CV input and small attenuverter can additively modulate the Warp value. The sum of the knob plus attenuated CV is clamped to the range for the current mode. With the Warp knob at the default noon position, and no CV, there is no Warp effect.

The traces below demonstrate the different Warp effects that are available. CCW represents full counter-clockwise Warp, and CW represents full clockwise Warp. Each trace uses a decay ratio of 1 and a frequency ratio of 15.

![Warp Effects image](doc/WarpEffects.png)

### Ripple Phase Modulation

Phase modulation can be applied to the Ripple wave via the **PM** CV input, with its own dedicated attenuator.

*Quick Links: [Intro](#sofias-daughter) | [Fundamental](#fundamental-section) | [Ripples](#ripple-sections) | [Global](#global-section) | [Output](#output-section) | [ContextMenus](#context-menus) | [XAOS-Venom Differences](#differences-from-xaoc-devices-sofia)*

## Global Section
![Global section image](doc/GlobalSection.png)  
The Global section controls the following:
- Overall mix of the main output
- Frequency Modulation applied to the Fundamental and both Ripple elements
- Oversampling to mitigate digital aliasing
- Polyphony Reset to deal with unwanted polyphonic feedback

### Overall Mix Control

Two different controls are used to create the main Final Mix output

The **RIPPLE MIX** knob controls the mix of Ripple A and Ripple B, with full counter-clockwise selecting 100% A, and full clockwise representing 100% B. The default noon value is a 50/50 mix of both. The knob is scaled to represent the percentage of Ripple B. The Ripple A percentage is simply 100% minus the Ripple B percentage. 

The Ripple Mix can be modulated via the associated bipolar CV input and attenuverter. Each volt of CV equates to 10% Ripple B. The attenuated CV value is summed with the knob value to get the effective Ripple Mix. The effective value is clamped to between 0 and 100% Ripple B.

The **ELEMS MIX** knob controls the mix of the Fundamental saturated sine wave relative to the Ripple elements from the Ripple Mix. Full counter-clockwise is 100% Fundamental, and full clockwise is 100% Ripple Mix. The default noon value is a 50/50 mix of both. The knob is scaled to represent the percentage of Ripple Mix. The Fundamental percentage is simply 100% minus the Ripple Mix percentage.

The Elements mix can be modulated via the associated bipolar CV input and attenuverter. Each volt of CV equates to 10% Ripple Mix. The attenuated CV value is summed with the knob value to get the effective Elements Mix. The effective value is clamped to between 0 and 100% Ripple Mix.

### Global Frequency Modulation
The **FM** CV input and attenuator provides for simultaneous frequency modulation of the Fundamental sine and both Ripple elements.

There are three FM modes controlled by the small associated **LIN** button.
- **Off** ***(gray, default)***: Exponential FM is used. The CV is DC coupled.
- **AC coupled** ***(yellow)***: Through zero linear FM is used. The CV is AC coupled so that CV with DC bias can still produce harmonious results. A high pass filter removes DC bias from the CV.
- **DC coupled** ***(blue)***: Through zero linear FM is used, but this time the CV is DC coupled, preserving any DC bias.

Even though the Global FM modulates the Ripple element frequencies, it does not modulate the Ripple decay times. However, since global FM can fold the Fundamental sine, it can alter the rate at which the Ripple envelopes are triggered.

### Oversampling
DPW antialiasing is applied to Ripple square waves, and simple sine and triangle waves rarely have significant aliasing. So in the absence of any CV modulation, oversampling is rarely needed. But if audio rate CV modulation is applied, then unwanted digital aliasing can arise.

The square **OVER SAMPLE** button controls oversampling that is used to mitigate digital aliasing. It has 6 values:
- **Off** ***(default)***
- **x2**
- **x4**
- **x8**
- **x16**
- **x32**

When oversampling is activated, every CV input is upsampled with interpolation to the oversample rate times the VCV sample rate. All internal processing is done at this elevated rate. At each output, a low pass filter removes high frequency content that would alias at the original VCV sample rate, and then the signal is downsampled back to the VCV sample rate.

Each input that is upsampled with interpolation consumes a significant amount of CPU power, as does the low pass filter for each output. So if your system has limited CPU power, it may be wise to avoid oversampling unless unwanted aliasing is detected in the output. And if oversampling is used, the lowest value should be used that provides acceptable results.

### Polyphony Reset

Once polyphony is introduced at any one of the CV inputs, all outputs become polyphonic. If there is a feedback loop from one of the outputs to one of the inputs, then all output will remain polyphonic even after all external polyphonic CV sources are removed. In this case all poly channels will produce the same signal, except for possible phase differences.

The **RESET POLY** button can be used to temporarily force all outputs to monophonic so that upon release, the outputs will revert back to the maximum number of channels found across all external CV inputs.

*Quick Links: [Intro](#sofias-daughter) | [Fundamental](#fundamental-section) | [Ripples](#ripple-sections) | [Global](#global-section) | [Output](#output-section) | [ContextMenus](#context-menus) | [XAOS-Venom Differences](#differences-from-xaoc-devices-sofia)*

## Output Section
![Output section image](doc/OutputsSection.png)  

There are nine outputs available across the bottom of the module.

- **A ENV** - the unipolar decay envelope for Ripple A
- **A RAW** - the constant bipolar Ripple A signal without any damping from the decay envelope
- **A DAMP** - the bipolar damped Ripple A signal after the A decay envelope has been applied
- **RAW FUND** - the bipolar Fundamental sine signal without any saturation
- **FINAL MIX** - the bipolar mix of saturated Fundamental sine and both damped Ripple elements
- **SAT FUND** - the bipolar Fundamental sine signal with saturation applied
- **B DAMP** - the bipolar damped Ripple B signal after the B decay envelope has been applied
- **B RAW** - the constant bipolar Ripple B signal without any damping from the decay envelope
- **B ENV** - the unipolar decay envelope for Ripple B

Any of the outputs can be used as an audio source, or as a modulator. Many interesting sounds can be found by patching an output into one of Sofia's Daughter CV inputs, thus creating a feedback loop.

### Bypass Behavior

All outputs are constant monophonic 0V if Sofia's Daughter is bypassed.

*Quick Links: [Intro](#sofias-daughter) | [Fundamental](#fundamental-section) | [Ripples](#ripple-sections) | [Global](#global-section) | [Output](#output-section) | [ContextMenus](#context-menus)*

## Context Menus

### Custom Port and Parameter Names
Every port (input or output), and every parameter (knob, slider, or button) has its own context menu option to set a custom name. Custom names only appear in context menus and hover text - they do not change the faceplate graphics.

If a parameter or port is given a custom name, then an additional option is added to restore the factory default name.

Custom names are saved with the patch and with presets, and restored upon patch or preset load. Custom names are also preserved when duplicating a module.

### Parameter Locks and Custom Defaults
Every parameter (knob, slider, or button) has its own parameter context menu options to lock the paramenter as well as set a custom default value. In addition, there are module context menu options to lock and unlock all parameters.

Parameter lock and custom default settings are saved with the patch and with presets, and restored upon patch or preset load. Parameter lock and custom default settings are also preserved when duplicating a module.

**Parameter Locks**  
The parameter tooltip includes the word "locked" below the parameter name when hovering over a locked parameter.

The parameter value cannot be changed by any means while the parameter is locked. All of the normal means of changing a parameter value are blocked:

- The parameter cannot be dragged or pushed
- Context menu value keyins are ignored
- Double click and context menu initialization are ignored
- Randomization requests are ignored

**Custom Defaults**  
A custom default value overrides the factory default whenever a parameter is initialized. An additional parameter menu option is added to restore the factory default whenever a custom default is in effect.

### Themes
The module context menu includes options to set the default theme and default dark theme for the VenomOscillations plugin, as well as a theme override for each module instance.

There are 4 themes to choose from.

- Ivory (high contrast with off-white background)
- Coal (high contrast with blackish background)
- Earth (low contrast with a brown background)
- Danger (high contrast with vibrant red background)

If a module instance is set to use a specific theme, then that theme will be used regardless whether VCV Rack is set to use dark panels or not. If a module is set to use the default theme, then the VCV Rack "Use dark panels if available" setting controls which default is used. If not enabled, then the default theme is used. If enabled then the default dark theme is used.

If you want the default theme to disregard the VCV Rack dark panel setting, then simply set both defaults to the same theme.

The factory default theme is ivory, and the factory default dark theme is coal.

*Quick Links: [Intro](#sofias-daughter) | [Fundamental](#fundamental-section) | [Ripples](#ripple-sections) | [Global](#global-section) | [Output](#output-section) | [ContextMenus](#context-menus) | [XAOS-Venom Differences](#differences-from-xaoc-devices-sofia)*

## Differences from XAOC Devices Sofia

Here is a summary of the major known differences between XAOC Devices Sofia and Venom Sofia's Daughter.

### Inconsequential name changes (Sofia name -> Sofia's Daughter name)

**Controls and CV**
- Global FM -> Fundamental FM
- Pitch FM -> Global FM
- Elements Mix -> Ripple Mix
- Fund-Elem Mix -> Elems Mix
- Damp A / B -> A / B Decay (Decay time ratio)
- Ratio A / B -> A / B Freq (Frequency ratio)

**Outputs**
- Impulse A / B -> A / B Env (envelope)
- A / B Out -> A / B Raw

### Main disadvantage of Sofia's Daughter
-  The digital implementation limits frequency, and can also lead to aliasing. But oversampling options are available to mitigate aliasing

### Global Sofia's Daughter Enhancements
- Polyphony! All inputs and outputs are polyphonic
- Added attenuators or attenuverters for all CV inputs (except for V/Oct and sync inputs)
- Options for global linear through zero FM in addition to exponential FM (actually called pitched FM on Sofia)

### Fundamental Sine VCO enhancements
- User control of sine saturation level, including CV input
- Hard Sync
- Soft Sync
- One shot modes triggered by Hard Sync - great for percussion voices
- Phase Modulation (frequently called through-zero linear frequency modulation)
- Options for fundamental linear through zero FM in addition to exponential FM (actually called global FM on Sofia)

### Ripple Element VCO enhancements
- Additional triangle option to go along with sine and square
- Phase Modulation
- Multiple Warp modes available to all wave forms
  - Frequency ramp (the default mode that matches the XAOC hardware)
  - Inverse frequency ramp
  - J curve envelope decay shape
  - S curve envelope decay shape
  - Ripple PWM (even for sine and triangle!)
- Additional Ripple shaping Warp modes available to sine and triangle only
  - Ripple skew
  - Ripple J-curve
  - Ripple S-curve
  - Ripple rectify

### Additional Outputs
- Saturated fundamental sine
- Damped Ripple A
- Damped Ripple B

*Quick Links: [Intro](#sofias-daughter) | [Fundamental](#fundamental-section) | [Ripples](#ripple-sections) | [Global](#global-section) | [Output](#output-section) | [ContextMenus](#context-menus) | [XAOS-Venom Differences](#differences-from-xaoc-devices-sofia)*
