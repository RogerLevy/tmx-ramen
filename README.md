See ex/extilemap.f and ex/exearwig.f for examples
There are a various ways to use a Tiled map.  Let's break it down by data type.

Tilesets
  A) Load source image and break it up into an array of subbitmaps for tilemap display.
     (There is a global tile table provided in tilegame.f)
     If your tilemap only uses one tileset you could get away with using afsubimg directly,
     but you'd need to write your own tilemap-drawing routine.
  B) Load multiple images (one-image-per-tile) into the global tile array, as allegro bitmaps.
     These bitmaps are then referenced indirectly by GID for display.
  C) Don't load the images at all, just use the paths to instantiate objects sharing
     the same base name.  (This is more convenient than using Tiled's "type" field.)

Tilemaps (called Layers in Tiled)
  A) Reference by name and load into already-declared data structures.
  B) Some kind of dynamic system (an array of data structures referenced by pointer VALUEs)

Object Groups (called Object Layers in Tiled)
  A) You could ignore the separation of layers and just go through all objects instantiating them
     into a single object list.  See Tilesets > C
  B) Reference layers by name and instantiate objects into different object lists.
  C) Some kind of dynamic system
  D) Treat an object group as a "background layer" full of "tile" objects.  See Tilesets > B
  E) Mix and match the above

How scripted game object instantiation works:
  RAMEN has no idea about "classes" so normally object instantiation is pretty open-ended.
  Code that defines the creation of an object is called a recipe.

  Object scripts are made map-editor-agnostic by using conditional compilation, increasing ease of
  portability between projects - which could potentially be made with different map editors (or none at all!!!)

  Here's an example:
  : *myobjectmaker   ( ??? - )  ( objlist )  ONE  ( initialization ) ;
  [defined] :recipe [if]
      :recipe <myobjectname>  ( object-node - )  ... *myobjectmaker ... ;
  [then]

  The body of the :recipe object loader would call your actual recipe.

  .TMX files *MUST* be kept in a folder called data/.  They can be in any subfolder of that.
  Keep your object scripts in data/objects/.
  The object group loader loads them on-demand from there.
  The scripts will each only be loaded once.


Updating scripts in-game
  You can INCLUDE the script you want to update.  If you use the Role system and :recipe , the object's
  behavior and loader will be immediately altered, respectively.

  Because your Forth's license might not permit on-the-fly compilation, you will be able to pre-compile all your
  object scripts before publishing so this is mainly a development feature.

