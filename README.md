# Mewgenics' .gon functionality.
### UNDER HEAVY CONSTRUCTION

## Summary of GON formatting.
These are just simpler JSON. Only supports character strings (the "s are seemingly optional) and numbers. Line breaks denote ends of line. {}s are still used to denote scopes. It also supports comments with // 
Check https://github.com/NancokPS2/mewgenics-gon-map/blob/main/gon_description.txt for more information.
Files can have `.patch`, `.merge` or `.append` at the end of their names to let the game know how to treat its contents. Example: `classes.gon` to `classes.gon.patch`.  

When using `.patch` on the file name. You may selectively add `.overwrite`, `.append`, `.merge`, `.add` or `.multiply` to the identifiers themselves to choose an operation for them specifically. Example: 
```
Backflip {
	effects.append {
		Thorns 1
	}
}
```
Adds 1 stack of Thorns to the usual effects of backflip.

### CSV/text changes.
As of the current version of the game, all strings are stored in the same .csv file. Which seems to also be compatible with the .append
It is possible to add and even override strings by using a `combined.csv.append` file. Adding strings that already exist will cause them to take priority over the ones in the base game (last matching key is chosen)

## Property layout explanation
In the following sections are the identifiers and fields that can be used in the files for Mewgenics specifically. (or at least the ones that will have any effect and have been discovered).
In this document, they are laid out as a tree format to denote fields and their hierarchy.
Elements that are further to the left (have less indentation) are always dictionaries, and as such contain other fields inside of them.

Example of an ability:
- AbilityName (dict)
  - meta (dict)
    - name (str)
    - class (ident)
  - damage_instances (dict)
    - effects (list)

  
Which translates to this in the .gon file:

  
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

### Value types  
- (str) = An arbitrary string of characters, encased between "s. `SomeString "Text of the string"` 
- (ident) = The identifier of another, appropiate object. The exact kind of object it can use varies per field. `SomeObject IdentifierOfAnotherObject`  
- (bool) = Boolean, usually true or false. Altho numbers can also be treated as booleans (0 or less = false, 1 or more = true) `explodes true`  
- (num) = Number which can have decimals separated by a dot (.) (most things use integers, use decimals with care). It can be replaced by equations. Such as 2+variable_name 
- (list) = Same syntax as a dictionary. But does not have defined fields. It instead expects raw values or objects. ``  
- (array) = Contains any amount of values separated by spaces and surrounded by []s. If there's only 1 element, the []s can be skipped.  
- (dict) = Dictionary/objects, it contains other fields inside and are denoted by the following {}s. Elements that are further to the left (have less indentation) and have elements below them are always dictionaries, and as such contain other fields inside of them. I won't be noting them down below as it will be obvious from its sub-items.  

### Most identifiers represent objects, objects can be initialized in various ways
Abilities, passives, statuses, weathers and other objects (denoted by an identifier) can have a value after them to "initialize them", which essentially sets their initial value.
Statuses typically support a number that defines their stacks, like `Bleed 1`. But the most powerful way to create an object is with a dictionary. Example:
```
//This is a special ability effect
Temporary {
	status Brace
	stacks 1
	turns 1
}
```
Do note i can only document properties that are modified in the existing .gon files of the game. There may be more fields which i don't know about due to being touched solely within the code.


### Hardcoded object types (not modifyiable trough .gon files, still usable in other definitions)
- Map layout

### Property lists

#### Ability properties
- template(ident): Copies all of the values from the given ability template. While templates use a different naming convention and are named differently. There's nothing to denote they are any different from abilities in other files. Meaning it should be possible to use any ability as a template
- variant_of(ident): Seems to be the same as template. But it is typically used for upgraded versions.
- meta: Mostly information displayed to the player.
  - name(str): The key for the name identing found in the translation file inside the text folder.
  - desc(str): The same as name, but for the description.
  - class(ident): Defines the color of the background, doesn't seem to affect anything else since the level up choice pools are decided separately.
  - type_icon(str): A small icon overlayed on the ability supposed to denote the nature of the ability.
 
- tags(array): Contains simple keywords that other abilities can use to identify the object.

- graphics: The visuals caused by the ability in combat.
  - sync_speed(num): Looks like a modifier for the speed, since it is based on other factors like the cat's speed and tile being traversed.
  - max_tiles_single_loop(num): How many tiles can be traversed before the ability loops. Used by the Roll spell.
  - move_start_animation(ident): An animation to play before the main animation plays.
  - move_end_animation(ident): Same, but for after the main animation plays.
  - animation(ident): The animation that the unit uses when using the ability.
  - particle(ident): A particle to spawn at the target location.
  - projectile(ident): Seemingly launches a projectile from the source to the target.
  - single_projectile(bool): Seems to prevent AoE abilities from launching a projectile at each targeted tile. Instead firing only at the targeted tile.
  - ignore_slowtiles(bool): Causes the animation to ignore the slowdown caused by tiles (if the user moves trough one during its use)
  - chain_movieclip(ident): Unknown.
  - chain_distance(num): Unknown. Seems to be defined as a decimal.
  - use_projectile(bool)
  - affected_particle(ident)
  - lob_height(num): Seems to affect the arc of thrown projectiles.
  - animation_out(ident): Unknown, used by Tunnel to define some sort of end animation.
  - animation_in(ident): Same as above, but for the start.

- cost: Requirement for using the spell, it can be bypassed by effects that automatically cast spells.
  - infcantrip(bool): If false, this can only be used once per turn.
  - mana(num): The amount of current mana required to use the ability.
  - act_points(num): Unclear. Seems like every ability starts with 1 act_points, so by setting this to 1, you can limit its use to once per turn.
  - charge(num): Unknown
  - once_per_fight(bool)

- target: Denotes the tiles that will be targeted when this is aimed.
  - target_mode(ident): Seems like a general filter of what can be selected as the target.
  - range_mode(ident): The shape of the AoE (i assume this is a diamond-shaped area by default)
  - max_range(num): How far away the ability can reach. Seems to allow using "mov" as the value which may refer to movement range.
  - min_range(num): Can't target tiles closer than this. Used by hunter's default attack
  - aoe_mode(ident): The shape of the AoE
  - max_aoe(num): How big the AoE can be
  - restrictions(array): Simple identifiers that add limitations to what the ability can hit
  - straight_shot(bool): Unknown. May refer to line of sight requirements.
  - range_display_include_character_size(bool): Possibly modifies the targeting displayed to account for the size of the character.
  - aoe_considers_character_size(bool): Same as above, but for AoE
  - aoe_excludes_self(bool): Prevents selecting yourself as the target.
  - knockback_mode(ident): Unknown.
  - allow_diagonals(bool): Allows targeting to "fit" trough diagonal gaps between 2 objects which are only diagonally connected. 
 
- temporary_effects(list): Seems the same as the effects from damage_instance. But only until the ability resolves and applied to the user.

- damage_instance: Object containing what will happen to each target hit. But doesn't have to necessarily deal damage, nor does it seem to always trigger damage-based passives.
  - type(ident): Unknown. 
  - heal(num): Recovers health
  - damage(num): The amount of damage dealt.
  - piercing(bool): Ignores shield.
  - knockback(bool): Tiles to push the target back
  - cant_miss(bool)
  - makes_contact(bool): Mostly for things like thorns.
  - override_trample_damage(bool): If the user tramples during the use of this ability. The trample damage is replaced by the damage of this effect and ignores the trample stacks.
  - effects(list): Every effect that will trigger on the target due to this instance
  - elements(array): The elements associated with the ability.
- bonus_passives(list): Gives any amount of passives to the target, unclear if it lasts past the current fight. It is also unclear if effects are also accepted for this list.

## Object identifier list

### Ability variables    
These are identifiers that count as values, and can be used in number math.  
- bonus_ranged_damage: Seems to be the extra damage granted by Dexterity
- aux: Unknown.
- x: A number variable that is empty by default and can be set within the ability definition.
- str: Strength
- dex: Dexterity
- int: Intelligence
- mov: Speed?
- con: Consitution
- cha: Charisma
- lck: Luck

### Tags

#### Ability
- noncopyable
- musical

### Ability type
- none
- spell
- melee
- ranged
- status_spell
- misc
- unknown

### Target mode
- tile
- direction
- random_tile

### Range mode
- cross: Cardinal directions
- diagcross: Like cross, but diagonal.
- ground_move: The range becomes anywhere you could reach walking.

### AoE mode
- standard: Diamond shape?
- occupied_tiles: Only allows tiles which have something(?) in them.
- line

### Knockback mode
- character_to_tile
- character_to_tile_4snap
- pull_to_character

### Target restriction
- must_be_moveable
- must_move
- must_have_ally
- must_have_animate_character: Seems to expect the target to be alive? Used by Rat Roulette

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
- poopfart
- hopinplace
- thumbsup
- cheat
- castspell
- proud
- pissYourself
- dig
- digUp
- digDown
- roll
- startroll
- endroll
- buttScootStart
- buttScootLoop
- buttScootEnd

### Particle
- fx_statup
- HealSmall
- MagicMissileBlast
- Wave

### Projectile
- ArrowProjectile
- spitprojectile
- HookProjectile
- Fishball
- SmallRock

### Elements
- Holy
- Water

### Spawnable "things"
- BigFood
- BiggestFood
- CharmedMaggot
- Poop

### Tiles
- WaterTile

### Effects
**It is currently unclear which effects are valid as passives, statuses or ability effects, likely candidates for passives are denoted with a \* preffix.**  
All effects have a number value that denotes their stacks unless stated otherwise. Some effects use up their stacks instantly and as such are never displayed in-game. All effects seem to need at least 1 as their number to have any effect (unless they are a (dict)). Example: `Bleed 1`
Seems like effects also support setting a chance alongside their number, using an array where the stacks is the first value and the chance is the second one (this is the chance of the effect being applied). Example: `Stun [1 .15]`


#### Debuffs
- Sleep
- Confusion
- Bleed
- Marked
- Sleep
- DoubleStatus: Doubles the stacks of the given "status".

#### Buffs
- AllStatsUp
- StrengthUp
- DexterityUp
- IntelligenceUp
- ConstitutionUp
- SpeedUp
- CharismaUp
- LuckUp
- RandomStatUp
- Thorns*
- CritChanceUp: The number correlates to the dodge chance, from 0 to 100.
- Fury*: Chance to repeat the attack. The number correlates to the repeat chance, from 0 to 100.
- TempSpellDamageUp
- Reflect*
- HealthRegenUp*
- IgnoreTiles*
- Temporary: This is a special effect that causes a status effect for a limited time
  - status(ident): The identifier of the status effect to apply
  - stacks(num): The stacks of the status
  - turns(num): Turn duration, once these turns pass you loose all stacks.
  - expires_on_begin_turn(bool)
- Cleanse: Removes all debuffs and gives divine shield per cleared debuff. Use 0 to disable gaining shields while still cleansing the target.

#### Damaging
- RevengeDamage*: Causes an effect on an attacker. Works similar to damage_instance
  - effects

#### Resource change
- Shield: Instantly recovers shield equal to the number.
- ManaSteal: Takes mana from a unit. Seems like this steals ALL mana at -1 stacks
- ReduceManaCost
- ReduceManaCostExcludeBrainstorm: Funi hardcoding. Used exclusively by brainstorm to ensure it cannot make itself cheaper.
- RefreshMovePoints: Restores movement.
- RefreshActPoints: Restores basic attack.
- RefreshItemAbilities: Restores item use.
- TakeExtraTurn: Gives a free turn after the current one.

#### Mobility
- Displace: The target is moved by a tile in a random direction. Used by Dump

#### Misc
- FaceAway
- CollectsPickups
- ConjureBonusAbility: This seems to have the value of an ability pool (ident) to draw from and use. Used by Ponder's upgrade.
- SafeDie: Downs the cat without injuries.
- ReviveNextRound: The numbers represents how many turns before the cat revives. Higher numbers are worse.
- CopyCatPassive_Initializer: Special effect for making CopyCat start copying spells. Should be used as a bonus_passive. The number can be 1 or 2 for the unupgraded or upgraded version respectively.
- Metronome: Uses a random spell. The number can be 1 or 2 for the unupgraded or upgraded version respectively.
- CanceledQueuedInput: Unknown. It is used by some set bonuses that trigger a similar effect to Second Wind, like the Fighter item set.
- ChangeTile: Replaces the targeted tile. Takes an ident

#### Hit effects
- NextAttacKBonusRange: Increases the range of the next hit.
- ApplyToSourceOnKill: This is a special effect that applies an effect to the user when it kills something. Defined as a list just like regular effects are.
- NextActionLuckUp: The number corresponds to the luck gained until next hit.
- NextAttackSpecialCrit: Has special effects the next time an attack crits. Used by Gamble. This effect can either use a simple number or be treated as a dictionary.
  - extra_coins_per_stack(num)
  - crit_multiplier_bonus(num)
  - luck_increase(num)

#### Spawning
- LeaveBehind: This is an special kind of list that defines the type of "thing" to leave behind, then its identifier. The one example i found is `object CharmedMaggot`  
- ObjectOnHit: Same as above, may not work if the target area is occupied. Used by Dump
- SpawnThingIfHitKills: Spawns something on kill. Defined as an ident
- BounceObject: Throws an object at the target which bounces off them, dealing damage. Defined as an ident. Used by Stun (the spell)

#### Potentially removed/unused. There's no guarantee these will work.
- TilesMovedToStrength
- TilesMovedToMana
- TilesMovedToNeighborHeal: It seems to read from a "level" variable instead of a literal value.
- TilesMovedToCritChance
- NextAbilityHeals
- NextDamageReduceAndHealAllies
- KillsToManaThisTurn
  - mana(num)
  - stacks(num)
  - allstats_increase(num)
- KillsToMeatAndHealthThisTurn
  - stacks(num)
  - count(num)

### Effect conditions
These are special objects that "contain" the actual effect and act as a requirement for the effect to go off.
Example:
```
effects {
  Conditional_BossOrBig {
    Immobile [1 .25]
  }
}
```

- Conditional_BossOrBig
- Conditional_NotBossOrBig
