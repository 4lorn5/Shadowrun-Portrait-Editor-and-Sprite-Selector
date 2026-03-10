
# Shadowrun Portrait Editor and Sprite Selector

The Shadowrun Portrait Editor and Sprite Selector is an editor for Shadowrun on the Sega Genesis. It allows users to export and import custom portraits for party members, adjust their smaller portraits, and define their overworld sprites.


## Features:

- Load ROM in common formats (.smd, .bin, .gen, and .md)
- Export and import party member portraits as .png files
- Portrait preview (old and new)
- Party member sprite selector
- Generates ready to play ROM file (requires original ROM and changes made)
- HTML (run a lightweight, portable file; or use directly online)


## Running the Editor:

* HMTL: Just save file as .html and double-click from your local folder; it will open in your default browser.
* Online: The editor is live at: https://4lorn5.github.io/Shadowrun-Portrait-Editor-and-Sprite-Selector/


## Usage:

**- LOAD ROM:** Choose your Shadowrun (.smd, .bin, .gen, and .md extensions accepted). Left column lets you select a party member. Center area will show your selected party member's portrait. Right column has portrait and sprite options.

**- EXPORT PNG:** You can export the portrait data as a .png. Original portrait palettes will be shown on top right of editor.
	NOTE: There are Shared Tiles (displayed in the editor) and transparencies across portraits, something I couldn't fix. This means that when selecting between Joshua's archetypes, tiles from his other archetypes will show. This is not a problem in game, however, as it only affects Joshua's archetype selection screen (only shown once in the game).
	
**- IMPORT PNG:** Import your new or edited .png portrait.

**- HUD THUMBNAIL:** Define the small HUD portrait area. The game takes coordinates from the larger portrait to show the smaller portrait in the vertical, right hand part of the HUD. You'll need to select an area of 4x4 tiles to use ingame. Once done, click on "Write Crop Byte". 

**- OVERWORLD SPRITE:** Define what you want the characters to look like in the overworld.

**- PATCH ROM:** prepares changed ROM data in memory (doesn't affect the original ROM).

**- SAVE ROM:** Generate a ready-to-play "Shadowrun (USA)_patched" ROM (requires the original ROM and changes applied).
	
**- NOTE:** for best results, import custom portraits that don't stray too much from the original size, orientation and palettes, and also take into account an area of 4x4 tiles (8x8 pixels each) for the small portrait.


## What the editor doesn't do:

- The editor does not support changing party member sprites to Gargoyles or Hellhounds, as these sprites/entities are buggy. You can still do it manually; enjoy changing your Mage or Shaman to a Hellhound, and watch them become forever frozen in the "pelt" sprite frame once they cast a spell.

- The editor also does not support changing palettes for portraits or sprites. This might come at a later time.


## Documentation:

- Every party member in the game has two character blocks that define appearance; one for when they are party members (this block also includes information on stats, equipment, appearance, etc.) and another that defines their appearance while they are still NPCs (ie., not yet party members, or dismissed and waiting at their usual hangout). For the purposes of this editor, only portrait and sprite data is documented.

Data for appearance of party members (in party): 
- For large portraits, each character has 80 tile addresses (8 columns × 10 rows), total of 64x80 pixels, scattered across the ROM in several addresses (pre-mapped in the editor).
- Small portraits use a single byte to control what 4x4 tile section of the portrait appears in the HUD. The high nibble defines the row; the low nibble defines the column (portrait column + 2).

| Character         | Block Base |   Large portrait  |  Small portrait  | Sprite (in party) |
|-------------------|------------|-------------------|------------------|-------------------|
| Joshua (Samurai)  | `0x072920` | `20 FA (0x7296C)` |  `04 (0x07296F)` |  `03 (0x72929)`   |
| Joshua (Decker)   | `0x072A20` | `22 3A (0x72A6C)` |  `04 (0x072A6F)` |  `03 (0x72A29)`   |
| Joshua (Shaman)   | `0x072B20` | `21 9A (0x72B6C)` |  `04 (0x072B6F)` |  `03 (0x72B29)`   |
| Ricky             | `0x072C20` | `22 DA (0x72C6C)` |  `14 (0x072C6F)` |  `0B (0x72C29)`   |
| Winston Marrs     | `0x072D20` | `23 7A (0x72D6C)` |  `04 (0x072D6F)` |  `04 (0x72D29)`   |
| Trent Delisario   | `0x072E20` | `24 1A (0x72E6C)` |  `14 (0x072E6F)` |  `09 (0x72E29)`   |
| Petr Uvehr        | `0x072F20` | `24 BA (0x72F6C)` |  `14 (0x072F6F)` |  `0B (0x72F29)`   |
| Walking Bear      | `0x073020` | `25 5A (0x7306C)` |  `14 (0x07306F)` |  `0A (0x73029)`   |
| Phantom           | `0x073120` | `25 FA (0x7316C)` |  `04 (0x07316F)` |  `09 (0x73129)`   |
| Ilene Two Fists   | `0x073220` | `26 9A (0x7326C)` |  `04 (0x07326F)` |  `0A (0x73229)`   |
| Freya Goldenhair  | `0x073320` | `27 3A (0x7336C)` |  `04 (0x07336F)` |  `06 (0x73329)`   |
| Rianna Heartbane  | `0x073420` | `27 DA (0x7346C)` |  `04 (0x07346F)` |  `06 (0x73429)`   |
| Stark             | `0x073520` | `28 7A (0x7356C)` |  `08 (0x07356F)` |  `03 (0x73529)`   |

- All of Joshua's archetypes share the same sprite, so they are all 0x03; Stark also shares the same sprite and value.
- Ricky and Petr share the same sprite, value 0x0B.
- Trent and Phantom share the same sprite, value 0x09.
- Walking Bear and Ilene Two Fists share the same sprite, value 0x0A.
- Freya and Rianna share the same sprite, value 0x06.
- Winston is the only "unique"; sprite is shared with Ork Gang Members, but not with any other party NPC.

Data for appearance of party members (before joining party/after dismissed):
- The second table that is responsible for party member appearance is not documented as such in the ROM. While the "main" character block is easy to find thanks to its ASCII strings for recognizable character names, the secondary only lists 10 entities named "lone11". Whether this was developer oversight or a way to obfuscate their definition is unclear; regardless, these define party members' appearance before joining (or dismissed after joining).
- The block structure is not fully documented yet, but the offsets and values are easy to read - they have the same sprite IDs as the main block:

| Character         | Sprite (not in party) | Palettes (likely*) |
|-------------------|-----------------------|--------------------|
| Ricky             |    `0B (0x72C29)`     |   `20 (0x72C29)`   |
| Winston Marrs     |    `04 (0x72D29)`     |   `40 (0x72C29)`   |
| Trent Delisario   |    `09 (0x72E29)`     |   `40 (0x72C29)`   |
| Petr Uvehr        |    `0B (0x72F29)`     |   `60 (0x72C29)`   |
| Walking Bear      |    `0A (0x73029)`     |   `60 (0x72C29)`   |
| Phantom           |    `09 (0x73129)`     |   `20 (0x72C29)`   |
| Ilene Two Fists   |    `0A (0x73229)`     |   `20 (0x72C29)`   |
| Freya Goldenhair  |    `06 (0x73329)`     |   `40 (0x72C29)`   |
| Rianna Heartbane  |    `06 (0x73429)`     |   `20 (0x72C29)`   |
| Stark             |    `03 (0x73529)`     |   `60 (0x72C29)`   |

(*) (likely because they are the same values found in the main character blocks)

- There's no reference for Joshua in the second table since he is never "found" as an NPC; being the main character, he is always in the party, so there's no data for him outside the party. Because of this, any change to his sprite on the main character blocks is immediate and doesn't require additional updates elsewhere.


## Special thanks

- Most of the research was done by me, so yay me.
- Tony Hedstrom for his Shadowrun codes and editors, which allowed for some speedy testing and confirm some values.



