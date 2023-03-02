## About obfuscation

- [About obfuscation (Lua/Roblox)](#about-obfuscation)
  * [VM (Virtual Machine)](#vm-virtual-machine)
  * [Loadstring](#loadstring)
- [Defeat obfuscation](#defeat-obfuscation)
  * [VM obfuscation](#vm-obfuscation)
    + [Examples from ironbrew](#examples-from-ironbrew)


### VM Virtual Machine
Lua VM obfuscation is a technique used to protect Lua code from reverse engineering or tampering by making it difficult to understand or modify by someone who doesn't have access to the original source code. The Lua Virtual Machine (VM) is responsible for interpreting and executing Lua code, and obfuscation techniques can be applied to the bytecode generated by the VM to make it harder to read and modify.

Some common techniques used for Lua VM obfuscation include:

1.  **Bytecode scrambling:** This technique involves rearranging the bytecode instructions in a random order, making it harder to understand the flow of the code.
    
2.  **Opcode substitution:** In this technique, the original opcodes are replaced with different ones that perform the same function, but are harder to recognize.
    
3.  C**onstant encryption:** This involves encrypting the string and number constants used in the code, making it difficult to extract sensitive information.
    
4.  **Control flow obfuscation:** This technique involves altering the control flow of the code by inserting dummy code or jump instructions, making it harder to follow the actual logic of the code.    

These techniques can be applied individually or in combination to make it harder to reverse engineer or modify the Lua code. However, it's important to note that obfuscation is not foolproof and can be circumvented by determined attackers with sufficient time and resources.

### Loadstring
Loadstring obfuscation is a technique used by skids to which try and hide the code using a simple loadstring but can be defeated by.
```lua
loadstring = print
```


## Defeat obfuscation

### VM obfuscation
There are multiple factors towards defeating VM obfuscation like finding the decompress function which turns the bytecode into `lua opcodes` which gets run by the vm aka an interpreter.

#### Examples from ironbrew 
`which people still don't know how to fully deobfuscate still 💀 but only constant dump`

```lua
-- Example decompress function
local function decompress(b)
	local c,d,e="","",{}
	local f=256;
	local g={}
	
	for h=0,f-1 do
		g[h]=Char(h)
	end;
	
	local i=1;
	
	local function k()
		local l=ToNumber(Sub(b, i,i),36)
		i=i+1;
		local m=ToNumber(Sub(b, i,i+l-1),36)
		i=i+l;
		return m
	end;
	
	c=Char(k())
	e[1]=c;
	
	while i<#b do
		local n=k()
		if g[n]then
			d=g[n]
		else
			d=c..Sub(c, 1,1)
		end;
		g[f]=c..Sub(d, 1,1)
		e[#e+1],c,f=d,d,f+1
	end;
	return table.concat(e)
end;

local ByteString=decompress(_StringExpr_);
```

```lua
-- Constants (The most skiddy way to deobfuscate)
for Idx=1, ConstCount do
	local Type = gBits8();
	local Cons;

	if(Type == _NumberExpr_) then

	elseif(Type == _NumberExpr_) then

	elseif(Type == _NumberExpr_) then

	end;
	Consts[Idx] = Cons;
end;

-- print(table.unpack(Consts)) DEOBFUSCATED AAKAKKAKAKAKk
```
