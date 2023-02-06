# Lua

## Comments

```lua
-- single line commend

--[[
	multi
	line
	comment
]]
```

## Print

```lua
print("Hello, world!")

-- printf
io.write("Enter a number: ")
```

## Concatenate strings

```lua
print("Hello, " .. "world!")
```

## Variables

```lua
local name
local age = 13

-- global variable
_G.GlobalVar = 42

local word="peanut"

local poem = [[roses are red
violets are blue]]

local x, y, z = 1, 2, 3

print(type("Hello"))
```

## Strings

```lua
-- return the length
print(#x)
local length = #x

-- convert to string
local str = tostring(5)

-- convert to lowercase
print(string.lower(str))
-- convert to uppercase
print(string.upper(str))
-- length, same as #str
print(string.len(str))
```

## Math

```lua
print(math.pi)
print(math.min(2, 3))
print(math.max(2, 3))

-- round up
print(math.ceil(2.9))
-- round down
print(math.floor(2.9))

-- random number
print(math.random())
-- random with seed
print(math.randomseed(os.time()))
-- random with max
print(math.random(10))
-- random with range
print(math.random(10, 50))
```

## If statement

```lua
local i = 0
local j = 1

-- if equal
if i == 0 then
	print("zero!")
end

-- if not equal
if i ~= 0 then
	print("not zero!")
end

if i == 0 and j == 0 then
	print("both zero!")
end

if i == 0 or j == 0 then
	print("one of them is zero!")
end
```

## Loops

```lua
-- from 1 to 10
-- for(i = 1; i <= 10; i++)
for i = 1, 10 do
	print(i)
end

-- from 10 to 1 substracting 
-- for(i = 10; i >= 1; i--)
for i = 10, 1, -2 do
	print(i)
end

while true do
	-- ...

	if something do
		break
	end
end

-- do while
repeat
	-- ...
until count < 10
```

## User input

```lua
local ans = io.read()
```

## Tables

```lua
local tbl = {"This", 2, true, 9.9, {"a", "b"}}

for i = 1, #tbl do
	print(tbl[i])
end

-- to access a table within a table
print(tbl[5][1])
-- this will give "a"

-- append
table.insert(tbl, 3)
-- insert "hello" at position 2
table.insert(tbl, 2, "hello")

-- remove the first item
table.remove(tbl, 1)

-- loop through indexes and values
for k, v in pairs(tbl) do
	print(k, v)
end

-- concatenate items in table with spaces
print(table.concat(tbl, " "))

-- dictionary
local person = {
	name = "Mike",
	age = 12
}

print(person["name"])
```

## Functions

```lua
local function sayHello()
	print("Hello!")
end

sayHello()

-- with default value
local function salute(name)
	local name = name or "person"

	print("hello" .. name)
end

sayHello()
sayHello("Jack")

-- with return value
local function sum(x, y)
	return x + y
end

```

## Files

```lua
-- create file or empty if it exists
-- in bash: > myfile.txt
io.output("myfile.txt")

io.write("some text")
io.close()

-- read data
io.input("myfile.txt")

local fileData = io.read("*all")

print(fileData)

io.close()

-- most commonly used
local file = io.open("myfile.txt", "w")
if file ~= nil then
	file:write("hello")
	file:close()
else
	print("could not open the file")
end

local file = io.open("myfile.txt", "r")
if file ~= nil then
	-- read all
	print(file:read("*all"))
	-- read a line
	print(file:read("*line"))

	file:close()
else
	print("could not open the file")
end

local file = io.open("myfile.txt", "r")
if file ~= nil then
	-- read all
	print(file:read("*all"))
	-- read a line
	print(file:read("*line"))

	file:close()
else
	print("could not open the file")
end

-- append
local file = io.open("myfile.txt", "a")
if file ~= nil then
	file:write("Appended text\n")
	file:close()
else
	print("could not open the file")
end
```

## Modules

```lua
Mod = {
	sum = function (x, y)
	return x + y
}

return Mod
```

or

```lua
Mod = {}

function Mod.sum(x, y)
	return x + y
end

return Mod
```

and then on the main file

```lua
local mod = require("custom")
-- where custom is the name of the file (custom.lua)
```

## OOP

```lua
local function Pet(name)
	local age = 10
	return {
		-- name or the default being Charlie
		name = name or "Charlie",

		daysAlive = age * 365,

		--[[ this is valid, however it's better to always include
		self so that we can call functions with ":" ]]
		speak = function(sentence)
			print(sentence)
		end,

		-- to call this, we need to use dog:feed
		feed = function(self)
			print("eating", self.name)
		end
	}
end

local cat = Pet()

print(cat.name)

-----------------
-- Inheritance --
-----------------

local function Dog(name)
	local dog = Pet(name)

	dog.breed = "doberman"
	dog.loyalty = 0

	return dog
end
```

