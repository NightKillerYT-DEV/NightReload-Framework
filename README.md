🌙 NightReload Framework

A hot-update system for Roblox that lets you add, replace, or remove game content in real time — without shutting down your servers.
Easily update active servers, push new scripts, or remove old assets, all while keeping the game running smoothly.

📦 What You Get

✅ A ready-to-use Model called NightBundle

🧩 A powerful update system that works with all major Roblox services

🔁 Real-time server updates with one simple console command

🛡️ Protected service containers (only contents are changed, never deleted)

🧼 _Remove folder system to clean up unwanted assets using path structure

🧠 Support for continuous development without breaking your game

🪄 How It Works

Download or clone this repository.

Insert the NightBundle model into your game.

Publish the NightBundle model to your Roblox account or group.

Open:

ReplicatedStorage.Packages._Index.night_nightreload@1.0.0.night.Core.Manifest


and replace the bundle ID with your own model’s asset ID:

bundle = 12345678901234


Add the provided NightReload scripts:

Server: place the server script in ServerScriptService

Client: place the local script in StarterPlayer/StarterPlayerScripts

You’re ready to roll out updates live.

🧰 Folder Structure Inside NightBundle
NightBundle
├─ _Remove/                      # Paths here = what gets deleted
├─ Workspace/
├─ ReplicatedStorage/
├─ StarterGui/
├─ StarterPlayer/
│  ├─ StarterPlayerScripts/
│  └─ StarterCharacterScripts/
├─ StarterPack/
├─ ServerStorage/
│  └─ ServerScriptService/       # Server scripts go here
├─ Lighting/
├─ SoundService/
├─ Teams/
├─ TextChatService/
└─ MaterialService/

🧹 The _Remove Folder (Deleting Stuff)

Whatever you mirror in _Remove will be deleted during rollout.

The path must match exactly where the item exists in the game.

Example 1 — Remove a script inside nested folders:

_Remove
└─ Workspace
   └─ FolderA
      └─ FolderB
         └─ ScriptToRemove


👉 Deletes only Workspace/FolderA/FolderB/ScriptToRemove.

Example 2 — Remove a full folder:

_Remove
└─ Workspace
   └─ OldEvent      # empty folder


👉 Deletes Workspace/OldEvent entirely.

🛡️ Protected containers (will never be deleted, only their contents):
StarterPlayer, StarterPlayer/StarterPlayerScripts, StarterPlayer/StarterCharacterScripts, ServerStorage, ServerStorage/ServerScriptService, Workspace, ReplicatedStorage, StarterGui, StarterPack, Teams, Lighting, SoundService, TextChatService, MaterialService.

🧭 Adding / Replacing Content

All other service folders (siblings of _Remove) are used to add or replace existing content.

For example:

Workspace/MyNewModel → adds or replaces that model in Workspace

ServerStorage/ServerScriptService/MyScript → replaces or adds a script to SSS

If something with the same name already exists, it will be replaced automatically.

🧪 Checking the Current Version

In the Server Console:

print(game.ReplicatedStorage.Packages._Index["night_nightreload@1.0.0"].night.Core.Flags.CurrentVersion.Value)

🚀 Rolling Out Updates

In the Server Console:

local NR = require(game.ReplicatedStorage.Packages.NightReload)
NR.Rollout({
	bundle  = 12345678901234,  -- your bundle ID
	version = "0.0.2",         -- bump this every time
	mode    = "rollout",
})


📝 Important:

You must bump the version each time (e.g. 0.0.3, 0.0.4...).

If you don’t bump it, the rollout won’t run.

There’s a short cooldown (10 seconds by default) between rollouts.

🛠 Optional Rollout Trigger Script

If you prefer a button or event to trigger rollouts automatically, you can use the provided rollout script in ServerScriptService:

local Rep = game:GetService("ReplicatedStorage")
local NR  = require(Rep.Packages.NightReload)

local function bump(v)
	v = tostring(v or "0.0.0")
	local a,b,c = v:match("^(%d+)%.(%d+)%.(%d+)$")
	a,b,c = tonumber(a) or 0, tonumber(b) or 0, tonumber(c) or 0
	return string.format("%d.%d.%d", a, b, c + 1)
end

local Flags = Rep.Packages._Index["night_nightreload@1.0.0"].night.Core.Flags
local Bundle = require(Rep.Packages._Index["night_nightreload@1.0.0"].night.Core.Manifest).bundle

local EV = Instance.new("BindableEvent")
EV.Name = "RolloutTrigger"
EV.Parent = Rep

EV.Event:Connect(function()
	local nextv = bump(Flags.CurrentVersion.Value)
	NR.Rollout({ bundle = Bundle, version = nextv, mode = "rollout" })
	print("[NightReload] Rolled out", nextv)
end)

print("[NightReload] Ready → trigger with: game.ReplicatedStorage.RolloutTrigger:Fire()")


Then in the console:

game.ReplicatedStorage.RolloutTrigger:Fire()


✅ This will auto-bump the version and apply the update to all active servers.

🧠 Tips

Make sure your Manifest.bundle ID is always set to the latest version of your model.

New servers get the latest bundle automatically.

Old servers get the update when you rollout.

_Remove is optional. If you don’t need to delete anything, leave it empty.

ProfileService sessions or player data won’t reset — your game keeps running smoothly.

🪪 License
NightReload Framework
Copyright (c) 2025 NightKillerYT-DEV

This framework is licensed under the GPLv3.
You are free to use, modify, and distribute this project with proper attribution.
You may not claim ownership or sell this as your own product.


📜 Full license text is included in the LICENSE file of this repository.

👑 Credits

Created & maintained by NightKillerYT-DEV
If you use this framework in your project, please give proper credit and link back to the repository.

⭐ Consider starring the repo if it helped you!
