# Core Layout
- Card Name
- Card Cost
- Card Type
	- Hero
	- Skill
	- Tool
	- Location
- Card Classes
	- Summoning
	- Augmentation
	- Alchemy
	- Manifestation
	- Mage
	- Martial
	- Melee
	- Ranged
	- Weapon
	- Tech
	- Lore
	- Etc.
- Initial Energy
- Strength/Endurance
- Ability Text
	- Open ended, multiple entries per card

# Keywords
## Attack
This is the basic attack function. Target an opponent’s Hero. The attacking Hero and the target both deal damage equal to their strength to each other. This ability uses the stack.
## Reactive
This is a keyword that abilities can have. It enables them to be used while there are actions on the stack.
## Alert
This is a flag identify characters who can always react. For example, an ability might say “Attack. Heroes without *Alert* cannot react.” Alert does nothing on its own, it is only meant to be referenced by other effects.
## Protect
If this is on an ability that does not have Reactive, then I don't think it does anything. Protecting a Hero prevents the next instance of damage that would apply to them.
# Surprise
This is a keyword that goes on Skills, and it allows you to attach the Skill to a Hero as a reaction and use the Skill. This does not circumvent a Hero requiring Alert.
# Reach
When a character with Reach is involved in an attack, they do not receive combat damage except from other Heroes who also have reach.
# Together*
I'm not sure about this one as a keyword, but the intended usage is for "attacking together" and means that all "togethering" creatures are involved in a single combat instance. The ideas is that a "function," like Attack, is something only one Hero does at a time, and "together" is a modifier that brings another Hero into that function.

In the case of attacking, it works like the new MTG rules: Damage is dealt simultaneously, and both sides distribute their damage however they would like. There is no need to chose a blocker order or have trample or anything like that, any Hero can damage any number of other Heroes as long as they have that much damage to deal.
# Stun
Put a stun counter on the target. If a Hero would untap while it has a stun counter, it instead does not and removes the stun counter. A character with a stun counter cannot deal damage.