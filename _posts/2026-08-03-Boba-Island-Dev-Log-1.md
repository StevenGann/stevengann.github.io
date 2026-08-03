---
title: "Boba Island Dev Log 1: Tabula Rasa"
description: Starting a cozy island boba shop sim in Godot, and why the first weeks produced no game at all
categories: [Projects, Game Development]
tags: [boba-island, devlog, godot, gdscript, game-development, cozy-games, game-design, steam-deck]
mermaid: false
---


## Introduction

My first game, [_27 Survivors_]({% post_url 2026-08-02-27-Survivors-Postmortem %}), was developed fast and quiet. There wasn't a whole lot of technical innovation to talk about, being a more artistically driven game where I was expressing my thoughts and feelings following being laid off from Meta. In four months of working 6 days a week, I turned my anxiety into an experience I could share with the world. Now anyone could understand the feeling of shifting reality and grim helplessness as a distant ultra-wealthy authority launches you and many others to your doom in order to make themselves a little bit richer. I'll be honest, [_27 Survivors_](https://store.steampowered.com/app/4773130/27_Survivors/) is less about making a game and more about processing that chapter of my life, and as a therapeutic tool it worked well!

And now that it is behind me I am pressing forward with a game that's all about sunshine, cozy vibes, and hope.

## The Elevator Pitch

> In _Boba Island_ (working title), you arrive on a sleepy little island with nothing but a push cart full of bubble tea supplies and a dream. Sell drinks, open a real shop, and help revitalize a community that is a shadow of its former self by inspiring the local people to turn against faceless corporations and support local small businesses again! A little [_Animal Crossing_](https://en.wikipedia.org/wiki/Animal_Crossing), a little [_Stardew Valley_](https://en.wikipedia.org/wiki/Stardew_Valley), a little [_Diner Dash_](https://en.wikipedia.org/wiki/Diner_Dash), all soaked in the optimistic and comforting nostalgia of 00s games.

It is more than a little ambitious, but I've given myself 6 months to build it. I learned a lot from _27 Survivors_, especially about the [Godot engine](https://godotengine.org/) but also about just how much is involved going from a basic demo to a real game. Of course, with _27_ I was able to get away with art and atmosphere first, gameplay second, because that's the nature of the horror genre. This time around the gameplay is going to be a lot more involved than walking around interacting with things. Even with _27_ I learned the hard way that "that'll be easy to implement, I'll worry about it later" was always the seed of regret.

## Gameplay and Code First, Game Later

When I was a kid I had this dream of being "a freelance game developer living out of a van." My parents gently encouraged me to go to college instead, which was probably wise. Regardless, I consumed everything I could find from the makers of my favorite games. My two greatest game dev heroes were [Will Wright](https://en.wikipedia.org/wiki/Will_Wright_%28game_designer%29) and [Sid Meier](https://en.wikipedia.org/wiki/Sid_Meier). Wright is best known for the Sim family of games, like [_SimCity_](https://en.wikipedia.org/wiki/SimCity), [_SimEarth_](https://en.wikipedia.org/wiki/SimEarth), and probably most famous [_The Sims_](https://en.wikipedia.org/wiki/The_Sims), though my personal favorites were [_SimAnt_](https://en.wikipedia.org/wiki/SimAnt), [_SimCopter_](https://en.wikipedia.org/wiki/SimCopter), and [_SPORE_](https://en.wikipedia.org/wiki/Spore_%282008_video_game%29). In interviews with Wright, his approach to developing these games often came up and the answer was always the same: They'd develop a simulation of something (a city, an ant colony, an ecosystem, etc.) and make the simulation interesting and complex enough to have emergent behavior, and then they'd build a game around that. Some of these, like _SimEarth_ and [_SimLife_](https://en.wikipedia.org/wiki/SimLife), were barely "games" even when published.

Sid Meier is probably a better-known name, having a habit of prefacing game titles with his own name. For me, I knew him as the genius that drove the [_Civilization_](https://en.wikipedia.org/wiki/Civilization_%28series%29) series of games. I fell in love with [_Civilization II_](https://en.wikipedia.org/wiki/Civilization_II), which was easily modified through BMP images and TXT files. I discovered this before I was really old enough to use the Internet regularly, experimenting my way into modifying the game in all sorts of silly ways. My proudest creation was a fairly complex mod that was based on the TV show [_Avatar: The Last Airbender_](https://en.wikipedia.org/wiki/Avatar:_The_Last_Airbender). When I discovered the internet, the spark kindled into an obsession over game modding and then game development, so it is no surprise that I still look to the _Civilization_ series as role models of good game design and implementation. In the bonus DVD content for [_Civilization III_](https://en.wikipedia.org/wiki/Civilization_III), Sid Meier explained his philosophy for making a game was to focus on the gameplay design and software implementation first and to add graphics and sounds and such only once the game itself was already fun without them.

So armed with the wisdom of two legends, I have decided that my development path for _Boba Island_ will be defining the gameplay, building abstract prototypes of each element, and then assembling them together before I put too much work into the art and map building.

## The Prototypes So Far

I started experimenting with ideas in Godot as soon as I'd recovered from [OpenSauce 2026](https://opensauce.com/), and in the time since I've put together a handful of sketches that have turned into proving grounds for ideas and harnesses for validating their implementation.

### Portals

In _27 Survivors_ I sneakily put a few portals in a few locations to make the map layouts more disorienting through impossible geometry. I used a [very good addon](https://godotengine.org/asset-library/asset/4022) for it at the time. For _Boba Island_, I decided I don't want to ever have transitions between rooms but I also wanted the freedom of interiors that are shaped differently than their quaint little exteriors. How hard could it be to make something bigger on the inside? Inspired by [Sebastian Lague](https://www.youtube.com/watch?v=cWpFZbjtSQg) I figured it'd be simple enough to roll my own portal system.

It was not.

![Looking through a window that is not a window: the room behind it is a separate space, rendered live.](/assets/img/2026-08-03-Boba-Island-Dev-Log-1/01-portals.png)
_Looking through a window that is not a window: the room behind it is a separate space, rendered live._

The portals ended up being the most difficult thing to get right so far. The visual elements weren't that difficult, Lague's video did most of the hard thinking for that and recreating it in Godot wasn't hard, especially with existing Godot implementations to use for reference. The fly in the ointment was the player controller. As soon as I started building it (more on that below) portals broke horribly because it wasn't as simple as teleporting the camera to the other portal as soon as it passed through the portal plane. The player controller includes a dynamic third-person view and motion smoothing for a cozy feel. First I wrestled with the motion smoothing making the camera fly for a few frames from one portal to the other, and then I dealt with the case of when the player is on one side of the portal and the camera is on the other. A lot of special cases, a lot of edge cases. I like [gdUnit4](https://github.com/godot-gdunit-labs/gdUnit4) and my workflow for chasing these bugs is to build test cases that reproduce them reliably and keep iterating until they're all green. These test cases stick around to serve as regression tests in the future so I can see if I break something again later or accidentally undo the fix when trying to be clever and delete some contrived code I forgot the purpose of.

### The Player Controller

My vision for the game calls for some fairly complex camera movement, mostly third-person but I also want the player to be able to zoom in to first-person and walk around if that's more comfortable for them. Otherwise, it is a fairly standard player controller: a pill-shaped collider sliding around. The most interesting thing is that the head smoothly fades out when you switch to first-person, and fades back in when you zoom back out, but the body remains. I might hide the body too, or just the torso, but I like the idea of looking down at your feet.

![The player, in full greybox. Two spheres and a shadow — everything here is placeholder except how it moves.](/assets/img/2026-08-03-Boba-Island-Dev-Log-1/02-player-controller.png)
_The player, in full greybox. Two spheres and a shadow — everything here is placeholder except how it moves._

### Villager Faces

My mood board is full of screenshots of [Miis](https://en.wikipedia.org/wiki/Mii) from [_Wii Sports Resort_](https://en.wikipedia.org/wiki/Wii_Sports_Resort) and [_Wii Fit_](https://en.wikipedia.org/wiki/Wii_Fit), so I needed to figure out how hard it'd be to recreate the animated 2D faces on 3D heads style. My approach is transparent textures on a curved plane just slightly in front of the head mesh. I made a very basic blinking animation just to test the rendering and it worked beautifully with minimum fuss.

![A face is a 2D card, not a rig. This one blinks on its own.](/assets/img/2026-08-03-Boba-Island-Dev-Log-1/03-villager-face.png)
_A face is a 2D card, not a rig. This one blinks on its own._

### Villager Navigation

I've played [_Skyrim_](https://en.wikipedia.org/wiki/The_Elder_Scrolls_V:_Skyrim) more than nearly any game, certainly more than any game on Steam. One of my favorite things about _Skyrim_ is the many NPCs going about their lives, walking from one location to another by whatever logic fits that specific NPC. I didn't want my NPCs bound to specific locations so I had to work out giving them the ability to navigate around the world to wherever they want to be.

![Marisol is walking to her shift at the supermarket; Pip is browsing the plaza. Neither route is written in code.](/assets/img/2026-08-03-Boba-Island-Dev-Log-1/04-villager-schedules.png)
_Marisol is walking to her shift at the supermarket; Pip is browsing the plaza. Neither route is written in code._

![Thirty villagers at once, each picking a destination, routing around the obstacles and steering around each other. The blue lines are intended routes; the green rings are where they're trying to end up.](/assets/img/2026-08-03-Boba-Island-Dev-Log-1/09-crowd.png)
_Thirty villagers at once, each picking a destination, routing around the obstacles and steering around each other. The blue lines are intended routes; the green rings are where they're trying to end up. Their name labels are switched off in this shot — at thirty agents they cover the entire arena._

### The UI Kit

I've got a close friend, Damen, who is more web-focused in his projects. He's always been my worst critic and one of his most common complaints is that my taste in UI design is horrible. With _27 Survivors_ I leaned into the retro amber CRT aesthetic to avoid trying to make GUIs that look good, just functional. For this game I want UIs that feel satisfying and easy on the eyes, with sounds and animations that feel "thocky" like a good mechanical keyboard or a fidget toy.

![The control set, shot at the Steam Deck's real panel size so "is that legible at 7 inches" has an answer.](/assets/img/2026-08-03-Boba-Island-Dev-Log-1/05-ui-kit.png)
_The control set, shot at the Steam Deck's real panel size so "is that legible at 7 inches" has an answer._

I built up this prototype to work out what the UI elements will look like and fine-tune their behavior so I can be sure they're ready to drop into the actual game later on. I also wanted to test on [Steam Deck](https://www.steamdeck.com/) and verify controller logic to get ahead of some UI bugs I dealt with in _27 Survivors_.

### Terrain

Terrain is the newest thing I've been hammering at and it has a long way to go. I am very happy with what I have so far and it is worthy of a dedicated dev log. Inspired by the technology of [_Townscaper_](https://en.wikipedia.org/wiki/Townscaper), the terrain is defined as hexagonal chunks subdivided into quadrilaterals, and all the vertices are shifted pseudorandomly along the horizontal plane and then a relaxation algorithm nudges them around to make the quads more similar in scale. Hexagons control elevation with the whole hex having a set elevation and then its six vertices being offset to either snap to a neighbor's vertex or to be a cliff face. At the quad scale, each quad can be assigned a material (currently just sand or grass) and a fragment shader adds some [Perlin noise](https://en.wikipedia.org/wiki/Perlin_noise) to make the material borders more organic than sharp polygons.

![An irregular hex grid, relaxed until it stops reading as a grid. The beach generates where the land meets the water.](/assets/img/2026-08-03-Boba-Island-Dev-Log-1/06-terrain.png)
_An irregular hex grid, relaxed until it stops reading as a grid. The beach generates where the land meets the water._

![The map editor. The block of text up top is the walkability instrument reporting on the island below it.](/assets/img/2026-08-03-Boba-Island-Dev-Log-1/07-map-editor.png)
_The map editor. The block of text up top is the walkability instrument reporting on the island below it._

Definitely will be the subject of my next dev log, once I have a few key features implemented.

### The Sandbox

Once I feel like a prototype is "done", I stitch it into the sandbox so I can play around with it and see how different features interact with each other. It is how I discovered the problems between the player controller and the portals, for example.

![Everything at once: the player rig, live portals, villagers on real schedules, one scene.](/assets/img/2026-08-03-Boba-Island-Dev-Log-1/08-sandbox.png)
_Everything at once: the player rig, live portals, villagers on real schedules, one scene._

## Where It Stands

A bunch of the more complicated elements are prototyped. Once I am happy with terrain I'll face down the actual drink-making mechanic that the game is _actually about_. In the meantime I've been reading up on [boba tea](https://en.wikipedia.org/wiki/Bubble_tea), as well as tea and coffee in general, and visiting boba shops around me to observe and get a better understanding of the business.