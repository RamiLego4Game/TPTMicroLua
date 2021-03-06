# Official MicroLua User Manual

MicroLua is a luascript for ThePowderToy which adds Lua proccessors to the game that can be saved !

![SevSeg Demo](https://raw.githubusercontent.com/RamiLego4Game/TPTMicroLua/master/MicroLua%201.gif "SevenSegment display driven by a MicroLua Proccessor")

## How much this script is safe ?

The proccessors code is run in a well done Lua Sandbox, with 3 layers of protection:

1. **Custom Environment:** The code in the proccessor runs with a reduced version of standard Lua library, 
with all the miscellaneous functions like `loadstring`, `load`, `os.execute`, `os.remove`, `io.open`, ... removed.

2. **Runs Inside a Coroutine:** This way the script will have more control over the proccessors code, so whenever a proccessors wants to call an API function it yields the coroutine, so the game won't freeze, and the script will know what's the proccessor is calling.

3. **Instructions Qouta:** This way, the proccessor is limited to not execute over than 400000 instruction per tick, in other words a proccessors with `while true do end` code wont freeze the game.

## Installation Guide:
1. Install The Script Manager by openning the lua console either by pressing `~` or by clicking on the C button in the top-right corner, and type:
```lua
tpt.getscript(1,"autorun.lua",1)
```

2. Open the script manager by clicking the button on the right with "Lua" on it.
3. Click on 'Script Folder'.
4. Open https://pastebin.com/Jt5eE4aP in your browser and click on "download", save it into the Scripts folder you opened before.
5. In tpt check the MicroLua checkbox and press "Done".
6. Enjoy !

## Elements:

### ![LMPU](https://raw.githubusercontent.com/RamiLego4Game/TPTMicroLua/master/LMPU.png "LMPU") Lua Micro Proccessing Unit:

### ![LCBL](https://raw.githubusercontent.com/RamiLego4Game/TPTMicroLua/master/LCBL.png "LCBL") Lua Cable:

### ![LMRC](https://raw.githubusercontent.com/RamiLego4Game/TPTMicroLua/master/LMRC.png "LMRC") Lua Micro Read Chip:

### ![LROM](https://raw.githubusercontent.com/RamiLego4Game/TPTMicroLua/master/LROM.png "LROM") Lua Read Only Memory:

### ![LPIN](https://raw.githubusercontent.com/RamiLego4Game/TPTMicroLua/master/LPIN.png "LPIN") Lua PIN:

## Proccessors Code Global Environment:

**NOTE:** Calling any api functions (even the standard lua library) will cost a tick in powdertoy.

### MicroLua API:

* `tick()`: Waits for the next powdertoy tick, and resets the qoute counter.
* `sleep(n)`: Sleeps for `n` number of powdertoy ticks.
* `spark(pin)`: Sparks a pin with the given id (The pin's TMP property value, press `d` for debug view).
 - You can also pass a table with pins numbers, to spark them at the same tick.
* `bool = isSparked(pin)`: Returns if a pin is sparked or not.
 - `bool, pid = isSparked({pinslist}}`: Returns true if all the pins are sparked, false and a pin id if any pin is not sparked.
 - `results = isSparked({pinslist}, true)`: Returns a table with all the pin ids replace by booleans representing each one if sparked or not.
 - `pin = isSparked()`: Checks all the MPU pins, and returns the id of the first pin it finds sparked, or false if all the pins aren't sparked.
* `setColor({red,green,blue})`: Sets the LMPU pixel color, values go from 0 -> 255.
 - Can be called with no args to restore the default color.

### Standard Lua Libraries:

* `pairs`, `ipairs`, `next`, `pcall`, `xpcall`, `print`, `select` , `type`, `tostring`, `tonumber`, `unpack`, `error`, `assert`, `_VERSION`

* **Table**:
`table.insert`, `table.remove`, `table.maxn`, `table.sort`, `table.concat`

* **Math:**
`math.abs`, `math.fmod`, `math.floor`, `math.ceil`, `math.min`, `math.max`, `math.modf`, `math.sqrt`, `math.pow`, `math.exp`, `math.log`, `math.log10`, `math.log`, `math.frexp`, `math.ldexp`, `math.deg`, `math.rad`, `math.sin`, `math.cos`, `math.tan`, `math.asin`, `math.acos`, `math.atan`, `math.random`, `math.randomseed`, `math.pi`, `math.huge`
 
 * **String:**
`string.len`, `string.sub`, `string.rep`, `string.upper`, `string.lower`, `string.reverse`, `string.byte`, `string.char`, `string.format`, `string.find`, `string.gmatch`, `string.gsub`
 
 * **Coroutine:**
`coroutine.create`, `coroutine.resume`, `coroutine.running`, `coroutine.yield`, `coroutine.status`, `coroutine.wrap`
 
 * **OS:**
`os.time`, `os.difftime`, `os.date`, `os.clock`

 * **Bit:**
 `bit.tobit`, `bit.tohex`, `bit.bnot`, `bit.band`, `bit.bor`, `bit.bxor`, `bit.lshift`, `bit.rshift`, `bit.arshift`, `bit.rol`, `bit.rol`, `bit.ror`, `bit.bswap`

## Flashing a LMPU:
![Flashing Tutorial Gif](https://raw.githubusercontent.com/RamiLego4Game/TPTMicroLua/master/MPU%20Flashing.gif "A gif showing how to flash a proccessor")
1. Locate your LMRC chip location.
2. Press `D` to enable debug view and notice the part id of the LMRC (The number shown in `#xxxx`).
2. Open the Lua Console either by pressing `~` or by clicking on the C button in the top-right corner.
3. Type the following code and press enter (Replace index with your LMRC id, Replace filename with your Lua code file (In tpt folder))
```lua
micro.flushMRC(index,filename)
```
4. Unpause the game (Save and reload the game if you had it unpause so the LMPU reboots).
5. Enjoy !
