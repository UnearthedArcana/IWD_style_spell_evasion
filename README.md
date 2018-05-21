<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" lang="en" xml:lang="en">
<head>
<title>IWD-style Spell Evasion</title>
<meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1" />
<link rel="stylesheet" href="style/g3readme_cam.css" type="text/css" />
<link href="style/g3icon.ico" rel="icon" type="image/bmp" />
</head>
<body>
<h1>Add IWD's "Evasion" ability to BGEE/BG2EE</h1>
<div class="section">
  <p><strong>Author:</strong> <a href="http://forums.gibberlings3.net/index.php?showuser=6306">Duns Scotus, the SubtleDoctor</a><br />
</p>
  <p><strong> Version 2.2 </strong></p>
</div>
<h2>WHAT THIS IS</h2>
<div class="section">
  <p>This is a drop-in utility that lets anyone add the IWD "Evasion" feat to their BGEE or BG2EE game.  A character with this feat, when targeted by certain magic spells, can make a saving throw vs. Breath Weapon to avoid the effect altogether.  The spell only works on them if the character fails that save... and then the spell itself may offer its own normal saving throw to avoid or reduce the effect (e.g. Fireball).</p>
  <p>The way this works is, it patches a spell - let's say Fireball - with a 177 effect calling an .EFF file <i>before</i> the rest of the spell's effects take place.  The .EFF is created by this function, with an opcode 324 effect protecting the target against the effects of Fireball, IF the target has the Evasion spellstate.  Then the function creates another 324 effect in Fireball, <i>before</i> the 177 effect.  <i>This</i> 324 effect protects against that .EFF... but only if the target fails a saving throw.</p>
  <p>So the spell has an effect making targets with the Evasion state immune to it, but those targets must make a saving throw for their immunity to kick in.  The functional result is exactly like IWD's Evasion: you get an extra saving throw to avoid the effects of a spell.</p>
  <p>This version is more generalized, and thus a lot more versatile, than the actual version of Evasion in IWDEE.  This one can use any arbitrary spellstate, not just the Evasion spellstate; and it can use whichever saving throw you like; and you can apply different variants of the Evasion effect however and however much you like.  So you can let thieves evade fireballs, and berserkers evade charms, and you can switch the "Shield" spell to give an extra save to avoid Magic Missiles, instead of full immunity, etc. This besically can be used to give anybody an extra saving throw to avoid any spells or spell effects.
</div>
<h2>HOW TO USE THIS</h2>
<div class="section">
  <p>If you don't already have one, make a folder called "lib" in your mod's main folder.  Drop the "spell_evasion" folder there.  Unless you want to edit the function itself, the placement of this folder is quite strict.
  <p>Now you need to do two things:<br />
  <p>1) Activate the Evasion function with the following code:</p>
    <div class="kit_description">
	INCLUDE ~%MOD_FOLDER%/lib/spell_evasion/spell_evasion.tpa</b></p>
	</div>
  <p>2) Create a spell or other effect that sets the Evasion spellstate for some character.  NOTE: this does <b>not</b> have to be the actual Evasion spellstate (252)... you can add a new spellstate and use that one.  This allows you to use the basic "extra chance to save" mechanic in different ways, against different spells, with different saving throws, for different characters.  See below for more information.</p>
  <p>For example, I create a spell to grant the Evasion ability permanently like this:
    <div class="kit_description">
	ACTION_IF NOT FILE_EXISTS_IN_GAME ~d5evade.spl~ BEGIN<br />
	  COPY_EXISTING ~spcl221.spl~ ~override/d5evade.spl~<br />
		SAY NAME1 ~ ~<br />
		SAY UNIDENTIFIED_DESC ~ ~<br />
		LPF DELETE_EFFECT INT_VAR match_probability1 = 100 END<br />
		LPF ADD_SPELL_EFFECT INT_VAR opcode = 328 target = 1 parameter2 = 252 timing = 9 special = 1 END<br />
	  BUT_ONLY<br />
	END</p>
	</div>
  <p>3) Apply the Evasion passive ability to some character.  You can add it to a kit ability table, or make it an HLA, or make it a temporary effect of a custom spell... you can apply it directly to .CRE files... whatever you want.  You just need to apply that 328 effect to set the Evasion spellstate to someone.
  <p>4) Apply the Evasion behavior to some spells:
    <div class="kit_description">
	ACTION_FOR_EACH spell IN ~spell_1~ ~spell_2~ ~spell_3~ BEGIN<br />
	  LAF add_evade_spell <br />
		INT_VAR <br />
		  evasion_spellstate = 252 <br />
		  evasion_save = 2 <br />
		STR_VAR <br />
		  evade_spell = EVAL ~%spell%~ <br />
		  evade_prefix = ~[up to 4-character string]~ <br />
		RET <br />
		  evasion_ind <br />
	  END</p>
	</div>
  <p>There are some things there that need explaining.</p>
  <p>The "evasion_spellstate" variable refers to an entry in SPELLSTATE.IDS.  For traditional IWD-style Evasion, I use the Evasion spellstate, #252.  But you can use any other spellstate in your application, including custom spellstates.</p>
  <p>The "evasion_save" variable refers to the saving throw used for the extra save.  The traditional Evasion saving throw is vs. Breath Weapon (something like a Dodge save, I think).  But you can apply an alternate version that uses other saves... you can give characters an extra Death save to avoid Energy Drain spells, or something like that.</p>
  <p>The "evade_spell" variable refers to the spell being patched by the function.  To apply this to one spell, simply use that spell's name.  If applying it to a list of spells, as in the example above, use the variable to go through the list.</p>
  <p>The "evade_prefix" variable must be created by you.  it should be unique, and it MUST be 4 characters or fewer, and it should not conflict with any spell or item name when any 3 numbers are appended to it.  I use "d5ev" - "d5" is my modding prefix, and "ev" is for evasion.  This prefix has two uses: 1) it is combined with the "evasion_ind" integer variable to create a unique filename for the .EFF files uses with each spell to which this function is applied; and 2) it is combined with the spellstate number of the spellstate used, in the name of a .2DA file which is created and which lists all of the spells and the associated .EFF files to which this function is applied.  The function checks this .2DA file to prevent the same evasion check from being applied more than once.</p>
  <p>Note: DO NOT use the same prefix with versions that use different spellstates.  It could cause problems.</p>
  <p>You can define your own list of spells to patch this way; but this package has a pre-defined array of the base IWD set.  You can call it with:</p>
    <div class="kit_description">
  	ACTION_PHP_EACH evade_spells AS evadable_spell => ind BEGIN <br />
  	LAF add_evade_spell INT_VAR evasion_spellstate = 252 evasion_save = 2 STR_VAR evade_spell = EVAL ~%evadable_spell%~ evade_prefix = ~[prefix]~ END <br />
	END</p>
	</div>
	To use the expanded set of spells I've made, add this before you INCLUDE the .tpa file:</p>
    <div class="kit_description">
OUTER_SET optional_evade = 1
	</div>
</div>
<h2>Compatibility</h2>
<div class="section">
  <p>BG2 already has a High-Level Ability called "Evasion," so for the sake of clarity I have taken to calling this ability "Spell Evasion." But this function itself doesn't add any strings; all you see in-game is "Charname: save vs. breath weapon" and the spell will be evaded.</p>
  <p>Note: this will only work in the EE versions of the game, version 1.4 or higher.</p>
</div>
</body>
</html>
