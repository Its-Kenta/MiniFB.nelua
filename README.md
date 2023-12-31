<div align="center">
<p>

# MiniFB.nelua - [MiniFB](https://github.com/emoon/minifb) binding for the [Nelua Programming Language](https://nelua.io/)

</p>

[Installation](./INSTALL.md) • [Development](./DEVELOPMENT.md) • [Contributing](./CONTRIBUTING.md) • [License](./LICENSE)

</div>

**MiniFB.nelua** is a [MiniFB](https://github.com/emoon/minifb) binding for the latest MiniFB version.  small cross platform library to create a frame buffer that you can draw pixels in.

### Code Example

<details>
<summary><b>MiniFB - Basic Window with Rectangle</b></summary>

```lua
require 'minifb'
require 'memory'

local BUFF_SIZE: uint32 <comptime> = 2 * 512

local buffer: [BUFF_SIZE * BUFF_SIZE]uint32

local function drawSquare(buffer: *[0]uint32, dimen: uint32): void
    memory.set(buffer, 127, dimen * dimen * 4)

    local oneHalfDimen: uint32 <const> = dimen // 2
    local oneQuarterDimen: uint32 <const> = oneHalfDimen // 2
    local threeQuarterDimen: uint32 <const> = oneHalfDimen + oneQuarterDimen

    for x = oneQuarterDimen, threeQuarterDimen - 1 do
        for y = oneQuarterDimen, threeQuarterDimen - 1 do
            buffer[y * dimen + x] = (x & 1) ~= 0 and mfb.ARGB(0xff, 223, 0, (255 * (x - oneQuarterDimen)) // oneHalfDimen) or mfb.ARGB(0xff, 0, 0, 0)
        end
    end
end

drawSquare(&buffer, BUFF_SIZE)

local window: *mfb.window = mfb.open("HighRes", BUFF_SIZE / 2, BUFF_SIZE / 2)
local state: mfb.updateState

repeat
    state = mfb.updateEx(window, &buffer, BUFF_SIZE, BUFF_SIZE)
until not mfb.waitSync(window)

```

</details>

<details>
<summary><b>MiniFB - HiDPI</b></summary>

```lua
-- While this code works, it will report Segmentation Fault on closing windows.
-- this is caused by destroy_GL_context() function which destroys the windows and releases memory. This is also possibly caused by Nelua's GC albeit it was tested without and still returned Sigfault.

require 'minifb'
require 'memory'

local DIMEN_LOW: uint32 <comptime> = 512
local DIMEN_HIGH: uint32 <comptime> = 2 * DIMEN_LOW

local lowBuffer: [DIMEN_LOW * DIMEN_LOW]uint32
local highBuffer: [DIMEN_HIGH * DIMEN_HIGH]uint32

local function drawSquare(buffer: *[0]uint32, dimen: uint32): void
    memory.set(buffer, 127, dimen * dimen * 4)

    local oneHalfDimen: uint32 <const> = dimen // 2
    local oneQuarterDimen: uint32 <const> = oneHalfDimen // 2
    local threeQuarterDimen: uint32 <const> = oneHalfDimen + oneQuarterDimen

    for x = oneQuarterDimen, threeQuarterDimen - 1 do
        for y = oneQuarterDimen, threeQuarterDimen - 1 do
            buffer[y * dimen + x] = (x & 1) ~= 0 and mfb.ARGB(0xff, 223, 0, (255 * (x - oneQuarterDimen)) // oneHalfDimen) or mfb.ARGB(0xff, 0, 0, 0)
        end
    end
end

drawSquare(&lowBuffer, DIMEN_LOW)
drawSquare(&highBuffer, DIMEN_HIGH)
local windowLow: *mfb.window = mfb.open("LowRes", DIMEN_LOW, DIMEN_LOW)

local windowHigh: *mfb.window = mfb.open("HighRes", DIMEN_HIGH / 2, DIMEN_HIGH / 2)

while windowHigh or windowLow do
    if windowLow then
        if mfb.updateEx(windowLow, &lowBuffer, DIMEN_LOW, DIMEN_LOW) ~= mfb.updateState.OK then
            windowLow = nilptr
            break
        end
    end
    
    if windowHigh then
        if mfb.updateEx(windowHigh, &highBuffer, DIMEN_HIGH, DIMEN_HIGH) ~= mfb.updateState.OK then
            windowHigh = nilptr
            
        end
    end
    
    if windowHigh then
        mfb.waitSync(windowHigh)
    elseif windowLow then
        mfb.waitSync(windowLow)
    end
end


```

</details>
