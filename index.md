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

follows the format:  
*ID : {
    "opcode": opcode,  
    "next":IDofNext,  
    "parents": IDofParent,  
    "inputs": inputs,  
    "fields": fields,  
    "shadow" : shadow,  
    "topLevel" : topLevel,  
     }*  
if it is "topLevel",
ID will also contain:  
*"x":x,  
"y":y*

if "opcode" is "procedures_prototype"
ID will also contain:  
*"mutation":mutation*
  
* *ID* is string  
* *opcode* is string (pls look at their github for opcodes i dont want to make a full table for that)   
* *IDofNext* is the ID of the next block or null
* *IDofParent* is the ID of the last block or null
* *inputs* is Json
* *fields* is Json
* *shadow* is bool
* *topLevel* is bool
