---
id: scenes_reference
name: Scenes Reference
title: Scenes Reference
tags:
    - Reference
---

# Scenes

## Summary

A **Scene** is a way to have different assets and logic for a specific part of the game. A game can be made up of many scenes (e.g. main menu, tutorial, different levels) that display different information, and usually there is some way for a **Player** to get from one scene to the other.

All projects come with one scene by default called **Main**, which will be shown in the **Scenes** panel as the active scene.

## Creating a Scene

Creating a **Scene** can be done from the **Scenes** panel. Clicking on the **Scenes** button at the top of the **Hierarchy** will open a panel that will show a list of all scenes for the project, and a **Create New Scene** button at the bottom of that list.

![Scene Button](../img/scenes/scenes_button.png)
![Scene Panel](../img/scenes/scenes_panel.png)
*Scenes button (left), Scenes panel (right)*
{: .image-cluster}

1. Open the **Scenes** panel.
2. Click the **Create New Scene** button at the bottom of the **Scenes** panel.
3. Enter a name for the new scene.

When a new scene has been created, the creator will be asked if they want to load the new scene.

!!! info "Limits"
    - Support for up to 64 scenes per project.
    - Maximum total size of a game is 500mb.
    - Maximum size of a scene is 100mb.

## Loading a Scene

When loading a **Scene**, **Core** will unload all assets in the project **Hierarchy** (i.e. template instances, script instances, UI), and load all the assets for the scene that will be loaded. When loading a scene, make sure to save the current scene or progress will be lost. **Core** will notify the creator of any unsaved changes when attempting to switch scene.

A **Scene** can be loaded by clicking on the scene name in the **Scene** panel.

Another way to load a scene is from the **Scene** options panel.

1. Click the button to the right of the scene.
2. Click **Load Scene** from the options menu.

![Loading Scene](../img/scenes/option_load_scene.png){: .center loading="lazy" }

## Renaming a Scene

Any scene in the project can be renamed. Scenes can't share the same name as each other. If a scene is renamed to the same name as an existing scene in the project, then that new name will be appended an index. For example, if there is already a scene named "Tutorial", renaming another scene to "Tutorial" will be named "Tutorial_1".

1. Open the **Scenes** panel.
2. Click the button to the right of the scene.
3. Click **Rename Scene** from the options menu, and set the new name for the scene.

![Renaming a Scene](../img/scenes/option_rename_scene.png){: .center loading="lazy" }

## Setting a Main Scene

The main scene for a project, is the scene that will be loaded for players by default when entering your game. Any scene created can be set up as the main scene for the project. For example, if there is 2 scenes that have a different theme (i.e. Christmas vs Summer), it is easy to change which scene is the main one and republish your project. When players load your game, they will load the main scene first.

1. Open the **Scenes** panel.
2. Click the button to the right of the scene.
3. Click **Set as Main Scene** from the options menu.

After a scene has been set as the main scene, the name of that scene will also contain the text **Main** to indicate that scene is now the main scene for your project.

Scenes set to **Main Scene** will also be the first scene loaded when loading your project.

![Set Main Scene](../img/scenes/option_set_main_scene.png){: .center loading="lazy" }

## Duplicating a Scene

Duplicating a scene is a good way to use an existing scene as a base. All assets in the scene being duplicated will be copied over to the new scene. For example, creating a tutorial based on the first level of your game without needing to start a new scene from scratch.

When duplicating a scene, **Core** will name the duplicated scene the same name as the scene being duplicated, but append an index to the end so it is unique. For example, duplicating the scene "Opening Level" will be named "Opening Level_1".

1. Open the **Scenes** panel.
2. Click the button to the right of the scene.
3. Click **Duplicate Scene** from the options menu.

![Duplicate a Scene](../img/scenes/option_duplicate_scene.png){: .center loading="lazy" }

## Include for Publish

Scenes can be included or excluded when publishing a game. This can be done from the **Scenes** panel by clicking on the checkbox, or by selecting from the options menu.

If a scene is already set to **Include for Publish**, the option will change to **Exclude from Publish**.

1. Open the **Scenes** panel.
2. Click the button to the right of the scene.
3. Click **Include for Publish** from the options menu.

![Include for Publish](../img/scenes/option_include_for_publish.png){: .center loading="lazy" }

## Deleting a Scene

To delete a scene from your project, it can not be currently loaded, or set as the main scene. When deleting a scene, all objects in the **Hierarchy** will be deleted. Objects such as template instances (including script instances) will be deleted from the **Hierarchy**, but not from **Project Content**. This means, any changes to a template that needs to be kept,, should be updated before the scene is deleted.

!!! info "**Core** will confirm if the scene should be deleted."

1. Open the **Scenes** panel.
2. Click the button to the right of the scene.
3. Click **Delete Scene** from the options menu.

![Deleting a Scene](../img/scenes/option_delete_scene.png){: .center loading="lazy" }

## Show Scene in Explorer

Each scene created is a physical folder on your system. Only scenes that have been saved can be revealed in the explorer.

1. Open the **Scenes** panel.
2. Click the button to the right of the scene.
3. Click **Show Scene in Explorer** from the options menu.

![Show in Explorer](../img/scenes/option_show_in_explorer.png){: .center loading="lazy" }

## Publishing Scenes

Publishing a game will allow creators to select which scenes to publish. If no scenes have been created, then the main scene will be selected by default. If there are new scenes added to your project, those scenes will need to be ticked in the *Scenes** panel, or from the publishing window.

Click on the **Publish Game** button to bring up the **Game Publishing Settings**.

![Publish Button](../img/scenes/publish_button.png){: .center loading="lazy" }

The **Game Publishing Settings** window has a **Scenes** drop down where creators can select which scenes should be included when publishing.

!!! info "When publishing any scenes that isn't the main scene, **Core** will only publish the assets used for that scene."

![Publishing Scenes](../img/scenes/publish_scenes.png){: .center loading="lazy" }

## Transferring to a Scene

Transferring players between scenes can be done by calling `TransferToScene`. When calling this function, the scene name must be passed, and must be named exactly as the scene is named.

!!! warning "`TransferToScene` does not work in preview mode or games played locally."

Below is an example. When a player overlaps the trigger, they will be transferred to the **Tutorial** Scene.

```lua
local TRIGGER = script:GetCustomProperty("trigger"):WaitForObject()

-- Name of scene to transfer too
local sceneName = "Tutorial"

-- Transfer player when they overlap the trigger
function OnOverlap(trigger, other)
    if other:IsA("Player") then
        other:TransferToScene(sceneName)
    end
end

TRIGGER.beginOverlapEvent:Connect(OnOverlap)
```

`TransferToScene` supports a second argument that allows creators to specify the spawn point based on a matching key. If no match is found, then players will spawn at the origin, (0, 0, 0).

```lua
other:TransferToScene(sceneName, {spawnKey = "TutorialArea"})
```

## Storage

Any data stored in **Storage** is transferred to the scene being loaded. For example, if a player has inventory data, this means the scene can look at the **Storage** for the player to retrieve that data without needing to use a **Shared Storage Key**.

## Leaderboards

Any **Leaderboard** setup for your project will be available to all your scenes. For example, if there is a score that persists between scenes, then this can be submitted to a **Leaderboard** from any scene within your project.

## Parties

Parties work differently for scenes. Currently, creators can not transfer all party members to another scene. Creators can transfer each party member individually to a scene, but it is not guaranteed all party members will be in the same scene instance.

## Migrating from Child Games to Scenes

Creators that have been using our Child Games system previously, [here's how to convert](https://forums.coregames.com/t/migrating-from-child-games-to-scenes/1900) to the new **Scenes** system.

## Learn More

[TransferToScene](../api/player.md) | [TransferAllPlayersToScene](../api/game.md) | [GetCurrentSceneName](../api/game.md)
