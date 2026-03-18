# Mewgenics' .gon functionality.
### UNDER HEAVY CONSTRUCTION

## Summary of GON formatting.
These are just simpler JSON. Only supports character strings (without the "s) and numbers only. Line breaks denote ends of line. {}s are still used to denote scopes. 
Check https://github.com/NancokPS2/mewgenics-gon-map/blob/main/gon_description.txt for more information.

### Hardcoded object types (not modifyiable trough .gon files, still usable in other definitions)
- Status effects
- Map layout

### Basic properties
#### Abilities
##### Sections
- meta: Mostly information displayed to the player.
  - name: The key for the name string found in the translation file inside the text folder.
  - desc: The same as name, but for the description.
  - class: Defines the color of the background, doesn't seem to affect anything else since the level up choice pools are decided separately.
  - type_icon: A small icon overlayed on the ability supposed to denote the nature of the ability.
- graphic: The visuals caused by the ability in combat.
  - animation: The animation that the unit uses when using the ability.
