# Roblox-Remote-BindableFunction-Handling-Service

```luau
print("Welcome to my project!")
```
> **Ever worried because your Roblox experience gets hacked due to faulty remotes?**
>
> Then use my project!

A service designed to help developers handle and secure their **RemoteEvents** and **RemoteFunctions** in Roblox experiences.
```luau
--Use This To Implement To Main Remote Code / script in serverscriptservice
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local Security = require(script.Parent.RemoteSecurity)

local Remote = ReplicatedStorage.Remotes.Example

Remote.OnServerEvent:Connect(function(player, ...)
	if not Security.Validate(...) then
		warn("Blocked suspicious request from " .. player.Name)
		return
	end

	print("Accepted request from " .. player.Name)
end)
```
```luau
--Security Module Script /NAME : RemoteSecurity And Put in sss ( server script service)
local Security = {}

local blockedPatterns = {
	"require%s*%(",
	"loadstring%s*%(",
	"getfenv%s*%(",
	"setfenv%s*%(",
}

function Security.Validate(...)
	for _, value in {...} do
		if typeof(value) == "string" then
			local text = value:lower()

			for _, pattern in blockedPatterns do
				if text:match(pattern) then
					return false
				end
			end
		end
	end

	return true
end

return Security
```

