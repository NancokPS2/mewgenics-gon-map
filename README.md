# Mewgenics' .gon functionality.
### UNDER HEAVY CONSTRUCTION

## Summary of GON formatting.
These are just simpler JSON. Only supports character strings (the "s are optional) and numbers only. Line breaks denote ends of line. {}s are still used to denote scopes. 
Check https://github.com/NancokPS2/mewgenics-gon-map/blob/main/gon_description.txt for more information.

### Hardcoded object types (not modifyiable trough .gon files, still usable in other definitions)
- Status effects
- Map layout

## Properties
These are the identifiers and fields that can be used in the files for Mewgenics specifically. (or at least the ones that will have any effect).
Here, they are laid out as a tree format to denote blocks and the values inside said blocks
Example of an ability:
- meta
  - name
  - class
- damage_instances
  - effects
Translates to this in the .gon file:
```
AbilityIdentifier {

  meta {
    name "ABILITY_IDENTIFIER_NAME"
    class Colorless
  }

  damage_instance {
    effects {
      Shield 5
    }
  }

}
```

### Abilities
- meta: Mostly information displayed to the player.
  - name: The key for the name string found in the translation file inside the text folder.
  - desc: The same as name, but for the description.
  - class: Defines the color of the background, doesn't seem to affect anything else since the level up choice pools are decided separately.
  - type_icon: A small icon overlayed on the ability supposed to denote the nature of the ability.
- graphic: The visuals caused by the ability in combat.
  - animation: The animation that the unit uses when using the ability.
