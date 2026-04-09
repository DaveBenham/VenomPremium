# Venom Premium
This is documentation for Venom Premium Plugins for VCV Rack containing commercial modules.

Also checkout the [Venom site](https://github.com/DaveBenham/VenomModules/blob/main/README.md) for documenation about free Venom modules.

## Table of Contents

- [Standard Venom Context Menus](#standard-venom-context-menus)
- [Venom Oscillations Plugin](VenomOscillations.md#venom-oscillations-plugin)
  - [Sofia's Daughter](VenomOscillations.md#sofias-daughter)
  - [Spice Factory](VenomOscillations.md#spice-factory)
- [Venom Chaos Boxes Plugin](VenomChaosBoxes.md#venom-chaos-boxes-plugin)
  - [Venjolin](VenomChaosBoxes.md#venjolin)
  - [Venjolin Plus](VenomChaosBoxes.md#venjolin-plus)
  - [Vlippoo Box](VenomChaosBoxes.md#vlippoo-box)
  - [Vlippoo Box Plus](VenomChaosBoxes.md#vlippoo-box-plus)
  - [Hybrid Knot](VenomChaosBoxes.md#hybrid-knot)
  - [Chaos Gates Expander](VenomChaosBoxes.md#chaos-gates-expander)
  - [Chaos Volts Expander](VenomChaosBoxes.md#chaos-volts-expander)


# Standard Venom Context Menus

There are a set of context menu options that are common to all Venom modules.

## Custom Port and Parameter Names
Every port (input or output), and every parameter (knob, slider, or button) has its own context menu option to set a custom name. Custom names only appear in context menus and hover text - they do not change the faceplate graphics.

If a parameter or port is given a custom name, then an additional option is added to restore the factory default name.

Custom names are saved with the patch and with presets, and restored upon patch or preset load. Custom names are also preserved when duplicating a module.

## Parameter Locks and Custom Defaults
Every parameter (knob, slider, or button) has its own parameter context menu options to lock the paramenter as well as set a custom default value. In addition, there are module context menu options to lock and unlock all parameters.

Parameter lock and custom default settings are saved with the patch and with presets, and restored upon patch or preset load. Parameter lock and custom default settings are also preserved when duplicating a module.

### Parameter Locks
The parameter tooltip includes the word "locked" below the parameter name when hovering over a locked parameter.

The parameter value cannot be changed by any means while the parameter is locked. All of the normal means of changing a parameter value are blocked:

- The parameter cannot be dragged or pushed
- Context menu value keyins are ignored
- Double click and context menu initialization are ignored
- Randomization requests are ignored

### Custom Defaults
A custom default value overrides the factory default whenever a parameter is initialized. An additional parameter menu option is added to restore the factory default whenever a custom default is in effect.

## Themes
The module context menu includes options to set the default theme and default dark theme for the Venom plugin, as well as a theme override for each module instance. Each plugin maintains its own defaults.

There are 4 themes to choose from.

- Ivory (high contrast with off-white background)
- Coal (high contrast with blackish background)
- Earth (low contrast with a brown background)
- Danger (high contrast with vibrant red background)

If a module instance is set to use a specific theme, then that theme will be used regardless whether VCV Rack is set to use dark panels or not. If a module is set to use the default theme, then the VCV Rack "Use dark panels if available" setting controls which default is used. If not enabled, then the default theme is used. If enabled then the default dark theme is used.

If you want the default theme to disregard the VCV Rack dark panel setting, then simply set both defaults to the same theme.

The factory default theme is ivory, and the factory default dark theme is coal.

*[Venom Premium TOC](#table-of-contents)*
