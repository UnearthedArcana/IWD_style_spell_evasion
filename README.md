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
  <p><strong> Version 1.0 </strong><br />
    <strong> Languages:</strong> English<br />
    <strong>Platforms: </strong>WeiDU</p>
</div>
<h2>WHAT THIS IS</h2>
<div class="section">
  <p>This is a drop-in utility that lets anyone add the IWD "Evasion" feat to their BGEE or BG2EE game.  A character with this feat, when targeted by certain magic spells, can make a saving throw vs. Breath Weapon to avoid the effect altogether.  The spell only works on them if the character fails that save... and then the spell itself may offer its own normal saving throw to avoid or reduce the effect (e.g. Fireball).</p>
</div>
<h2>TO USE THIS</h2>
<div class="section">
  <p>If you don't already have one, make a folder called "lib" in your mod's main folder.  Drop the "d5_spell_evasion.tpa" file there.  Now you need to do two things:<br />
  <p>1) Activate the Evasion function with the following code:</p>
  <p>INCLUDE ~%MOD_FOLDER%/lib/d5_spell_evasion</b></p>
  </p>2) Give the Evasion ability to somebody by applying "d5evade.spl" to them.  This can be done to all thieves, or to certain kits, or to certain enemies, whatever.  Applying that spell with set the Evasion spellstate on the individual in question, and anyone with that spellstate will get the benefits of the Evasion skill.</p>
</div>
<h2>Compatibility</h2>
<div class="section">
  <p>BG2 already has a High-Level Ability called "Evasion," so for the sake of clarity I have taken to calling this ability "Spell Evasion." But this file itself doesn't add any strings; all you see in-game is "Charname: save vs. breath weapon" and the spell will be evaded.</p>
  <p>Why I'm publishing this as a drop-in file, rather than just telling people to do it on their own, is this: if everyone uses this version, it can be done by six different mods in six different ways, and it will detect itself and get out of its own way so that they all work in concert.</p>
  <p>Note: this will only work in the EE versions of the game, version 1.4 or higher.</p>
</div>
</body>
</html>
