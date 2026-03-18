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
(num) = Number. It can be replaced by equations. Such as 2+variable_name 
(list) = Can have any amount of entries, separated by line breaks.  
(array) = Contains any amount of values separated by spaces and surrounded by []s. If there's only 1 element, the []s can be skipped.
(dict) = Dictionary, it contains other fields inside. Elements that are further to the left (have less indentation) and have elements below them are always dictionaries, and as such contain other fields inside of them. I won't be noting them down below.  

#### Abilities
- template(str): Copies all of the values from the given ability.
- variant_of(str): Seems to be the same as template. But it is typically used for upgraded versions.
- meta: Mostly information displayed to the player.
  - name(str): The key for the name string found in the translation file inside the text folder.
  - desc(str): The same as name, but for the description.
  - class(str): Defines the color of the background, doesn't seem to affect anything else since the level up choice pools are decided separately.
  - type_icon(str): A small icon overlayed on the ability supposed to denote the nature of the ability.
 
- graphics: The visuals caused by the ability in combat.
  - sync_speed(num): Looks like a modifier for the speed, since it is based on other factors like the cat's speed and tile being traversed.
  - max_tiles_single_loop(num): How many tiles can be traversed before the ability loops. Used by the Roll spell.
  - move_start_animation(str): An animation to play before the main animation plays.
  - move_end_animation(str): Same, but for after the main animation plays.
  - animation(str): The animation that the unit uses when using the ability.
  - particle(str): A particle to spawn at the target location.
  - projectile(str): Seemingly launches a projectile from the source to the target.
  - single_projectile(bool): Seems to prevent AoE abilities from launching a projectile at each targeted tile. Instead firing only at the targeted tile.

- cost: Requirement for using the spell, it can be bypassed by effects that automatically cast spells.
  - infcantrip(bool): If false, this can only be used once per turn.
  - mana(num): The amount of current mana required to use the ability.
  - act_points(num): Unclear. Seems like every ability starts with 1 act_points, so by setting this to 1, you can limit its use to once per turn.
  - charge(num): Unknown

- target: Denotes the tiles that will be targeted when this is aimed.
  - target_mode(str): Seems like a general filter of what can be selected as the target.
  - range_mode(str): The shape of the AoE (i assume this is a diamond-shaped area by default)
  - max_range(num): How far away the ability can reach. Seems to allow using "mov" as the value which may refer to movement range.
  - min_range(num): Can't target tiles closer than this. Used by hunter's default attack
  - aoe_mode(str): The shape of the AoE
  - max_aoe(num): How big the AoE can be
  - restrictions(array): Simple identifiers that add limitations to what the ability can hit
  - straight_shot(bool): Unknown. May refer to line of sight requirements.
  - range_display_include_character_size(bool): Possibly modifies the targeting displayed to account for the size of the character.
  - aoe_considers_character_size(bool): Same as above, but for AoE
  - aoe_excludes_self(bool): Prevents selecting yourself as the target.
  - knockback_mode(str): Unknown.
 
- temporary_effects(list): Seems the same as the effects from damage_instance. But only until the ability resolves and applied to the user.

- damage_instance: Object containing what will happen to each target hit. But doesn't have to necessarily deal damage, nor does it seem to always trigger damage-based passives.
  - type(str): Unknown. 
  - heal(num): Recovers health
  - damage(num): The amount of damage dealt.
  - piercing(bool): Ignores shield.
  - knockback(bool): Tiles to push the target back
  - makes_contact(bool): Mostly for things like thorns.
  - override_trample_damage(bool): If the user tramples during the use of this ability. The trample damage is replaced by the damage of this effect and ignores the trample stacks.
  - effects(list): Every effect that will trigger on the target due to this instance
  - elements(array): The elements associated with the ability.
- bonus_passives(list): Gives any amount of passives to the target, unclear if it lasts past the current fight. It is also unclear if effects are also accepted for this list.

## Identifier list

### Ability variables    
These are identifiers that count as values, and can be used in number math.  
- bonus_ranged_damage: Seems to be the extra damage granted by Dexterity

### Ability type
- none
- spell
- melee

### Target mode
- tile: Tiles that are not occupied(?)

### Range mode
- cross: Like Thief's basic attack
- ground_mode: The range becomes anywhere you could reach walking.

### AoE mode
- standard: Diamond shape?
- occupied_tiles: Only allows tiles which have something(?) in them.

### Knockback mode
- character_to_tile: Unknown

### Target restriction
- must_be_moveable
- must_move
- must_have_ally

### Type icon
- defense
- ranged

### Animation
- none
- brace
- thinking
- backflipLoop
- showclaws: Potentially removed.
- coinFlip
- pointout
- throwArrow
- throwobject

### Particle
- fx_statup
- HealSmall
- MagicMissileBlast

### Projectile
- ArrowProjectile
- spitprojectile

### Elements
- Holy
- Water

### Spawnable "things"
- BigFood
- BiggestFood

### Effects
These all take numbers as their value. All effects have a number value that denotes their stacks unless stated otherwise. Some effects use up their stacks instantly and as such are never displayed in-game.
- Sleep
- Shield
- AllStatsUp
- StrengthUp
- DexterityUp
- IntelligenceUp
- ConstitutionUp
- ManaSteal: Takes mana from a unit. Seems like this steals ALL mana at -1 stacks
- RandomStatUp
- Thorns
- Marked
- Fury: Chance to repeat the attack. The number correlates to the repeat chance, from 0 to 100.
- TempSpellDamageUp
- NextAttacKBonusRange: Increases the range of the next hit.
- NextAttackSpecialCrit: Has special effects the next time an attack crits. Used by Gamble. This effect can either use a simple number or be treated as a dictionary.
  - extra_coins_per_stack(num)
  - crit_multiplier_bonus(num)
  - luck_increase(num)
- ConjureBonusAbility: This seems to have the value of an ability pool (str) to draw from and use. Used by Ponder's upgrade.
- CritChanceUp: The number correlates to the dodge chance, from 0 to 100.
- ReduceManaCost
- ReduceManaCostExcludeBrainstorm: Funi hardcoding. Used exclusively by brainstorm to ensure it cannot make itself cheaper.
- TilesMovedToStrength: Potentially removed.
- TilesMovedToMana: Potentially removed.
- TilesMovedToNeighborHeal: Potentially removed. It seems to read from a "level" variable instead of a literal value.
- TilesMovedToCritChance: Potentially removed.
- NextAbilityHeals: Potentially removed.
- NextDamageReduceAndHealAllies: Potentially removed.
- KillsToManaThisTurn: Potentially removed.
  - mana(num)
  - stacks(num)
  - allstats_increase(num)
- KillsToMeatAndHealthThisTurn: Potentially removed
  - stacks(num)
  - count(num)
- Temporary: This is a special effect that causes a status effect
  - status(str): The identifier of the status effect to apply
  - stacks(num): The stacks of the status
  - turns(num): Turn duration, once these turns pass you loose all stacks.
  - expires_on_begin_turn(bool)
- ApplyToSourceOnKill: This is a special effect that applies an effect to the user when it kills something. Defined as a list just like regular effects are.
- SpawnThingIfHitKills: Spawns something on kill. Defined as a str

### Passives
- HealthRegenUp(num)
