# Mewgenics' .gon functionality.
### UNDER HEAVY CONSTRUCTION

## Summary of GON formatting.
These are just simpler JSON. Only supports character strings (the "s are optional) and numbers only. Line breaks denote ends of line. {}s are still used to denote scopes. 
Check https://github.com/NancokPS2/mewgenics-gon-map/blob/main/gon_description.txt for more information.

### Hardcoded object types (not modifyiable trough .gon files, still usable in other definitions)
- Status effects
- Map layout
- Effects

## Property layout
These are the identifiers and fields that can be used in the files for Mewgenics specifically. (or at least the ones that will have any effect).
Here, they are laid out as a tree format to denote fields and their hierarchy.
Elements that are further to the left (have less indentation) are always dictionaries, and as such contain other fields inside of them

Example of an ability:
- meta (dict)
  - name (str)
  - class (str)
- damage_instances (dict)
  - effects (list)

  
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
			DieSafe 1
    }
  }

}
```

### Glossary
(str) = String of characters.  
(bool) = Boolean, usually true or false. Altho numbers can also be treated as booleans (0 or less = false, 1 or more = true)
(num) = Number.
(list) = Can have any amount of entries.  
(dict) = Dictionary, it contains other fields inside. Elements that are further to the left (have less indentation) and have elements below them are always dictionaries, and as such contain other fields inside of them. I won't be noting them down below.  

#### Abilities
- meta: Mostly information displayed to the player.
  - name(str): The key for the name string found in the translation file inside the text folder.
  - desc(str): The same as name, but for the description.
  - class(str): Defines the color of the background, doesn't seem to affect anything else since the level up choice pools are decided separately.
  - type_icon(str): A small icon overlayed on the ability supposed to denote the nature of the ability.
- graphic: The visuals caused by the ability in combat.
  - animation(str): The animation that the unit uses when using the ability.
- cost: Requirement for using the spell, it can be bypassed by effects that automatically cast spells.
  - infcantrip(bool): If false, this can only be used once per turn.
  - mana(num): The amount of current mana required to use the ability.
- damage_instance: Object containing what will happen to each target hit. But doesn't have to necessarily deal damage, nor does it seem to always trigger damage-based passives.
  - effects(list): Every effect that will trigger on the target due to this instance 
