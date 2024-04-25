# \.sb3 file format unofficial docs

.sb3 is a .zip renamed file format. inside contains the following files:

* project.json
* sounds.wav
* costumes.svg

All files except project.json are require a id/file name that follows the following regex:  
``^[a-fA-F0-9]{32}$``  
Therefore must be a 32 diget hexidecimal number as a id.

## project.json

project.json must contain a json object with these children:

* "targets": list
* "monitors": list
* "extensions": list
* "meta": json
  
Lets take a look at each child:  
    "targets" contains information on sprites, blocks, comments, etc.  
    "monitors" contains information on variables/list displays on stage.  
    "extensions" contains information on the added extensions
    "meta" contains information on the project.  

### "targets"

"targets" is a list of 1 or more json objects.  
The first json object will be the stage.  
The stage contains some special children:

#### stage json struct

| Child                  | Type   | Discription                                       |
| ---------------------- | ------ | ------------------------------------------------- |
| "isStage"              | Bool   | if this sprite is the stage (must be True)        |
| "name"                 | String | name of this sprite (must be "Stage")             |
| "variables"            | Json   | contains "for all sprites" variables              |
| "lists"                | Json   | contains "for all sprites" lists                  |
| "broadcasts"           | Json   | dont really know TBH                              |
| "blocks"               | Json   | contains blocks in the stage                      |
| "comments"             | Json   | contains costumes in the stage                    |
| "currentCostume"       | Int    | index of current costume                          |
| "costumes"             | Json   | contains backgrounds                              |
| "sounds"               | Json   | contains sounds in the stage                      |
| "volume"               | Int    | contains volume of the project                    |
| "layerOrder"           | Int    | Layer level of the stage (must be 0)              |
| "tempo"                | Int    | tempo for music extension (still required?)       |
| "videoTransparency"    | Int    | transparency of video extension (still required?) |
| "videoState"           | String | video mode of video extension (still required?)   |
| "textToSpeechLanguage" | String | language for TTS extension (still required?)      |

#### sprite json struct

| Child            | Type   | Discription                                 |
| ---------------- | ------ | ------------------------------------------- |
| "isStage"        | Bool   | if this sprite is the stage (must be False) |
| "name"           | String | name of this sprite                         |
| "variables"      | Json   | contains "for this sprite" variables        |
| "lists"          | Json   | contains "for this sprite" lists            |
| "broadcasts"     | Json   | dont really know TBH                        |
| "blocks"         | Json   | contains blocks in this sprite              |
| "comments"       | Json   | contains costumes in this sprite            |
| "currentCostume" | Int    | index of current costume                    |
| "costumes"       | Json   | contains costumes for this sprite           |
| "sounds"         | Json   | contains sounds in this sprite              |
| "volume"         | Int    | contains volume of this sprite              |
| "layerOrder"     | Int    | Layer level of this sprite                  |
| "visable"        | Bool   | if this sprite is visable                   |
| "x"              | Float  | X cords of this sprite                      |
| "y"              | Float  | Y cords of this sprite                      |
| "size"           | Int?   | Size of this sprite                         |
| "direction"      | Float? | Direction of this sprite                    |
| "draggable"      | Bool   | if this sprite is draggable                 |
| "rotationStyle   | String | rotation style                              |

Of these items, 7 are not leaf nodes. we will cover those here:  

#### "variables"

follows the format:  
*ID : [name, value]*

* *ID* is string  
* *name* is string  
* *value* is any  

#### "lists"

follows the format:  
*ID : [name, [values]]*

* *ID* is string  
* *name* is string  
* *values* is a list of any values  

#### broadcasts

who uses these anyway?

#### blocks

each block has a id as a key and several children  

| Child      | Type                          | Discription                                                       |
| ---------- | ----------------------------- | ----------------------------------------------------------------- |
| "opcode"   | String                        | the code for the type of block                                    |
| "next"     | String                        | the id of the next block (or null)                                |
| "parents"  | String                        | id of the last block (or null)                                    |
| "inputs"   | Json  (optional if no inputs) | one key value pair for each input to the block                    |
| "fields"   | Json                          | dropdown inputs to the block              (optional if no fields) |
| "shadow"   | Bool                          | if the block is "shadowed" aka cant be moved by cursor (optional for not procedure-protypes) |
| "topLevel" | Bool                          | if it is a hat block       (optional for non-hats)                |
| "x"        | int/float?                    | only if topLevel is true              (optional)                  |
| "y"        | int/float?                    | only if topLevel is true                   (optional)             |
| "mutation" | Json                          | only if opcode is "procedures_protoype" or "procedure_call"       |

once again, of these, 3 are not leaf nodes:

##### inputs

| Child       | Type | Discription                                              |
| ----------- | ---- | -------------------------------------------------------- |
| "CONDITION" | List | only for if-else and if-then.                            |
| "SUBSTACK"  | List | for enclosing blocks like repeats, forever, and if-then. |
| "SUBSTACK2" | List | only for if-else.                                        |
| "VALUE"     | List | for set/change variable?                                 |
| "STRING"    | List | for any string input?                                    |
| "ITEM"      | List | for list functions                                       |
| "OPERAND"   | List | for bool blocks with 1 input (not only?)                 |
| "OPERAND1"  | List | for bool blocks with 2 inputs                            |
| "OPERAND2"  | List | for bool blocks with 2 inputs                            |
| "NUM1"      | List | for math operations                                      |
| "NUM2"      | List | for math operations                                      |
| "INDEX"     | List | location in a list. can be "random", "last" or "all" too |
| "LETTER"    | List | for letter_of block                                      |
| "DURATION"  | List | for wait_seconds block                                   |
| "SECS"      | List | for say/think for _                                      |
| "MESSAGE    | List | for say/think                                            |
| "EFFECT"    | List | for effects dropdown                                     |
| "CHANGE"    | List | for change ___ by                                        |
| "SIZE"      | List | for set size                                             |
| "COSTUME"   | List | costume number                                           |

for each of these they have the value:  
*InputType: [shadowtype, id/array_repersentation, (secondary id/list_repersentation if shadowtype 3)]*  

shadowtype: 1 if the input is a shadow, 2 if there is no shadow, and 3 if there is a shadow but it is obscured by the input.  

id/array_repersentation:  
Can either be a block id (like a operator) or a array repersenting a value  

| Type             | 1st element | 2nd element                                                  | 3rd element | 4th element                | 5th element                |
| ---------------- | ----------- | ------------------------------------------------------------ | ----------- | -------------------------- | -------------------------- |
| Number           | 4           | value                                                        |             |                            |                            |
| Positive number  | 5           | value                                                        |             |                            |                            |
| Positive integer | 6           | value                                                        |             |                            |                            |
| Integer          | 7           | value                                                        |             |                            |                            |
| Angle            | 8           | value                                                        |             |                            |                            |
| Color            | 9           | "#" followed by a hexadecimal numeral representing the color |             |                            |                            |
| String           | 10          | value                                                        |             |                            |                            |
| Broadcast        | 11          | name                                                         | id          |                            |                            |
| Variable         | 12          | name                                                         | id          | x-coordinate, if top-level | y-coordinate, if top-level |
| List             | 13          | name                                                         | id          | x-coordinate, if top-level | y-coordinate, if top-level |

##### fields

dropdowns WIP  

| Child                 | Type | Discription                                                                           | Format             |
| --------------------- | ---- | ------------------------------------------------------------------------------------- | ------------------ |
| "VALUE"               | List | for "argument_reporter_string_number"                                                 | [String, null]     |
| "LIST"                | List | for list blocks                                                                       | [listname, listid] |
| "VARIABLE"            | List | for variable blocks                                                                   | [varname, varid]   |
| "BROADCAST_OPTION"    | List | for broadcasts                                                                        | [dont know]        |
| "STOP_OPTION"         | List | for stop_ block. typeofstop can be "all", "this script", or "other scripts in sprite" | [typeofstop, null] |
| "CLONE_OPTION"        | List | for clone block                                                                       | [dont know yet]    |
| "TOUCHINGOBJECTMENU"  | List | for touching block. can be "\_mouse\_" or "\_edge\_"                                  | [dont know]        |
| "BACKDROP"            | List | for set backdrop                                                                      | dontknow           |
| "WHENGREATERTHANMENU" | List | for when _ > input hat                                                                | dontknow           |
| "BROADCAST_INPUT"     | List | for broadcasts                                                                        | idk                |
| "KEY_OPTION"          | List | for when key pressed and if key pressed                                               | idk                |

theres many more but im bored for now