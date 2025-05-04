# Dungeon crawler screenshots

Here are some screenshots of an experimental dungeon crawler game engine I worked on a while back as a hobby project. It was written from scratch in C++ and I was targeting OpenGL ES 3.0-level hardware. Dungeons are generated procedurally using geometry from tilesets built in 3D modeling software like Blender or Maya. Once tiles are placed, the geometry is further refined and lighting is then baked into a static lightmap. Other features include skeletal animation supporting skinned meshes, character physics, navmesh-based A\* pathfinding, integrated LUA scripting, and a GUI editor for game asset management.

### Rendering

Here's a simple scene with a player and a few items on the ground.

![A simple scene](/Rendering/scene.png)

Some work on an overlay inventory system.

![Overlay inventory](/Rendering/inventory1.png)
![Overlay inventory bounding boxes](/Rendering/inventory2.png)

For efficiency, I used a simple "portal"-based culling system in which the scene was divided rooms and a room would only render if it was visible a series of doorways (which would be successively clipped). In this screenshot, the next room is visible but the room after is not because its doorway cannot be seen through the doorway to the right of the player.

![Portal-based culling](/Rendering/portals1.png)

This technique was nice because it was low complexity and very effective for the type of environment I was working with, even in large scenes.

![Portal-based culling](/Rendering/portals2.png)
![Portal-based culling](/Rendering/portals3.png)

And here, we have some arrows shot into the wall! Very exciting.

![Arrows](/Rendering/arrows.png)

### Procedural generation

To generate levels, I would first create a tileset.

![Tilsets](/ProceduralGeneration/displayed_tiles.png)

In addition to the rendered tiles, collision tiles are also required...

![Tilsets](/ProceduralGeneration/collision_tiles.png)

... as well as navmesh tiles.

![Tilsets](/ProceduralGeneration/navmesh_tiles.png)

Here is the geometry resulting from procedurally generating a level.

![Generated level](/ProceduralGeneration/generated_geometry1.png)
![Generated level](/ProceduralGeneration/generated_geometry2.png)

### Lightmapping

Since I was targeting OpenGL ES 3.0-level hardware, baked lighting seemed like a good approach to balance a reasonably good look with good performance. (The lizard just has a simple drop shadow.)

![Baked lighting](/Lightmapping/room1.png)
![Baked lighting](/Lightmapping/room2.png)

I used a packing algorithm to pack many generated lightmaps into a single texture.

![Lightmap](/Lightmapping/lightmap.png)

### Physics

I implemented 3D-platformer style physics for players, enemies, and game objects. These dynamic entities could interact with other dynamic entities as well as the static level geometry and used capsules (or spheres) for their collision meshes.

![Dynamic entities](/Physics/dynamic_entities.png)

"Movable" convex static meshes were also supported, which could push dynamic entities around. These were intended to be used for objects like opening doors and moving platforms.

![Movable entities](/Physics/movable_entities.png)

### Pathfinding

I used A* pathfinding on top of navmeshes to allow enemies to find their way to players.

![A path passing through a room](/Pathfinding/path_room.png)
![Navmeshes and a path](/Pathfinding/path_scene.png)
![A long path](/Pathfinding/long_path.png)

### Network

To support multiplayer, I wrote a networking system inspired by the famous TRIBES II networking model paper, plus a few of my own customizations. It supported non-guaranteed, guaranteed, and ordered messages as well as automatic object state synchronization. Here are a few images of network diagonstic displays (with artificially inflated ping and packet drop rate for testing purposes).

![Network diagnostics](/Network/network1.png)
![Network diagnostics](/Network/network2.png)

### Editor

To build game content, I built a little GUI-based editor which let you build entities out of components, set up level generation parameters, and manage various other assets.

![Setting up attacks](/Editor/attacks.png)
![Building the lizard enemy](/Editor/entities1.png)
![Building the treasure chest entity](/Editor/entities2.png)
![Setting up player items](/Editor/items.png)
![Specifying map tiles for level generation](/Editor/level_generation.png)
