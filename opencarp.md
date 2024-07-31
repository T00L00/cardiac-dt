## General notes

- Example 03_study_prep_APD shows how to load a custom wire mesh file

## imp_region

Defines the ionic models used for the simulation

- `num_imp_regions`
  - Total number of regions with different ionic models

- `imp_region[PrMelem1].ID`
  - Array of element tags associated with this model region
  - Default: 0

- `imp_region[PrMelem1].num_IDs`
  - Number of elements tags listed in 'imp_region[].ID[]'
  - Default: 0

- `imp_region[PrMelem1].name`
  - Arbitrary name of the region

- `imp_region[PrMelem1].im`
  - Defines the ionic model to be used in the simulation.
  - Query for available ionic models by calling 'bench' with the command list-imps.
  - Default: "LuoRudy91"

- `imp_region[PrMelem1].im_param`
  - Specify a comma separated list of changes from default values
  - Example: `'APDshorten*4,Gks*1.29,...'`

- `imp_region[PrMelem1].cellSurfVolRatio`
  - Defines the default single cell surface-to-volume-ratio used if it is not specified by the ionic model plugin (IMP)
  - Default: 0.14 (um^-1)
  - Options: Must be greater than 0.

- `imp_region[PrMelem1].volFrac`
  - Defines the portion of the volume occupied by cells.
  - Default: 1.
  - Options: Must be betweeen 0 and 1.

- `imp_region[PrMelem1].plug_param`
  - User can modify the values for various ionic model parameters.
  - Every PARAMETER=VALUE modification needs to be separated with a comma.
  - Default: ""

- `imp_region[PrMelem1].plug_sv_dumps`
  - This produces a colon separated lists of state variables of a plugin to dump 
  - Use `bench --imp=XXX --plug-in=YYY --imp-info` to get a list of state variables for imp XXX and plug-in YYY
  - Default: ""

- `imp_region[PrMelem1].im_sv_dumps`
  - Specify a comma separated list of state variables of an ionic model to be dumped into the simulation folder.
  - Use `bench --imp=XXX --imp-info` to get a list of state variables for imp XXX.
  - Default: ""

- `imp_region[PrMelem1].plugins`
  - Specify a colon separated list of plug-ins to use with the ionic model. 
  - Query for available plug-ins by calling `bench --plugin-outputs`.
  - Default: ""

- `imp_region[PrMelem1].im_sv_init`
  - Name of the file containing the initial of state variable values.
  - Default: ""

### Example imp_region definitions

In parameter file:
```
num_imp_regions          = 4

imp_region[0].im         = MahajanShiferaw
imp_region[0].num_IDs    = 1
imp_region[0].ID[0]      = 0
imp_region[0].im_param   = "GNa*1"

imp_region[1].im         = MahajanShiferaw
imp_region[1].num_IDs    = 1
imp_region[1].ID[0]      = 1
imp_region[1].im_param   = "GNa*2"

imp_region[2].im         = MahajanShiferaw
imp_region[2].num_IDs    = 1
imp_region[2].ID[0]      = 2
imp_region[2].im_param   = "GNa*4"

imp_region[3].im         = MahajanShiferaw
imp_region[3].num_IDs    = 1
imp_region[3].ID[0]      = 3
imp_region[3].im_param   = "GNa*8"
```

In carputils:
```
cmd += [
    'num_imp_regions',          1,
    'imp_region[0].im',         'Courtemanche'
    'imp_region[0].num_IDs',    1,
    'imp_region[0].ID[0]',      1     # use this setting for the mesh region with tag 1    
]
```

## gregions

Defines the conductivity of regions in your model

- `num_gregions`
  - Total number of regions with different conductivity settings
  - Default: 1
  - Options: Must be greater than 0
  
- `gregion[PrMelem1].ID`
  - Specify a list of model regions (equivalent to mesh element tags) using this conductivity setting
  - Default: 0

- `gregion[PrMelem1].num_IDs`
  - Shows how many model regions use this conductivity setting
  - Default: 0

- `gregion[PrMelem1].name`
  - Name for this conductive region
  - Default: ""

- `gregion[PrMelem1].g_bath`
  - Defines the isotropic conductivity for non-myocardium domain, e.g blood.
  - The absence of anisotropy is assumed for this region. 
  - If anisotropy is required, use a negative tag for the mesh elements in the .elem file and set fibre direction in the .lon file.
  - Default: 1. (S/m)
  - Options: Must be greater than 0.

- `gregion[PrMelem1].g_en`
  - Defines the EXTRACELLULAR conductivity in the local SHEET direction, i.e. perpendicular to the longitudinal fiber `g_el` and transversal fiber directions `g_et`.
  - Default: 0.236 (S/m)
  - Options: Must be greater than 0.

- `gregion[PrMelem1].g_in`
  - Defines the INTRACELLULAR conductivity in the local SHEET direction, i.e. perpendicular to the longitudinal fiber `g_il` and transversal fiber `g_it` directions.
  - Default: 0.019 (S/m)

- `gregion[PrMelem1].g_et`
  - Defines the EXTRACELLULAR conductivity transverse to the FIBER direction i.e. perpendicular to the longitudinal fiber `g_il` and local sheet `g_it` direction.
  - Default: 0.236 (S/m)
  - Options: Must be greater than 0.

- `gregion[PrMelem1].g_it`
  - Defines the INTRACELLULAR conductivity transverse to the FIBER direction i.e. perpendicular to the longitudinal fiber `g_il` and local sheet `g_it` direction.
  - Default: 0.019 (S/m)
  - Options: Must be greater than 0.

- `gregion[PrMelem1].g_el`
  - Defines the EXTRACELLULAR conductivity along the FIBER direction (longitudinal).
  - Default: 0.625 (S/m)
  - Options: Must be greater than 0.

- `gregion[PrMelem1].g_il`
  - Defines the INTRACELLULAR conductivity along the FIBER direction (longitudinal).
  - Default: 0.174 (S/m)
  - Options: Must be greater than 0.

- `gregion[PrMelem1].g_mult`
  - Defines the factor by which all conductivities in the region should be multiplied (after all other modifications are performed).
  - Default: 1.0
  - Options: Must be greater than 0.

### Example gregion definitions

In parameter file:

```
num_gregions             = 1
gregion[0].g_il          = 1.0
gregion[0].g_it          = 1.0
gregion[0].g_in          = 1.0
gregion[0].num_IDs       = 4
gregion[0].ID[0]         = 0
gregion[0].ID[1]         = 1
gregion[0].ID[2]         = 2
gregion[0].ID[3]         = 3

```

In carputils:
```
cmd += [
    '-num_gregions',			1,
    '-gregion[0].name', 		"myocardium",
    '-gregion[0].num_IDs', 	    1,  		# one tag will be given in the next line
    '-gregion[0].ID', 		    "1",		# use these settings for the mesh region with tag 1
    # mondomain conductivites will be calculated as half of the harmonic mean of intracellular
    # and extracellular conductivities
    '-gregion[0].g_el', 		0.625,	# extracellular conductivity in longitudinal direction
    '-gregion[0].g_et', 		0.236,	# extracellular conductivity in transverse direction
    '-gregion[0].g_en', 		0.236,	# extracellular conductivity in sheet direction
    '-gregion[0].g_il', 		0.174,	# intracellular conductivity in longitudinal direction
    '-gregion[0].g_it', 		0.019,	# intracellular conductivity in transverse direction
    '-gregion[0].g_in', 		0.019,	# intracellular conductivity in sheet direction
    '-gregion[0].g_mult',		0.5		# scale all conducitivites by a factor (to reduce conduction velocity)    		
]
```


## stim 

Used to define the stimuli for the simulation. The `Stimulus` options are legacy and replaced with these `Stim`options. Many of the examples are outdated and still use `Stimulus`. 

- `num_stim`
  - number of stimuli
  - Default: 2
  - Options: Must be greater than 0

- `stim[PrMelem1].name`
  - Name of your stimulus

### ptcl options

Defines stimulation protocol

`stim[PrMelem1].ptcl`

- `stim[PrMelem1].ptcl.name`
  - Name of your protocol

- `stim[PrMelem1].ptcl.stimlist`
  - List of activation times defining protocol. Entries separated by comma.
  - The list should be surrounded by quotes. An activation time can also be expressed as an increment to the previous activation, by starting with a '+', e.g.: '0,+240'
  - Default: ""

- `stim[PrMelem1].ptcl.duration`
  - Defines duration of one single stimulation event in milliseconds (ms)
  - Default: ?? (ms)

- `stim[PrMelem1].ptcl.bcl`
  - Defines the basic cycle length for repetitive stimulation. `protocol.duration` must be smaller than `protocol.bcl`, as it specifies the duration of a single stimulation event. The full protocol duration is `protocol.npls` * `protocol.bcl`. This is derived automatically from the user input.
  - Default: ?? (ms)

- `stim[PrMelem1].ptcl.npls`
  - Defines the number of pulses in the stimulation protocol. 
  - The period of pulses can be set with bcl in ms. 
  - If set to 0, the stimulus will have a single instance.
  - Default: ??

- `stim[PrMelem1].ptcl.start`
  - Defines start time of the stimulation protocol (in ms)
  - Default: 0 ms

### crct options

Defines setup and wiring of a stimulation circuit

`stim[PrMelem1].crct`

- `stim[PrMelem1].crct.total_current`
  - strength as total current in uA
  - value between 0 and 1
  - Default: 0

- `stim[PrMelem1].crct.balance`
  - Defines whether the electrode is balancing another electrode. The balance value is interpreted as the electrode index to balance. The waveform is mirrored, but with opposite polarity. If the balance value is -1, no balancing is applied.
  - value between -1 and `num_stim` - 1
  - Default: -1

- `stim[PrMelem1].crct.type`
  - Defines the physics of the actual source used for stimulation and in some cases also the wiring of the electric stimulation circuit.
  - Default: 0
  - Options:
    - 0: transmembrane current (use uA/cm^2)
    - 1: extracellular current (use uA/cm^3)
    - 2: extracellular voltage (closed loop, use mV)
    - 3: extracellular ground (enforce 0 mV)
    - 4: intracellular current
    - 5: extracellular voltage (open loop, use mV)
    - 6: illumination (use mW/mm^2)
    - 9: Vm clamp (use mV)

### elec options

Defines electrode geometry

`stim[PrMelem1].elec`

- `stim[PrMelem1].elec.dump_vtx_file`
  - For volume based electrode definitions, vertices of this electrode are dumped to a file.
  - Default: 0
  - Options: 0 and 1 ??

- `stim[PrMelem1].elec.geom_type`
  - This value defines the geometry used for the electrode. For now only predefined geometries are supported.
  - Default: 2
  - Options:
    - 1: sphere
    - 2: block
    - 3: cylinder
    - 4: element list (volume or surface), not implemented
    - 5: vertex list, not implemented??

- `stim[PrMelem1].elec.radius`
  - Sets radius of sphere or cylinder geometry. 
  - Ignored for Box geometry. Use Ta- gregion.p1 and Ta- gregion.p0 instead.
  - Default: 100 um

- `stim[PrMelem1].elec.p1`
  - If box geometry, this is the upper right corner of box.
  - If cylinder geometry, describes end of cylinder main axis
  - Box & cylinder needs additional param electrolde
  - Sphere and cylinder need electrode.radius specified
  - Default: 0 um

- `stim[PrMelem1].elec.p0`
  - If sphere geometry, this is the center of the sphere
  - If box, defines lower left corner of box
  - If cylinder, defines start of the cylinder main axis
  - For sphere and cylinder, electrode.radius also needed
  - For box and cylinder, electrode.p1 also needed
  - Default: (0, 0, 0)

- `stim[PrMelem1].elec.vtx_fcn`
  - Whether or not electrode is spatially constant or varying
  - Default: 0
  - Options:
    - 0: homogenous
    - 1: inhomogenous

- `stim[PrMelem1].elec.vtx_file`
  - File name allowing a vertex based electrode definition. Using a non-empty stringswitches to vertex-based definition and ignores other settings. For file format specifications check the first chapters of the openCARP manual.
  - Default: ""

- `stim[PrMelem1].elec.domain`
  - Tissue domains affected
  - Default: 1
  - Options:
    - 1: myocardium only
    - 2: Purkinje only
    - 3: all

- `stim[PrMelem1].elec.geomID`
  - Region ID defining electrode's geometry. Standard block definitions used if set to -1.
  - Default: -1

### pulse options

Defines stimulation pulse

`stim[PrMelem1].pulse`

- `stim[PrMelem1].pulse.bias`
  - Constant term added to the stimulus pulse
  - Default: 0

- `stim[PrMelem1].pulse.tau_plateau`
  - Defines a time constant governing plateau of pulse (>10^5 is infinite).
  - A larger tau_plateau will result in a longer plateauphase.
  - Default: 1000 ms

- `stim[PrMelem1].pulse.tau_edge`
  - Defines the time constant governing leading and trailing edge of pulse.
  - A larger tau_edge will result in a fast drop, while a smaller tau_edge will have a longer exponentialy decreasing flank.
  - Formulation used resembles the equation: Amp * (1 - exp( -t/tau_edge)) * exp( -t/tau_plateau )
  - Default: 0.01 ms

- `stim[PrMelem1].pulse.s2`
  - Value only used for defining biphasic pulse
  - It's a relative value and defines the stimulus strength of the trailing pulse (after 0 crossing) relative to leading pulse (from start to 0 crossing).
  - Giving value of 0 will result in a flip of polarity
  - Default: 0 
  - Options: must be between 0 - 10

- `stim[PrMelem1].pulse.tilt_ampl`
  - prescribe fixed tilt amplitude (as in real shock delivery devices by capacitive discharge)
  - Default: 1.0
  - Options: must be between 0 - 1.0

- `stim[PrMelem1].pulse.tilt_time`
  - prescribe fixed tilt time (not value) relative to pulse duration
  - Default: 1.0
  - Options: must be between 0 - 1.0

- `stim[PrMelem1].pulse.strength`
  - Defines strength of prescribed stimulation. 
  - Strength is defined as amplitude of signal. 
  - If dealing with currents, it is defined as uA/volume. 
  - If dealing with voltages it is defined as mV. 
  - To create a stimulation the minimum requirement is a given `pulse.strength` and a `pulse.duration`
  - Default: 0. (uA/cm^2 for 2D, uA/cm^3 for 3D, or mV)

- `stim[PrMelem1].pulse.file`
  - Reads in pulse definition from external file.
  - For file format specifications check the first chapters of the openCARP manual.
  - Default: ""

- `stim[PrMelem1].pulse.shape`

  - Defines the shape of the prescribed pulse if an implemented pulseform is chosen (see choices below). To impose a manually created stimulus use the pulse.file functionality.
  
  - By default, the pulse shape is defined as a monophasic pulse of square-like shape of a certain duration and strength. There are two time constants:
  
  - `tau_edge`, governs the leading and trailing edges of the pulse.
  
  - `tau_plateau`, governs the plateau phase.

  - By default, these time constants are set to yield essentially a square-like pulse shape, but can be easily adjusted to generate truncated exponential-like pulses. Biphasic shapes are specified by two additional parameters, the duration of the first part of the pulse relative to the duration of the entire pulse (specified by `pulse.d1` in range of [0,1] ), and the strength of the second part of the pulse relative to the strength of the first part (specified by `pulse.s2` in range of [0,1] ).

  - To create a stimulation the minimum requirement is a given pulse.duration and pulse.strength

  - Default: 0
  - Options:
    - 0: square wave
    - 1: truncated exponential wave
    - 2: sine wave

- `stim[PrMelem1].pulse.name`
  - Name of your specified pulse waveform


### Example stimulus definition

If providing a stimulation parameter file:

```
# stimulus setup
num_stim = 1 
stim[0].name = "S1"
stim[0].crct.type =  0 			# stimulate using transmembrane current 
stim[0].pulse.strength = 250.0	# uA/cm^2
stim[0].ptcl.start = 0.			# apply stimulus at t=0ms
stim[0].ptcl.duration = 2.		# stimulate for 2ms
```
In carputils:

```
stim = [
    '-num_stim',                1,
    '-stim[0].name',            'S1',
    '-stim[0].crct.type',       0,
    '-stim[0].pulse.strength',  args.S1_strength,
    '-stim[0].ptcl.start',      0,
    '-stim[0].ptcl.duration',   args.S1_dur
]
```